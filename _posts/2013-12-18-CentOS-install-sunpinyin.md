---
layout: post
title: CentOS6.5安装sunpinyin输入法
category: blog
description: CentOS6.5 自带的输入法词库较少,试试sunpinyin如何
---

感觉centos6.4自带的那个输入法不太舒服，喜欢折腾，就安装了sunpinyin试试
安装过程参考 https://code.google.com/p/sunpinyin/wiki/BuildUnix
依赖组件：

	yum install make gcc*  ibus-devel  sqlite-devel scons
	
安装：
从http://code.google.com/p/sunpinyin/下载sunpinyin-2.0.3.tar.gz 和 ibus-sunpinyin-2.0.3.tar.gz
解压
	tar -zxvf sunpinyin-2.0.3.tar.gz
	cd sunpinyin-2.0.3
由于词库修改，修改raw中的Makefile文件，文件名在https://code.google.com/p/open-gram/downloads/list
修改为：

	LM_URL=http://open-gram.googlecode.com/files
	WGET=wget
	TAR=tar
	all: lm_sc.t3g.arpa dict.utf8
	    @echo done
	lm_sc.t3g.arpa: stamp-lm
	stamp-lm: lm_sc.t3g.arpa.tar.bz2
	    $(TAR) -jxf $^
	    touch $@
	lm_sc.t3g.arpa.tar.bz2:
	    $(WGET) -O lm_sc.t3g.arpa.tar.bz2 $(LM_URL)/lm_sc.t3g.arpa-20121025.tar.bz2
	dict.utf8: stamp-dict
	stamp-dict: dict.utf8.tar.bz2
	    $(TAR) -jxf $^
	    touch $@
	dict.utf8.tar.bz2:
	    $(WGET) -O dict.utf8.tar.bz2 $(LM_URL)/dict.utf8-20131212.tar.bz2
	clean:
	    @rm -f stamp-dict stamp-lm lm_sc.t3g.arpa dict.utf8
	    @echo cleaned
		
之后编译安装：

	scons --prefix=/usr
	scons install
	
再安装ibus-sunpinyin

	tar -zxvf ibus-sunpinyin-2.0.3.tar.gz & cd
	
编译安装

	scons --prefix
	scons install
	
这里可能会出问题 Checking for sunpinyin-2.0...no
解决方式是 将源码目录中的sunpinyin-2.0.3.pc复制到/etc/share/pkgconfig或者是把/usr/lib/pkgconfig下的sunpinyin-2.0.pc复制到/etc/share/pkgconfig中 
此时重启ibus添加sunpinyin，启动卡死
解决：

	cd /etc/ld.so.conf.d/
	mkdir -p sunpinyin/usr/lib
	ldconfig
	
重启ibus即可
