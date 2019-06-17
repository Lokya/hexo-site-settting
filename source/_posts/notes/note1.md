---
title: too many open files
categories: linux
tags:
    - linux
    - note
---

## 原因

前端Jenkins跑服务时候报错，怎么都跑不起来，我就去服务器手动执行脚本。在`build`阶段发现服务器报错 `too many open files`,于是去查找资料找解决方案。

这个报错是只打来文件过多，超过了系统限制的文件数量以及通讯链接数，这边的`files`不单是文件的意思，也包括打开的通讯链接(比如socket)，正在监听的端口等，也可以叫做句柄(handle)。这个错误通常也可以叫做句柄数超出系统限制。 

## 查看

可以使用 ulimit -a 或者 ulimit -n 来查看当前系统最大句柄限制数

```bash
$ ulimit -a

core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 127922
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 127922
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited

$ limit -n

1024

```
open files那一行就代表系统目前允许单个进程打开的最大句柄数，这里是1024。 

同时使用 `-n` 查看下来也是1024。

## 修改

### 临时修改

**ulimit -n**


```bash
$ ulimit -n 65535

$ ulimit -a

core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 127922
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 65535
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 127922
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited

$ limit -n

65535
```

但这种设置方法在重启后会**还原为默认值**。

`ulimit -n`命令非root用户只能设置到4096，想要设置到更大需要sudo权限或者root用户。

### 修改conf文件，永久配置

```bash
$ vim /etc/security/limits.conf  
#在最后加入  
* soft nofile 65535  
* hard nofile 65535 
```

`*`代表所有用户，可以给特点用户设定句柄最大数值。

```bash
$ vim /etc/security/limits.conf  
#在最后加入  
jenkins soft nofile 65535  
jenkins hard nofile 65535
```

上面就是把jenkins用户最大句柄数改成65535

之后再查看大小的话可以发现已经修改为 65535 。
