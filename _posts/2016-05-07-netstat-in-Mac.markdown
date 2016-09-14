---
layout: post
title: OS X中的netstat命令使用方法
categories: Tools
---
OS X中的netstat和Linux中的netstat中的使用主要区别在于:
* 使用-p protocol来指定查看通信协议(TCP、UDP)的统计信息。
* -l选项是使得输出使用完整的IPv6地址,而不是只输出listening socket。
...
