---
layout: post
title: "使用cscope和ctags阅读C语言项目"
categories: Read-Code
---

## 简介
cscope是一个交互式的用于追踪代码的代码浏览器。
ctags是一个用于从程序源代码树产生tags，从而便于文本编辑器来实现快速定位的实用工具。
它会生成tags文件，能和vim等编辑器集成在一起，帮助我们快速定位对象。

## cscope功能
- 查找一个符号
- 查找一个全局定义
- 查找被这个函数所调用的所有函数
- 查找这个函数所调用的其它函数
- 查找字符串
- 更改字符串
- 搜索正则表达式
- 查找文件
- 查找包含了这个文件的所有文件
- 查找对该符号的赋值

## 安装cscope
  在Debain中使用apt-get安装cscope,在RHEL中使用yum安装cscope:

```bash
sudo apt-get install cscope
```

```bash
sudo yum install cscope

  OS X:
```bash
sudo brew install cscope
```

##  使用cscope的前期准备
在使用cscope之前，需要生成几个文件来帮助cscope快速定位符号。这几个文件的介绍可以通过`man cscope`
查看帮助手册中FILE节中的介绍。
1. 生成cscope.files
cscope.files其实就是一个纯文本的源文件列表，cscope根据cscope.files来决定扫描哪些文件
C 项目:

	```bash
	find . -name '*.[chly] ' > cscope.files
	```
Python 项目:   

	```bash
	find . -name '*.py’  > cscope.files   
	```
	注意*.py一定要加引号。

2. 生成符号交叉引用(symbol cross-reference)和倒排索引(inverted index)

```bash
cscope -qb
```

> 选项说明:
> - \-b : 只生成symbol cross-reference 即cscope.out
> - \-q: 除了生成cscope.out,还生成inverted index 即cscope.in.out和cscope.po.out
> - \-k:  kernel mode , 不使用/usr/include,这个选项只有在该项目是Linux kernel时才使用。

linux内核代码一步搞定:`make cscope`

## 启动cscope
`cscope`   : 如果文件有改动或者文件列表有变化，则重新生成cscope.out
`cscope -d`  ：不重新生成cscope.out

## cscope 功能使用
- tab：  在匹配的结果和命令列表中切换
- up:    移动到上一个命令
- down： 移动到下一个命令
- <+>:     当匹配的结果有很多时，翻到前一页
- <->：    翻到下一页
- <ctrl+d>：退出cscope

## ctags生成tags
`ctags -R`

## 配合vim使用ctags
`ctrl ]` ：跳转到该符号的定义
`ctrl t` :返回

## 问题：
1. Linux kernel 有很多同名函数，在用ctags跳转时，怎么选择？
答：它也无能为力了，遇到这种情况，你可以先搜索一下sysmblos来找到你想要找的函数。
2. cscope除了支持C语言，还支持哪些语言？
答：还比较好的支持C++、Java。pycscope支持python。
