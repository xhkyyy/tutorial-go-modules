
> module name: example.com/hello

> Both `go.mod` and `go.sum` should be checked into version control.

> https://blog.golang.org/using-go-modules


```sh
⚠️

# download and install packages and dependencies


go get ${packages} 
```



```sh
go test
```



```sh
go list -m -versions ${module name}

# 搜索 rsc.io/q 前缀的 module
go list -m rsc.io/q...
```



```
go doc ${module name}
```



```sh
👍 # Removing unused dependencies
go mod tidy
```



```sh
go get ${module name}@${module version} # [1]

go get rsc.io/sampler@v1.3.1

# [1] In general each argument passed to go get can take an explicit version; the default is @latest.
```



```sh
# adding one direct dependency often brings in other indirect dependencies too.
# this command lists the current module and all its dependencies
go list -m all

# ---------------------------------------------------
example.com/hello 																			# [1]
golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c		# [2]
rsc.io/quote v1.5.2
rsc.io/sampler v1.3.0
# ---------------------------------------------------

# [1] the current module, also known as the main module, is always the first line
# [2] version v0.0.0-20170915032832-14c0d48ead0c is an example of a pseudo-version
```



#### file 'go.mod'

go.mod - 1

```go
module example.com/hello

go 1.12

require rsc.io/quote v1.5.2
```



go.mod - 2

```go
module example.com/hello

go 1.12

require (
	golang.org/x/text v0.3.0 // indirect [1]
	rsc.io/quote v1.5.2
)

// [1] The indirect comment indicates a dependency is not used directly by this module, only indirectly by other module dependencies. （indirect 表示这个包不是当前这个模块直接引用的，而是被其它依赖间接引用的）
```



#### file 'go.sum'

Containing the [cryptographic hashes](https://golang.org/cmd/go/#hdr-Module_downloading_and_verification) of the content of each module versions.

```sh
# file: go.sum

golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c h1:qgOY6WgZOaTkIIMiVjBQcw93ERBE4m30iBm00nkL0i8=
golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c/go.mod h1:NqM8EUOU14njkJ3fqMW+pc6Ldnwhi/IjpwHt7yyuwOQ=
rsc.io/quote v1.5.2 h1:w5fcysjrx7yqtD/aO+QwRjYZOKnaM9Uh2b40tElTs3Y=
rsc.io/quote v1.5.2/go.mod h1:LzX7hefJvL54yjefDEDHNONDjII0t9xZLPXsUe+TKr0=
rsc.io/sampler v1.3.0 h1:7uVkIFmeBqHfdjD+gZwtXXI+RODJ2Wc4O7MPEh/QiW4=
rsc.io/sampler v1.3.0/go.mod h1:T1hPZKmBbMNahiBKFy5HrXp6adAjACjK9JXDnKaTXpA=
```

The `go` command uses the `go.sum` file to ensure that future downloads of these modules retrieve the same bits as the first download, to ensure the modules your project depends on do not change unexpectedly, whether for malicious, accidental, or other reasons. Both `go.mod` and `go.sum` should be checked into version control.



## 使用 Go module 流程

+ `go mod init` creates a new module, initializing the `go.mod` file that describes it.
+ `go build`, `go test`, and other package-building commands add new dependencies to `go.mod` as needed.
+ `go list -m all` prints the current module’s dependencies.
+ `go get` changes the required version of a dependency (or adds a new dependency).
+ `go mod tidy` removes unused dependencies.



### 其它

```
Each different major version (v1, v2, and so on) of a Go module uses a different module path: starting at v2, the path must end in the major version. In the example, v3 of rsc.io/quote is no longer rsc.io/quote: instead, it is identified by the module path rsc.io/quote/v3.
```

