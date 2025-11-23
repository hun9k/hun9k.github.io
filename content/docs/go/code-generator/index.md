+++

title = '生成Go代码'

+++

代码生成的主要思路：

- 直接基于代码模板生成，`text/template`
- 基于已有结构生成，`go/ast` + `text/template`
- 代码格式化

## 基于代码模板生成

通常来说，最简单的生成代码，就是那种固定的代码，例如，生成某个资源的Rest路由，例如：

```go
router := gin.Default()
group := router.Group("v1").Group("articles")
group.POST("", articles.Post)                  // 创建一个资源
group.DELETE(":id", articles.DeleteId)         // 删除一个资源
group.PUT(":id", articles.PutId)               // 更新一个资源
group.GET(":id", articles.GetId)               // 获取一个资源
group.GET("", articles.Get)                    // 获取多个资源
```

代码中，只有articles资源名称部分是不同的，因此我们仅需要更改articles部分即可。

那么就可以创建一个模板文件，使用Go标准库 text/template：

```go
tmplRouter = `router := gin.Default()
group := router.Group("{{.RouterVersion}}").Group("{{.ResourceName}}")
group.POST("", articles.Post)                  // 创建一个资源
group.DELETE(":id", articles.DeleteId)         // 删除一个资源
group.PUT(":id", articles.PutId)               // 更新一个资源
group.GET(":id", articles.GetId)               // 获取一个资源
group.GET("", articles.Get)                    // 获取多个资源
`
```

利用这个模板，替换必要的版本和资源名部分，就可以生成Go代码，后边写入到`articles.go`文件即可。



## 基于已有结构生成

有的时候，我们需要根据以后的代码结构，来生成。例如，基于GORM的模型定义，生成相关的业务逻辑代码，此时往往需要知道模型中有哪些字段，然后根据字段类型或其他信息，分别处理，例如：

```go
// schemas/articles.go

type Articles struct {
	// 自定义字段
	Subject     string    `gorm:"" json:"subject"`
	Summary     string    `gorm:"" json:"summary"`
	Content     string    `gorm:"" json:"content"`
	Likes       int       `gorm:"" json:"likes"`
	IsPublished bool      `gorm:"" json:"is_published"`
	Birthday    time.Time `gorm:"" json:"birthday"`

	// 嵌入基础字段，ID，CreatedAt，UpdatedAt, DeletedAt
	gorm.Model
}
```

以上模型是我们手动定义的，接下来个根据这个模型定义生成基础的`CRUD`代码。

第一步，就需要知道到底有哪些字段。这就属于动态的结构，因为我们不知道，到底有哪些字段。

那如何才能知道这个模型的结构呢？就需要Go源码分析工具。Go标准库的**`go/ast`**及相关的包，可以完成Go源码的分析任务，相关的包：

- `go/parser`，包解析器实现了Go源文件的解析器
- `go/ast`，包声明用于表示Go包的语法树的类型
- `go/token`，包义了表示Go语言的词法token，以及对token的基本操作的常量
- `go/types`，包声明数据类型并实现Go包的类型检查算法
- `go/format`，包实现了Go源代码的标准格式
- `go/doc`，包从`AST`中提取源代码文档

当然还有其他包，可以从https://pkg.go.dev/go 中查看。

快速演示，获取模型定义中，有哪些字段：

```go
// 解析模型结构.go文件
fset := token.NewFileSet()
filename := filepath.Join("schemas/articles.go")
file, err := parser.ParseFile(fset, filename, nil, parser.ParseComments)
if err != nil {
    return nil, err
}

// 找到模型定义的结构体
var schemaStruct *ast.StructType
ast.Inspect(file, func(n ast.Node) bool {
    switch x := n.(type) {
    case *ast.TypeSpec:
        schemaStruct = x.Type.(*ast.StructType)
        return false
    }
    return true
})
if schemaStruct == nil {
    return nil, errors.New("schema struct not found")
}

// 遍历全部字段
for _, field := range schemaStruct.Fields.List {
    switch t := field.Type.(type) {
    case *ast.Ident: // 常规类型，例如 string, int, bool
        fmt.Println(t.Name)
    case *ast.SelectorExpr: // 带有选择器的类型，例如 time.Time, gorm.Model
        fmt.Println(t.X.(*ast.Ident).Name, t.Sel.Name)
    }
}
```

示例代码到此，就可以确定有哪些需要处理的字段。进而就可以根据字段，做后续的代码生成了。后续的工作，就变为了基于代码模板生成代码了。



## `go/ast`与 `reflect`的差别

多说一句，要分清`go/ast`抽象语法树与`reflect`反射的差别：

-  `go/ast`抽象语法树，基于Go源码（字符串）获取信息
-  `reflect`反射，基于Go程序（内存结构）获取信息

因此，代码生成通常基于Go源码来生成，使用`go/ast`相关包多些。