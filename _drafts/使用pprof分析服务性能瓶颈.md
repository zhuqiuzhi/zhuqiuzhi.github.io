---
layout: post
title: 使用pprof分析服务性能瓶颈
---

## 编译相应平台的二进制 pprof
由于机器上可能没有安装go，所以编译一个单独使用的二进制 pprof
以下是编译 Linux 的 pprof:

```shell
cd $GOROOT/src/cmd/pprof
GOOS=linux go build
```

## 收集性能数据 

go tool pprof 最简单的使用方式为 go tool pprof [binary] [source]，binary 是应用的二进制文件，用来解析各种符号；source 表示 profile 数据的来源，可以是本地的文件，也可以是 http 地址。

当没有指定二进制时，从数据源收集30s的数据,保存在当前目录下: pprof/pprof.二进制名称.ip:port.samples.cpu.002.pb.gz
linuxpprof http://127.0.0.1:48437/debug/pprof/profile

## 使用 go-torch

```shell
go get github.com/uber/go-torch
```

```shell
git clone https://github.com/brendangregg/FlameGraph.git
```
将FlameGraph中的*.pl 复制到某个在PATHd的目录下

```shell
go-torch bin pprof.pb.gz
```
