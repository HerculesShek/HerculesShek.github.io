---
layout: post
title: Jekyll & Markdown Note
category: blog
tags: jekyll markdown 
description: personal note
---

{% assign openTag = '{%' %}
{% assign openTag2 = '{{' %}

## 前言
本文记录如何使用jekyll来写博客，本站就是使用jekyll和markdown建立，网站内容托管在[github pages][0]上，域名从godaddy购买，我在本地部署了jekyll环境，便于直接看到文章或者页面修改的效果。

## 本机安装jekyll环境
mac下安装ruby和jekyll，因为mac自带了ruby，但是它的版本不是最新的，这里使用[RVM][1]（Ruby Version Manager）来管理ruby的版本，所以需要首先安装rvm，可以参考官网的说明

```
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
$ \curl -sSL https://get.rvm.io | bash -s stable
```

安装完之后，需要source两个文件 一般是`.bash_profile`和`.zshrc` 具体需要看安装完之后的提示。

## jekyll相关
### jekyll网站的目录结构
```
├── CNAME
├── README.md
├── _config.yml
├── _includes
│   ├── about.html
│   ├── default.css
│   └── disqus.html
├── _layouts
│   ├── default.html
│   ├── home.html
│   └── post.html
├── _posts
│   ├── 2013-12-30-dynamic-programming-LCS.md
│   └── 2013-3-21-loop-invariants.md
├── atom.xml
├── favicon.ico
├── assets
├── _sass
├── _drafts
├── images
│   ├── external_link.gif
│   ├── external_link_on.gif
│   ├── icons.gif
│   └── octocat.ico
├── index.html
├── 404.html
├── LICENSE.txt
└── js
    ├── highlighter
    │   ├── lang-apollo.js
    │   ├── lang-basic.js
    │   └── lang-clj.js
    ├── jquery-1.7.1.min.js
    └── post.js
```
[官方介绍][5]

| 文件/目录  | 描述   | 
|---------|---|
|  _config.yml  &nbsp; | 保存配置数据。很多配置选项都可以直接在命令行中进行设置，但是如果你把那些配置写在这儿，你就不用非要去记住那些命令了。  |  
| _drafts  | drafts（草稿）是未发布的文章  | 
| _includes  | 你可以加载这些包含部分到你的布局或者文章中以方便重用。可以用这个标签  {{openTag}} include file.ext %} 来把文件 _includes/file.ext 包含进来。|
|_layouts||
|_posts||
|_data||
|_site||
{: rules="groups"}
{:.mbtablestyle}

<br/>




## Liquid模板语言相关 
<hr/>
Liquid相关介绍学习看[官网][2]，这里的介绍比较详细。<br/>
[jekyll的一些小技巧][3] <br/>
[比如转义][4] 

###  Basic
+ Tags 标签 其实就像其他编程语言中的关键字 比如 `{{ openTag }} if user.name == 'will' %} Hey Will {{ openTag }} endif %}`
- Objects 就是带有属性的对象 比如 `{{ openTag2 }} product.title }}`
- Filters 类似于编程语言中的方法

## jekyll下的markdown几个技巧
### 解释器
jekyll默认的markdown解释器是kramdown 用这个就好了

### 表格
markdown中加入一个表格的话可以给这个表格加入class，比如：`{:.mbtablestyle}`，可以查看本文的markdown源码。[参考于此][6]，表格的样式可以参考：
```

table.mbtablestyle tr:nth-child(odd) {
    background-color: #F5F5F5;
}

table.mbtablestyle th {
    vertical-align: baseline;
    padding: 5px 15px 5px 6px;
    background-color: #3F3F3F;
    border: 1px solid #3F3F3F;
    text-align: left;
    color: #fff;
    text-align: left;
}

.mbtablestyle {
    border-collapse: collapse;
    margin-top: 15px;
    border: 1px solid #aaa;
    width: 100%;
}

.mbtablestyle > tbody {
    display: table-row-group;
    vertical-align: middle;
}

```
{:.css}

[0]: https://pages.github.com "Github Pages"
[1]: https://rvm.io/ "Ruby Version Manager"
[2]: https://help.shopify.com/themes/liquid
[3]: http://blog.ingphone.com/jekyll/2015/01/28/jekyll-tips.html 
[4]: https://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags
[5]: http://jekyllcn.com/docs/structure/
[6]: https://stackoverflow.com/questions/28806135/jekyll-kramdown-how-to-display-table-border


