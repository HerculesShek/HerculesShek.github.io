---
layout: post
title: 前端 HTML CSS JS 学习指南
category: blog
tags: 前端 HTML CSS JS
description: 前端HTML CSS JS学习指南
---

## 首先是安装必备的包
环境：fedora 22 64位

```
sudo dnf install gcc-g++
```

## 安装protoc

会生成2个文件 `addressbook.pb.cc  addressbook.pb.h`


```
$ g++ -l protobuf addressbook.pb.cc message_writer.cc -o message_w
$ ./message_w book
./message_w: error while loading shared libraries: libprotobuf.so.9: cannot open shared object file: No such file or directory
```

这里一定注意的是 使用 g++ 并且要使用`-l protobuf`选项！
此时执行一般会报上面的错误：

```
$ export LD_LIBRARY_PATH=/usr/local/lib
$ ./message_w book
```






[0]: https://developers.google.com/protocol-buffers/ "protobuf介绍"
[1]: https://developers.google.com/protocol-buffers/docs/downloads "protobuf-download"
[2]: https://developers.google.com/protocol-buffers/docs/cpptutorial "C++ Toturial"

