---
date: 2025-11-21T23:46:48+08:00
draft: false
title: 代码片段
---

VSCode的代码片段（Snippets），是指预设好代码，通过简短提示，快速生成大量代码的功能。例如，最典型的错误判定代码：

```go
if err != nil {
    return err
}

if err == nil {
    // your code
}
```

编写Go代码时，几乎每个函数中都要出现好几次。代码片段的目的就是让这种常用代码快速实现。键入`iferr`后`<Tab>`：

```go
// iferr<Tab>
if err != nil {
	return nil, err
}

```



### 内置Go语言Snippets

稍微注意下，是VSCode的**Go扩展**（Go for Visual Studio Code）内置的，不是VSCode内置的：

```json {filename="$HOME\.vscode\extensions\golang.go-<version>\snippets\go.json"}
{
	".source.go": {
        "single import": {
            "prefix": "im", "body": "import \"${1:package}\""
        },
        "multiple imports": {
            "prefix": "ims", "body": "import (\n\t\"${1:package}\"\n)"
        },
        "single constant": {
            "prefix": "co", "body": "const ${1:name} = ${2:value}"
        },
        "multiple constants": {
            "prefix": "cos", "body": "const (\n\t${1:name} = ${2:value}\n)"
        },
        "type function declaration": {
            "prefix": "tyf","body": "type ${1:name} func($3) $4"
        },
        "type interface declaration": {
            "prefix": "tyi", "body": "type ${1:name} interface {\n\t$0\n}"
        },
        "type struct declaration": {
            "prefix": "tys", "body": "type ${1:name} struct {\n\t$0\n}"
        },
        "package main and main function": {
            "prefix": "pkgm", "body": "package main\n\nfunc main() {\n\t$0\n}"
        },
        "function declaration": {
            "prefix": "func", "body": "func $1($2) $3 {\n\t$0\n}"
        },
        "single variable": {
            "prefix": "var", "body": "var ${1:name} ${2:type}"
        },
        "multiple variables": {
            "prefix": "vars", "body": "var (\n\t${1:name} ${2:type}\n)"
        },
        "switch statement": {
            "prefix": "switch", "body": "switch ${1:expression} {\ncase ${2:condition}:\n\t$0\n}"
        },
        "select statement": {
            "prefix": "sel", "body": "select {\ncase ${1:condition}:\n\t$0\n}"
        },
        "case clause": {
            "prefix": "cs", "body": "case ${1:condition}:$0"
        },
        "for statement": {
            "prefix": "for", "body": "for ${1:i} := ${2:0}; $1 < ${3:count}; $1${4:++} {\n\t$0\n}"
        },
        "for range statement": {
            "prefix": "forr", "body": "for ${1:_}, ${2:v} := range ${3:v} {\n\t$0\n}"
        },
        "channel declaration": {
            "prefix": "ch", "body": "chan ${1:type}"
        },
        "map declaration": {
            "prefix": "map", "body": "map[${1:type}]${2:type}"
        },
        "empty interface": {
            "prefix": "in", "body": "interface{}"
        },
        "if statement": {
            "prefix": "if", "body": "if ${1:condition} {\n\t$0\n}"
        },
        "else branch": {
            "prefix": "el", "body": "else {\n\t$0\n}"
        },
        "if else statement": {
            "prefix": "ie", "body": "if ${1:condition} {\n\t$2\n} else {\n\t$0\n}"
        },
        "if err != nil": {
            "prefix": "iferr", "body": "if err != nil {\n\t${1:return ${2:nil, }${3:err}}\n}"
        },
        "fmt.Println": {
            "prefix": "fp", "body": "fmt.Println(\"$1\")"
        },
        "fmt.Printf": {
            "prefix": "ff", "body": "fmt.Printf(\"$1\", ${2:var})"
        },
        "log.Println": {
            "prefix": "lp", "body": "log.Println(\"$1\")"
        },
        "log.Printf": {
            "prefix": "lf", "body": "log.Printf(\"$1\", ${2:var})"
        },
        "log variable content": {
            "prefix": "lv", "body": "log.Printf(\"${1:var}: %#+v\\\\n\", ${1:var})"
        },
        "t.Log": {
            "prefix": "tl", "body": "t.Log(\"$1\")"
        },
        "t.Logf": {
            "prefix": "tlf", "body": "t.Logf(\"$1\", ${2:var})"
        },
        "t.Logf variable content": {
            "prefix": "tlv", "body": "t.Logf(\"${1:var}: %#+v\\\\n\", ${1:var})"
        },
        "make(...)": {
            "prefix": "make", "body": "make(${1:type}, ${2:0})"
        },
        "new(...)": {
            "prefix": "new", "body": "new(${1:type})"
        },
        "panic(...)": {
            "prefix": "pn", "body": "panic(\"$0\")",
            "description": "Snippet for panic"
        },
        "http ResponseWriter *Request": {
            "prefix": "wr", "body": "${1:w} http.ResponseWriter, ${2:r} *http.Request"
        },
        "http.HandleFunc": {
            "prefix": "hf", "body": "${1:http}.HandleFunc(\"${2:/}\", ${3:handler})"
        },
        "http handler declaration": {
            "prefix": "hand", "body": "func $1(${2:w} http.ResponseWriter, ${3:r} *http.Request) {\n\t$0\n}"
        },
        "http.Redirect": {
            "prefix": "rd", "body": "http.Redirect(${1:w}, ${2:r}, \"${3:/}\", ${4:http.StatusFound})"
        },
        "http.Error": {
            "prefix": "herr", "body": "http.Error(${1:w}, ${2:err}.Error(), ${3:http.StatusInternalServerError})"
        },
        "http.ListenAndServe": {
            "prefix": "las", "body": "http.ListenAndServe(\"${1::8080}\", ${2:nil})"
        },
        "http.Serve": {
            "prefix": "sv", "body": "http.Serve(\"${1::8080}\", ${2:nil})"
        },
        "goroutine anonymous function": {
            "prefix": "go", "body": "go func($1) {\n\t$0\n}($2)"
        },
        "goroutine function": {
            "prefix": "gf", "body": "go ${1:func}($0)"
        },
        "defer statement": {
            "prefix": "df", "body": "defer ${1:func}($0)"
        },
        "test function": {
            "prefix": "tf", "body": "func Test$1(t *testing.T) {\n\t$0\n}"
        },
        "test main": {
            "prefix": "tm", "body": "func TestMain(m *testing.M) {\n\t$1\n\n\tos.Exit(m.Run())\n}"
        },
        "benchmark function": {
            "prefix": "bf", "body": "func Benchmark$1(b *testing.B) {\n\tfor ${2:i} := 0; ${2:i} < b.N; ${2:i}++ {\n\t\t$0\n\t}\n}"
        },
        "example function": {
            "prefix": "ef", "body": "func Example$1() {\n\t$2\n\t//Output:\n\t$3\n}"
        },
        "table driven test": {
            "prefix": "tdt", "body": "func Test$1(t *testing.T) {\n\ttestCases := []struct {\n\t\tdesc\tstring\n\t\t$2\n\t}{\n\t\t{\n\t\t\tdesc: \"$3\",\n\t\t\t$4\n\t\t},\n\t}\n\tfor _, tC := range testCases {\n\t\tt.Run(tC.desc, func(t *testing.T) {\n\t\t\t$0\n\t\t})\n\t}\n}"
        },
        "init function": {
            "prefix": "finit", "body": "func init() {\n\t$1\n}"
        },
        "main function": {
            "prefix": "fmain", "body": "func main() {\n\t$1\n}"
        },
        "method declaration": {
            "prefix": "meth", "body": "func (${1:receiver} ${2:type}) ${3:method}($4) $5 {\n\t$0\n}"
        },
        "hello world web app": {
            "prefix": "helloweb", "body": "package main\n\nimport (\n\t\"fmt\"\n\t\"net/http\"\n\t\"time\"\n)\n\nfunc greet(w http.ResponseWriter, r *http.Request) {\n\tfmt.Fprintf(w, \"Hello World! %s\", time.Now())\n}\n\nfunc main() {\n\thttp.HandleFunc(\"/\", greet)\n\thttp.ListenAndServe(\":8080\", nil)\n}"
        },
        "sort implementation": {
            "prefix": "sort", "body": "type ${1:SortBy} []${2:Type}\n\nfunc (a $1) Len() int           { return len(a) }\nfunc (a $1) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }\nfunc (a $1) Less(i, j int) bool { ${3:return a[i] < a[j]} }"
        }
    }
}
```

