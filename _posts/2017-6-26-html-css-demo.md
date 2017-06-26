---
layout: post
title: google-protobuf使用指南
category: blog
tags: google protobuf
description: C++方式记录google protobuf使用指南
---

## 首先是安装必备的包
环境：fedora 22 64位

```
sudo dnf install gcc-g++
```

## 安装protoc

首先阅读[protobuf介绍][0]官网的**What are protocol buffers?**部分 清晰明了的介绍 就是类似于XML方式的格式化数据序列化 然后去[protobuf-download][1]下载最新的protobuf 安装过程以2.6.1为例:

```
$ tar -xzvf protobuf-2.6.1.tar.gz
$ cd protobuf-2.6.1
$ ./configure
$ make
$ make check
$ sudo make install
```

## C++ demo实践
这里主要是跟着[C++ toturial][2]来进行 首先编写`.proto`文件 `emacs addressbook.proto`

```   
package tutorial;

message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phone = 4;
}

message AddressBook {
  repeated Person person = 1;
}
```

然后在当前目录下执行：

```
protoc -I=./ --cpp_out=./ addressbook.proto 
```

会生成2个文件 `addressbook.pb.cc  addressbook.pb.h`

序列化到文件中 `emacs message_writer.cc` :

```
#include <iostream>
#include <fstream>
#include <string>
#include "addressbook.pb.h"
using namespace std;

// This function fills in a Person message based on user input.
void PromptForAddress(tutorial::Person* person) {
  cout << "Enter person ID number: ";
  int id;
  cin >> id;
  person->set_id(id);
  cin.ignore(256, '\n');

  cout << "Enter name: ";
  getline(cin, *person->mutable_name());

  cout << "Enter email address (blank for none): ";
  string email;
  getline(cin, email);
  if (!email.empty()) {
    person->set_email(email);
  }

  while (true) {
    cout << "Enter a phone number (or leave blank to finish): ";
    string number;
    getline(cin, number);
    if (number.empty()) {
      break;
    }

    tutorial::Person::PhoneNumber* phone_number = person->add_phone();
    phone_number->set_number(number);

    cout << "Is this a mobile, home, or work phone? ";
    string type;
    getline(cin, type);
    if (type == "mobile") {
      phone_number->set_type(tutorial::Person::MOBILE);
    } else if (type == "home") {
      phone_number->set_type(tutorial::Person::HOME);
    } else if (type == "work") {
      phone_number->set_type(tutorial::Person::WORK);
    } else {
      cout << "Unknown phone type.  Using default." << endl;
    }
  }
}

// Main function:  Reads the entire address book from a file,
//   adds one person based on user input, then writes it back out to the same
//   file.
int main(int argc, char* argv[]) {
  // Verify that the version of the library that we linked against is
  // compatible with the version of the headers we compiled against.
  GOOGLE_PROTOBUF_VERIFY_VERSION;

  if (argc != 2) {
    cerr << "Usage:  " << argv[0] << " ADDRESS_BOOK_FILE" << endl;
    return -1;
  }

  tutorial::AddressBook address_book;

  {
    // Read the existing address book.
    fstream input(argv[1], ios::in | ios::binary);
    if (!input) {
      cout << argv[1] << ": File not found.  Creating a new file." << endl;
    } else if (!address_book.ParseFromIstream(&input)) {
      cerr << "Failed to parse address book." << endl;
      return -1;
    }
  }

  // Add an address.
  PromptForAddress(address_book.add_person());

  {
    // Write the new address book back to disk.
    fstream output(argv[1], ios::out | ios::trunc | ios::binary);
    if (!address_book.SerializeToOstream(&output)) {
      cerr << "Failed to write address book." << endl;
      return -1;
    }
  }

  // Optional:  Delete all global objects allocated by libprotobuf.
  google::protobuf::ShutdownProtobufLibrary();

  return 0;
}
```

编译执行：

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

编写序列化文件读取 `emacs message_reader.cc`

```
#include <iostream>
#include <fstream>
#include <string>
#include "addressbook.pb.h"
using namespace std;

// Iterates though all people in the AddressBook and prints info about them.
void ListPeople(const tutorial::AddressBook& address_book) {
  for (int i = 0; i < address_book.person_size(); i++) {
    const tutorial::Person& person = address_book.person(i);

    cout << "Person ID: " << person.id() << endl;
    cout << "  Name: " << person.name() << endl;
    if (person.has_email()) {
      cout << "  E-mail address: " << person.email() << endl;
    }

    for (int j = 0; j < person.phone_size(); j++) {
      const tutorial::Person::PhoneNumber& phone_number = person.phone(j);

      switch (phone_number.type()) {
        case tutorial::Person::MOBILE:
          cout << "  Mobile phone #: ";
          break;
        case tutorial::Person::HOME:
          cout << "  Home phone #: ";
          break;
        case tutorial::Person::WORK:
          cout << "  Work phone #: ";
          break;
      }
      cout << phone_number.number() << endl;
    }
  }
}

// Main function:  Reads the entire address book from a file and prints all
//   the information inside.
int main(int argc, char* argv[]) {
  // Verify that the version of the library that we linked against is
  // compatible with the version of the headers we compiled against.
  GOOGLE_PROTOBUF_VERIFY_VERSION;

  if (argc != 2) {
    cerr << "Usage:  " << argv[0] << " ADDRESS_BOOK_FILE" << endl;
    return -1;
  }

  tutorial::AddressBook address_book;

  {
    // Read the existing address book.
    fstream input(argv[1], ios::in | ios::binary);
    if (!address_book.ParseFromIstream(&input)) {
      cerr << "Failed to parse address book." << endl;
      return -1;
    }
  }

  ListPeople(address_book);

  // Optional:  Delete all global objects allocated by libprotobuf.
  google::protobuf::ShutdownProtobufLibrary();

  return 0;
}
```

之后：

```
$ g++ -l protobuf addressbook.pb.cc message_reader.cc -o message_r
[hercules@diana cpp]$ ./message_r book 
Person ID: 7
  Name: will
  E-mail address: xz.will.shek@gmail.com
  Mobile phone #: 13355667788
Person ID: 4
  Name: will
  E-mail address: will@gmai.com
Person ID: 6
  Name: Jack
  E-mail address: Jack.ma@alibaba.com
  Home phone #: 13370614984
  Home phone #: 88887777
Person ID: 8
  Name: Tom
  E-mail address: tom@126.com
  Mobile phone #: 13344445555
  Mobile phone #: 14433332222
  Work phone #: 88896754
```


[0]: https://developers.google.com/protocol-buffers/ "protobuf介绍"
[1]: https://developers.google.com/protocol-buffers/docs/downloads "protobuf-download"
[2]: https://developers.google.com/protocol-buffers/docs/cpptutorial "C++ Toturial"

