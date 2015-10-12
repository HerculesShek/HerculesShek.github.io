---
layout: post
title: google-protobuf使用指南
category: blog
tags: google protobuf
description: C++方式记录google protobuf使用指南
---

## 首先是安装必备的包 本人使用的是fedora 22
```
sudo dnf install gcc-g++
```

## 安装protoc

首先阅读[protobuf介绍][0]官网的*What are protocol buffers?*部分 清晰明了的介绍 就是类似于XML方式的格式化数据序列化 然后去[protobuf-download][1]下载最新的protobuf 安装过程以2.6.1为例:

```
	$ tar -xzvf protobuf-2.6.1.tar.gz
	$ cd protobuf-2.6.1
	$ ./configure
  $ make
  $ make check
	$ sudo make install
```


## C++ demo实践
这里主要是跟着[C++ toturial][2]来进行 首先编写`.proto`文件
```   

```
   





[0]: https://developers.google.com/protocol-buffers/ "protobuf介绍"
[1]: https://developers.google.com/protocol-buffers/docs/downloads "protobuf-download"
[2]: https://developers.google.com/protocol-buffers/docs/cpptutorial "C++ Toturial"

