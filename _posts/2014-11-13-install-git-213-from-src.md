---
layout: post
title: 源码方式安装最新版git
category: blog
tags: CentOS
description: 在64位CentOS6.5上使用源码方式安装最新的git
---

## 首先是安装必备的软件包
```
yum -y install curl curl-devel zlib-devel openssl-devel perl cpio expat-devel gettext-devel perl-ExtUtils-MakeMaker perl-CPAN tk xmlto openjade texinfo perl-XML-SAX
```


## 安装asciidoc

首先阅读[asciidoc][0]官网的*Distribution tarball installation*部分，然后去[SourceForge][1]下载最新的asciidoc的源代码
执行，以8.6.9为例:

```
	$ tar -xzf asciidoc-8.6.9.tar.gz
	$ cd asciidoc-8.6.9
	$ ./configure
	$ sudo make install		
```

## 安装docbook2X
```
   $ wget http://ftp.jaist.ac.jp/pub/Linux/Fedora//epel/6/x86_64/docbook2X-0.8.8-1.el6.x86_64.rpm
   $ rpm -ivh docbook2X-0.8.8-1.el6.x86_64.rpm --nodeps
   $ cd /usr/bin
   $ ln -s db2x_docbook2texi docbook2x-texi
```
## 下载并安装git
```   
   $ wget https://codeload.github.com/git/git/tar.gz/v2.1.3
   $ tar -zxvf v2.1.3
   $ cd git-2.1.3/
   $ make prefix=/usr/local all doc info
   # make prefix=/usr/local install install-doc install-html install-info
```
   











[0]: http://www.methods.co.nz/asciidoc/INSTALL.html "asciidoc"
[1]: http://sourceforge.net/projects/asciidoc/ "SourceForge"

