## HTTP server


```go
type route struct {
	pattern Pattern
	handler http.Handler
}

type ServeMux struct {
	routes []*route
	base   http.Handler
}

var DefaultServeMux = NewServeMux()

func NewServeMux() *ServeMux {

	return new(ServeMux)
}

func (h *ServeMux) ServeHTTP(w http.ResponseWriter, r *http.Request) {

	parts := strings.Split(r.URL.Path[1:], "/")

	for _, route := range h.routes {
		if args, ok := route.pattern.Match(r.Method, parts); ok {
			r.Header["*"] = args
			route.handler.ServeHTTP(w, r)
			return
		}
	}

	if h.base != nil {
		h.base.ServeHTTP(w, r)
	} else {
		http.NotFound(w, r)
	}
}
```
```go
// serverHandler delegates to either the server's Handler or
// DefaultServeMux and also handles "OPTIONS *" requests.
type serverHandler struct {
	srv *Server // http.Server
}

func (sh serverHandler) ServeHTTP(rw ResponseWriter, req *Request) {
	handler := sh.srv.Handler
	if handler == nil {
		handler = DefaultServeMux
	}
	if req.RequestURI == "*" && req.Method == "OPTIONS" {
		handler = globalOptionsHandler{}
	}
	handler.ServeHTTP(rw, req)
}
```


```go
func (srv *Server) ListenAndServe() error {
	addr := srv.Addr
	if addr == "" {
		addr = ":http"
	}
	ln, err := net.Listen("tcp", addr)
	if err != nil {
		return err
	}
	return srv.Serve(tcpKeepAliveListener{ln.(*net.TCPListener)})
}

// 接收一个连接，起一个goroutine 调用
func (srv *Server) Serve(l net.Listener) error {
	for () {
		rw, e := l.Accept()
		if e != nil {...}
		c := srv.newConn(rw) // 会将 svr 保存在 c.server
		c.setState(c.rwc, StateNew) // before Serve can return
		go c.serve(ctx) // for 循环中执行serverHandler{c.server}.ServeHTTP(w, w.req)
	}
}

// Create new connection from rwc.
func (srv *Server) newConn(rwc net.Conn) *conn {
	c := &conn{
		server: srv,
		rwc:    rwc,
	}
	if debugServerConnections {
		c.rwc = newLoggingConn("server", c.rwc)
	}
	return c
}
```

```go
func (c *conn) serve(ctx context.Context) {
	c.r = &connReader{conn: c}
	c.bufr = newBufioReader(c.r)
	c.bufw = newBufioWriterSize(checkConnErrorWriter{c}, 4<<10)

	for {
		w, err := c.readRequest(ctx)
		...
		serverHandler{c.server}.ServeHTTP(w, w.req)
	}
}
```

## 总结：

```go
package http

func ListenAndServe(addr string, handler Handler) error {
	server := &Server{Addr: addr, Handler: handler}
	return server.ListenAndServe()
	// 监听端口
	// handler != nil 使用 server.Handler.ServerHTTP(rw ResponseWriter, req *Request) 并发处理请求
	// handler == nil 使用 DefaultServeMux.ServeHTTP(rw ResponseWriter, req *Request)   并发处理请求
}
```
