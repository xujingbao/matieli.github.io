---
layout: post
title: Windows Jekyll建站步骤
date: 2016-08-11 15:32:24.000000000 +09:00
tag: Jekyll
---

* TOC
{:toc}


#### **What's this**

[Vno Jekyll](https://github.com/onevcat/vno-jekyll) is a theme for [Jekyll](http://jekyllrb.com). It is a port of my Ghost theme [vno](https://github.com/onevcat/vno), which is originally developed from [Dale Anthony's Uno](https://github.com/daleanthony/uno).

---

#### **安装Ruby、Gem**
[Ruby Downloads](http://rubyinstaller.org/downloads)

安装完成后输入

```bash
$ ruby -v
$ gem -v
```

在安装ruby 的gem的时候可能会出现下面这样的提示，所以特分享于此。

```
ERROR:  Error installing XXXXXXXXXXX:
            The 'XXXXXXXXXXXX' native gem requires installed build tools.
     
    Please update your PATH to include build tools or download the DevKit
    from 'http://rubyinstaller.org/downloads' and follow the instructions
    at 'http://github.com/oneclick/rubyinstaller/wiki/Development-Kit'
```

出错的原因是安装XXXXX的时候，需要build tools，但系统中没有。出错信息中同时也给出了解决的法案：

* 到 [Ruby Downloads](http://rubyinstaller.org/downloads) 去下载devKit,(我下载的这个：DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe)

* 按照 http://github.com/oneclick/rubyinstaller/wiki/Development-Kit/ 安装dev kit

主要安装步骤如下：

>- 1.如果原来系统中已经安装了旧版的dev kit, 则删除它 

>- 2.下载上面提到的dev kit 

>- 3.解压下载下来的文件到指定的目录，如c:/devkit。(注意：目录不能有空格) 

>- 4.在安装目录下运行ruby dk.rb,然后按照提示分别运行ruby dk.rb init 和 ruby dk.rb install来增强ruby 

>- 5.可以运行 gem install rdiscount –platform=ruby 来测试是否成功 

按照安装步骤，完成了DevKit的安装，非常简单。

然后，再次安装需要的gem即可。

安装完成gem后需要修改镜像地址

```bash
$ gem sources --remove https://rubygems.org/
$ gem sources --add http://gems.ruby-china.org/
```

---

#### **安装bundler**

安装完成后 需要修改bundler的镜像地址

```bash
$ gem install bundler 
$ bundle config 'mirror.https://rubygems.org' 'http://gems.ruby-china.org/'
```
至此准备工作准备完毕

---

#### **安装jekyll**

```bash
$ git clone https://github.com/onevcat/vno-jekyll.git your_site
$ cd your_site
$ bundler install
$ bundler exec jekyll serve
```

---

#### **Window10-错误**

```
Syntax error: Invalid GBK character "\xE5"
		on line 8 of E:\work\sass\sass\_big_box.scss
		from line 16 of E:\work\sass\sass\main.scss
		Use --trace for backtrace.
```

找到安装目录里面sass-3.3.7模块下面的engine.rb文件，例如下面路径：
```
D:\Ruby23-x64\lib\ruby\gems\2.3.0\gems\sass-3.4.21\lib\sass
```
在这个文件里面engine.rb，添加一行代码
Encoding.default_external = Encoding.find('utf-8')
放在第一行即可。
然后在运行启动命令，成功了：http://127.0.0.1:4000

---

#### **配置与个性修改**

修改_config.yml进行配置，修改对应的html进行修改吧。

---

#### **替换成kramdown**

使用默认的markdown，github上老给发警告邮件，建议换成kramdown
现在，想尝试使用karkdown的语法。kramdown是一个Markdown解析器，它能够正确解释公式内部的符号，不会与Markdown语法冲突，比如不会将^符号变成 标签。

安装kramdown

```bash
$ gem install kramdown
```

在_config.yml中的配置方式为：

```
markdown: kramdown
kramdown:
  input:  GFM
  use_coderay: true #我使用的版本中提示需要换成：enable_coderay
```

在编写文章时，插入下面代码，渲染之后就可以生成TOC了：

```
* TOC
{:toc}
```

krmadown支持和github一样的语法高亮，用三个 ```，但是需要安装coderay，而github pages上不支持coderay，所以该方式无法搞定，可行的解决方法是上传本地编译好的html。如果是本地或者自己的空间，可以安装coderay。

```bash
$ gem install coderay
```

最后的配置为

```
kramdown:
  input: GFM
  extensions:
    - autolink
    - footnotes
    - smart
  use_coderay: true
  syntax_highlighter: rouge
  coderay:
    coderay_line_numbers:  nil
```

coderay支持的语言有限，并且rouge兼容Pygments，故这里使用rouge：

```bash
$ gem install rouge
```

