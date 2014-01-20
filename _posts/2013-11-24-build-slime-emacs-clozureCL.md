---
layout: post
title: 使用最新的emacs+slime+CCL建立Common Lisp IDE
category: blog
description: lispbox现在已经不更新了，都是使用的老版本的emacs、slime和quicklisp
---

## 介绍

[Lispbox][0]安装简易，是新手学习Lisp无痛起步的不二之选。但现在没人维护了，其中使用的emacs，slime，ccl和quicklisp等都是比较老的版本了。所以我打算记录一下如何使用最新的上述工具搭建Common Lisp的IDE。

### 准备

- CentOS 6.5 64bit
- Emacs 24.3.1 64bit
- 下载[Clozure CL][1]这个是Common Lisp的一个优秀实现，CentOS系统选择 Linux的x86即可，其中包含了32和64的版本

## 步骤

### 安装ccl

- 过程还是比较简单的，解压下载的ccl，将目录下的scripts中的ccl（32位）或者是ccl64（64位）复制到/usr/local/bin或者是其他path能找到的路径中。
- 在自己的~中的.bashrc或者.bash_profile中添加变量 CCL\_DEFAULT\_DIRECTORY 为解压的ccl的目录的位置，比如/usr/local/src/ccl，如果不配置这个变量的话，则把解压的目录放到/usr/local/src/ccl中即可，因为不配置CCL\_DEFAULT\_DIRECTORY，ccl默认的就是找的此目录。

之后在terminal中运行ccl或者ccl64如下输出则OK：

	$ ccl
	Welcome to Clozure Common Lisp Version 1.9-r15756  (LinuxX8632)!
	? 
	
### 安装quicklisp

在[quicklisp][2]下载最新的quicklisp.lisp

	$ curl -O http://beta.quicklisp.org/quicklisp.lisp

之后打开终端：

	$ ccl --load quicklisp.lisp
	? (quicklisp-quickstart:install)
	? (ql:system-apropos "vecto")
	? (ql:quickload "vecto")
	? (ql:add-to-init-file)
	? (quit)
	
至此，quicklisp安装结束。

### 安装slime

终端

	$ ccl
	? (ql:quickload :quicklisp-slime-helper)
	? (quit)

### 为Emacs添加slime

打开~/.emacs 添加：

	(add-to-list 'load-path "~/src/slime/")  ;or wherever you put it

	(setq inferior-lisp-program "/usr/local/bin/ccl64 -K utf-8")

	(require 'slime)
	(setq slime-net-coding-system 'utf-8-unix)
	(slime-setup '(slime-fancy))

注意，上面的~/src/slime要换成slime的地址，本机是"~/quicklisp/dists/quicklisp/software/slime-20131211-cvs"

之后重启Emacs M-x slime即可。

### 问题

中间如出现 /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory 则：

	yum install glibc.i686


[0]: http://common-lisp.net/project/lispbox/  "Lispbox"
[1]: http://ccl.clozure.com/download.html  "clozure CL"
[2]: http://www.quicklisp.org/ "quicklisp"