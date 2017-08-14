# Go 1.7 的一个小问题

  当用方法 Post请求一个URL，返回状态码为 303, Location 为 http://www.qiniu.com,Golang HTTP 客户端会自动 Get http://www.qiniu.com
 而当这个请求返回状态码 301,Location 为 https 的网站时，如 https://www.qiniu.com 时，Golang 1.7 不会再 Get 这个 Location, 但 Golang 1.8 的 HTTP 客户端会继续 Get 这个Location.    

  以下是我向Go 提交的issue，但Go 维护者认为 Go 1.8 已经修复了这个问题，不再继续维护 Go 1.7，就把我这个 issue 关掉了。这个倒挺出乎我的意料之外的。我以为 Go 维护者们会继续维护旧的版本。[issue 链接](https://github.com/golang/go/issues/21426)

### What version of Go are you using (`go version`)?
go version go1.7.6 darwin/amd64
go version go1.6.4 darwin/amd64

### What operating system and processor architecture are you using (`go env`)?
GOARCH="amd64"
GOBIN=""
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/zhuqiuzhi/work"
GORACE=""
GOROOT="/Users/zhuqiuzhi/.gvm/gos/go1.7.6"
GOTOOLDIR="/Users/zhuqiuzhi/.gvm/gos/go1.7.6/pkg/tool/darwin_amd64"
CC="clang"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/dc/q_v8vj2957s1k0krncnl26q40000gn/T/go-build403359302=/tmp/go-build -gno-record-gcc-switches -fno-common"
CXX="clang++"
CGO_ENABLED="1"

GOARCH="amd64"
GOBIN=""
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/zhuqiuzhi/work"
GORACE=""
GOROOT="/Users/zhuqiuzhi/.gvm/gos/go1.6.4"
GOTOOLDIR="/Users/zhuqiuzhi/.gvm/gos/go1.6.4/pkg/tool/darwin_amd64"
GO15VENDOREXPERIMENT="1"
CC="clang"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fno-common"
CXX="clang++"
CGO_ENABLED="1"
### What did you do?

```go
package main

import (
	"net/http"
	"net/http/httptest"
	"fmt"
)

func main() {
	ts := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Location", "http://www.qiniu.com/")
		w.WriteHeader(http.StatusSeeOther)
	}))
	defer ts.Close()

	resp, err := http.Post(ts.URL,"multipart/form-data", nil)
	if err != nil {
		fmt.Printf("Post fail, ", err)
	}
	fmt.Println(resp.StatusCode)
	if resp.StatusCode != http.StatusOK {
		fmt.Printf("%v\n", resp.Header)
		fmt.Printf("Fail follow 301, https\n")
	} else {
		fmt.Println("OK")
	}
}
```

### What did you expect to see?
```
200
OK
```

### What did you see instead?
```
301
map[Connection:[keep-alive] Date:[Sun, 13 Aug 2017 12:33:51 GMT] X-Swift-Savetime:[Sun, 13 Aug 2017 12:33:51 GMT] Timing-Allow-Origin:[*] Eagleid:[7529f0cc15026276318052012e] Server:[Tengine] Content-Type:[text/html] Content-Length:[178] Location:[https://www.qiniu.com/] Via:[cache17.l2em21-1[31,301-0,M], cache8.l2em21-1[32,0], cache2.cn26[46,301-0,M], cache4.cn26[47,0]] X-Cache:[MISS TCP_MISS dirn:-2:-2] X-Swift-Cachetime:[0]]
Fail follow 301, https
```
