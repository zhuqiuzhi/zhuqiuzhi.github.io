##
 
Shadowsocks 分为 Sock5 服务器 和 Sock5 客户端

### 安装服务器

* Debian / Ubuntu:

```shell
apt-get install python-pip -y
pip install shadowsocks
```

使用pip 安装 shadowssocks 可能会遇到
```shell
locale.Error: unsupported locale setting
```

解决办法是设置环境变量 LC_ALL 为 en_US.UTF-8, 然后再次安装 shadowsocks

```
echo "export LC_ALL=en_US.UTF-8"  >>  /etc/profile
export LC_ALL=en_US.UTF-8
```

* CentOS:

```shell
yum install python-setuptools && easy_install pip
pip install shadowsocks
```

### 运行服务器

编辑一个 sock.json 文件, 内容如下

```json
{
    "server":"shadowsocks服务端所在服务器公网IP", 
    "server_port": shadowsocks服务端监听端口号, 
    "local_address": "127.0.0.1",
    "local_port": shadowsocks客户端端口号,
    "password":"FuckGFW1024!",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": true,
    "workers": 8
}
```

```
ssserver -c sock.json start -q 2> sock.log &
```

解释：
-c : 指定配置文件地址
-q : 静默模式，只输出错误或者警告信息

### 安装 Go 语言开发的 [Shadowsocks 服务器](https://github.com/shadowsocks/shadowsocks-go)
```shell
apt-get update && apt-get install golang -y
export GOPATH=`pwd`
go get github.com/shadowsocks/shadowsocks-go/cmd/shadowsocks-server
./bin/shadowsocks-server -c sock.json > log &
```