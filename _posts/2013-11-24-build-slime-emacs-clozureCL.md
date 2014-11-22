---
layout: post
title: 使用最新版emacs+slime+CCL建立Common Lisp IDE
category: blog
description: lispbox现在已经不更新了，使用的是老版本的emacs、slime和quicklisp
---

## 介绍

[Lispbox][0]安装简易，是新手学习Lisp无痛起步的不二之选。但现在没人维护了，其中使用的emacs，slime，ccl和quicklisp等都是比较老的版本。本文介绍如何使用最新的上述工具搭建Common Lisp的IDE。

### 准备

- CentOS 6.5 64bit / fedora 20 64bit
- Emacs 24.3.1 64bit 
- 下载[Clozure CL][1]这个是Common Lisp的一个优秀实现，大小约42M，CentOS系统选择Linux的x86即可，其中包含了32和64的版本，作者使用的是1.10发行版

## 步骤

### 安装ccl

- 解压下载的ccl，将其中scripts目录下的ccl（32位）或者是ccl64（64位）复制到/usr/local/bin或者是其他的PATH变量包含的路径中并且重命名为ccl——不重命名也可，只要与最终编辑.emacs环节中的"(setq inferior-lisp-program "/usr/local/bin/ccl64 -K utf-8")"对应即可。
- 在自己的~中的.bashrc或者.bash_profile中添加变量 CCL\_DEFAULT\_DIRECTORY 为解压的ccl的目录的位置，比如/usr/local/src/ccl，如果不配置这个变量的话，则把解压的目录放到/usr/local/src/ccl中即可，因为不配置CCL\_DEFAULT\_DIRECTORY，ccl默认的就是找的此目录。至于变量CCL\_DEFAULT\_DIRECTORY的默认值，打开ccl64那个脚本看一眼就知道了！源码说明一切！此说明来自于[ccl的文档][3]
- 安装的时候参考上面的文档就OK，暂时作者没有编译lisp-kernel

之后在terminal中运行ccl或者ccl64如下输出则OK：

```
$ ccl
Welcome to Clozure Common Lisp Version 1.10-r16196  (LinuxX8664)!

CCL is developed and maintained by Clozure Associates. For more information
about CCL visit http://ccl.clozure.com.  To enquire about Clozure's Common Lisp
consulting services e-mail info@clozure.com or visit http://www.clozure.com.

? (quit)
$ ccl -V 
Version 1.10-r16196  (LinuxX8664)
```
	
### 安装quicklisp

Quicklisp是Common Lisp的库管理工具，基于系统目前安装的Common Lisp实现用几个命令就可以下载、安装或者加载其余1000多个库

在[quicklisp][2]下载最新的quicklisp.lisp，只有55k

```
$ curl -O http://beta.quicklisp.org/quicklisp.lisp
```

之后打开终端：

	$ ccl --load quicklisp.lisp
	? (quicklisp-quickstart:install)
	? (ql:system-apropos "vecto")
	? (ql:quickload "vecto")
	? (ql:add-to-init-file)
	? (quit)
	
至此，quicklisp安装结束。这里的安装也是来自于[quicklisp文档][4]

### 安装slime

终端

	$ ccl
	? (ql:quickload :quicklisp-slime-helper)
	? (quit)

此说明也是来自于[slime的安装文档][5]

### 为Emacs添加slime

打开~/.emacs 添加：

```
(set-language-environment "utf-8")
(add-to-list 'load-path "~/src/slime/")  ;or wherever you put it

(setq inferior-lisp-program "/usr/local/bin/ccl64 -K utf-8")

(require 'slime)
(setq slime-net-coding-system 'utf-8-unix)
(slime-setup '(slime-fancy))
```
注意，上面的~/src/slime要换成slime的地址，本机是"/home/hercules/quicklisp/dists/quicklisp/software/slime-2.10.1"

之后重启Emacs M-x slime即可。

### 问题

中间如出现 /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory 则：

	yum install glibc.i686


[0]: http://common-lisp.net/project/lispbox/  "Lispbox"
[1]: http://ccl.clozure.com/download.html  "clozure CL"
[2]: http://www.quicklisp.org/ "quicklisp"
[3]: http://ccl.clozure.com/ccl-documentation.html#command-line-setup
[4]: http://www.quicklisp.org/beta/
[5]: http://trac.clozure.com/ccl/wiki/InstallingSlime
