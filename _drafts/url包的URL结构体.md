```go
// A URL represents a parsed URL (technically, a URI reference).
// The general form represented is:
//
//	scheme://[userinfo@]host/path[?query][#fragment]
//
// URLs that do not start with a slash after the scheme are interpreted as:
//
//	scheme:opaque[?query][#fragment]
//
// Note that the Path field is stored in decoded form: /%47%6f%2f becomes /Go/.
// A consequence is that it is impossible to tell which slashes in the Path were
// slashes in the raw URL and which were %2f. This distinction is rarely important,
// but when it is, code must not use Path directly.
//
// Go 1.5 introduced the RawPath field to hold the encoded form of Path.
// The Parse function sets both Path and RawPath in the URL it returns,
// and URL's String method uses RawPath if it is a valid encoding of Path,
// by calling the EscapedPath method.
//
// In earlier versions of Go, the more indirect workarounds were that an
// HTTP server could consult req.RequestURI and an HTTP client could
// construct a URL struct directly and set the Opaque field instead of Path.
// These still work as well.
type URL struct {
	Scheme     string
	Opaque     string    // encoded opaque data
	User       *Userinfo // username and password information
	Host       string    // host or host:port
	Path       string
	RawPath    string // encoded path hint (Go 1.5 and later only; see EscapedPath method)
	ForceQuery bool   // append a query ('?') even if RawQuery is empty
	RawQuery   string // encoded query values, without '?'
	Fragment   string // fragment for references, without '#'
}
```
