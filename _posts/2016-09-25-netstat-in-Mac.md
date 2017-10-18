---
layout: post
title: OS X中的netstat命令使用方法
categories: Tools
---
OS X中的netstat和Linux中的netstat中的使用主要区别在于:
* 使用 -p protocol 来指定查看通信协议(TCP、UDP)的统计信息。
* -l 选项是使得输出使用完整的IPv6地址,而不是只输出listening socket。
* -i 

Linux 输出 Kernel Interface table
```
Kernel Interface table
Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
bond0      1500 0   2471166      0     15 0       2309616      0      2      0 BMmRU
bond0:1    1500 0       - no statistics available -                        BMmRU
em1        1500 0     70386      0     12 0       1139144      0      0      0 BMsRU
em2        1500 0   2400780      0      3 0       1170472      0      0      0 BMsRU
lo        65536 0      3329      0      0 0          3329      0      0      0 LRU
p2p1       1500 0  17681437      0      0 0       2333836      0      0      0 BMRU
p2p2       1500 0       432      0      0 0      15358862      0      0      0 BMRU
tun0       1500 0    105540      0      0 0         55635      0      0      0 MOPRU
tun1       1500 0      1664      0      0 0          1156      0      0      0 MOPRU
```

OS X 输出
```
Name  Mtu   Network       Address            Ipkts Ierrs    Opkts Oerrs  Coll
lo0   16384 <Link#1>                        329056     0   329056     0     0
lo0   16384 127           localhost         329056     -   329056     -     -
lo0   16384 localhost   ::1                 329056     -   329056     -     -
lo0   16384 fe80::1%lo0 fe80:1::1           329056     -   329056     -     -
gif0* 1280  <Link#2>                             0     0        0     0     0
stf0* 1280  <Link#3>                             0     0        0     0     0
en0   1500  <Link#4>    f4:0f:24:28:e0:31 61759225     0 64061666     0     0
en0   1500  fe80::c37:1 fe80:4::c37:1b9b: 61759225     - 64061666     -     -
en0   1500  192.168.0     192.168.0.101   61759225     - 64061666     -     -
en1   1500  <Link#5>    26:00:48:10:b9:00        0     0        0     0     0
en2   1500  <Link#6>    26:00:48:10:b9:01        0     0        0     0     0
bridg 1500  <Link#7>    26:00:48:10:b9:00        0     0        0     0     0
p2p0  2304  <Link#8>    06:0f:24:28:e0:31        0     0        0     0     0
awdl0 1484  <Link#9>    e6:ee:2b:1f:a2:52    13336     0     4369     0     0
awdl0 1484  fe80::e4ee: fe80:9::e4ee:2bff    13336     -     4369     -     -
utun0 2000  <Link#10>                            0     0        3     0     0
utun0 2000  fe80::9a3e: fe80:a::9a3e:8be:        0     -        3     -     -
utun1 1500  <Link#11>                          369     0      650     0     0
utun1 1500  172.21.0.54/3 172.21.0.54          369     -      650     -     -
```
...