配置中：

- prefix，输入的前缀
- body，生成的代码片段
- Description，描述，为了空间，我删了。

### 自定义Snippets

除了扩展预定义的，也可以自定义，VSCode菜单：

```go
File > Preferences > Configure Snippets
// 文件 > 首选项 > 配置代码片段
```

可以为当前项目和Go全局创建自定义的Snippets配置文件：

![image-20251124134020446](image-20251124134020446.png)

选择后，自动生成配置文件：

```json {filename="go.json"}
{
	// Place your snippets for go here. Each snippet is defined under a snippet name and has a prefix, body and 
	// description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
	// same ids are connected.
	// Example:
	// "Print to console": {
	// 	"prefix": "log",
	// 	"body": [
	// 		"console.log('$1');",
	// 		"$2"
	// 	],
	// 	"description": "Log output to console"
	// }
}
```

依据示例，添加即可。其中：

- $1, $2用于制表符位置，$0表示光标的最终位置
- ${1:label}, ${2:another}表示占位符

示例：

```json
{
    "if err == nil": {
        "prefix": "ifne",
        "body": [
            "if err == nil {",
            "\t// your code here",
            "\t${1:return ${2:nil}}",
            "}"
        ],
        "description": "Snippet for if err != nil"
    }
}
```

使用：

```go
// ifne<tab>
if err == nil {
	// your code here
	return nil
}
```

