---
title:fmt包里的接口：Stringer
---

## 接口 Stringer

fmt 包中定义的 Stringer 是最普遍的接口之一。

```go
type Stringer interface {
    String() string
}
```

Stringer 是一个可以用字符串描述自己的类型。fmt 包（还有很多包）都通过此接口来打印值。
通过让 IPAddr 类型实现 fmt.Stringer 来打印点号分隔的地址。
例如，IPAddr{1, 2, 3, 4} 应当打印为 "1.2.3.4" 。

## 实现

```go
package main

import "fmt"

type IPAddr [4]byte

func (i IPAddr) String() string {
	return fmt.Sprintf("%d.%d.%d.%d",i[0],i[1],i[2],i[3])
}

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}
```

## 运行结果

```shell
loopback: 127.0.0.1
googleDNS: 8.8.8.8
```
