### 利用打包资源文件

> fsman.go

```go
package main

import (
    "embed"
    "io/fs"
    "log"
    "net/http"
    "os"
)


func main() {
    useOS := len(os.Args) > 1 && os.Args[1] == "live"
    http.Handle("/", http.FileServer(getFileSystem(useOS)))
    http.ListenAndServe(":8888", nil)
}

//go:embed static
var embededFiles embed.FS

func getFileSystem(useOS bool) http.FileSystem {
    if useOS {
        log.Print("using live mode")
        return http.FS(os.DirFS("static"))
    }

    log.Print("using embed mode")

    fsys, err := fs.Sub(embededFiles, "static")
    if err != nil {
        panic(err)
    }
    return http.FS(fsys)
}

```

```shell
[root@localhost golang1111]# find
.
./fsman.go
./static
./static/1.html
./static/abc
./static/abc/2.html
[root@localhost golang1111]#
[root@localhost golang1111]# go build fsman.go
[root@localhost golang1111]# ls
fsman  fsman.go  static
[root@localhost golang1111]# ./fsman &
[1] 1394974
[root@localhost golang1111]# 2022/01/18 23:48:34 using embed mode
[root@localhost golang1111]#
[root@localhost golang1111]# curl http://127.0.0.1:8888/1.html
fdsafdsaf
static/1.htm
[root@localhost golang1111]# curl http://127.0.0.1:8888/
<pre>
<a href="1.html">1.html</a>
<a href="abc/">abc/</a>
</pre>
[root@localhost golang1111]#

```

