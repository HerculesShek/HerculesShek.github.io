---
layout: post
title: CentOS6.5��װsunpinyin���뷨
category: blog
description: CentOS6.5�Դ������뷨�ʿ���٣���һ��sunpinyin
---

�о�centos6.4�Դ����Ǹ����뷨��̫�����ϲ�����ڣ��Ͱ�װ��sunpinyin����
��װ���̲ο� https://code.google.com/p/sunpinyin/wiki/BuildUnix
���������
	yum install make gcc*  ibus-devel  sqlite-devel scons
��װ��
��http://code.google.com/p/sunpinyin/����sunpinyin-2.0.3.tar.gz �� ibus-sunpinyin-2.0.3.tar.gz
��ѹ
	tar -zxvf sunpinyin-2.0.3.tar.gz
	cd sunpinyin-2.0.3
���ڴʿ��޸ģ��޸�raw�е�Makefile�ļ����ļ�����https://code.google.com/p/open-gram/downloads/list
�޸�Ϊ��
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
֮����밲װ��
	scons --prefix=/usr
	scons install
�ٰ�װibus-sunpinyin
	tar -zxvf ibus-sunpinyin-2.0.3.tar.gz & cd
���밲װ
	scons --prefix
	scons install
������ܻ������ Checking for sunpinyin-2.0...no
�����ʽ�� ��Դ��Ŀ¼�е�sunpinyin-2.0.3.pc���Ƶ�/etc/share/pkgconfig�����ǰ�/usr/lib/pkgconfig�µ�sunpinyin-2.0.pc���Ƶ�/etc/share/pkgconfig�� 
��ʱ����ibus���sunpinyin����������
�����
	cd /etc/ld.so.conf.d/
	mkdir -p sunpinyin/usr/lib
	ldconfig
����ibus����
