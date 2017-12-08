一、安装两个工具
go get -u golang.org/x/tools/cmd/go-contrib-init
go get golang.org/x/review/git-codereview

二、克隆一个仓库
git clone --depth 1  https://go.googlesource.com/go ~/golang

三、 在 仓库 golang 中 运行 go-contrib-init -dry-run
它可能要求你做：
1. 配置git 来使用 Gerrit (Golang 社区用来 review 代码)
在 https://go.googlesource.com/ 上登录 Google 账户，然后点击右上角的 Generate Password，然后在终端运行它提供的脚本
2. 用上面使用的 Google 账户登陆Gerrit ， https://go-review.googlesource.com/login/， 
https://go-review.googlesource.com/settings/#Agreements
设置你的 CLA （ Contributor License Agreement）

然后它会给 git 添加一条命令 codereview ,并在.gitconfig 中添加一些别名
[alias]
	change = codereview change
	gofmt = codereview gofmt
	mail = codereview mail
	pending = codereview pending
	submit = codereview submit
	sync = codereview sync
