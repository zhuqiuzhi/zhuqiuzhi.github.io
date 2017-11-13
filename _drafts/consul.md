# Consul - 分布式服务发现

# 编译二进制

克隆仓库(git@github.com:hashicorp/consul.git) 到 GOPATH/src/github.com 中,然后设置环境变量 CONSUL_DEV=1，使得make 只编译你机器操作系统的二进制

```shell
cd work/src/github.com
git clone git@github.com:hashicorp/consul.git
cd consul
CONSUL_DEV=1 make
```

# 启动

```shell
# 启动 server
consul agent -server -bind=0.0.0.0 --datacenter=nb

# 启动 agent，并join 到集群
consul agent -bind=0.0.0.0 --datacenter=nb -join
```

