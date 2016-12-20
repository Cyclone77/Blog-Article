---
title: Sass入门
tags:
  - Sass
date: 2016-12-18
updated: 2016-12-19
---


## 安装
Sass是基于ruby的，要先安装ruby环境。[ruay下载](http://rubyinstaller.org/downloads)， ruby安装好以后用以下命令安装sass
``` ruby
gem install sass

sass -v //检查是否安装成功
```
接下来就可以学习sass了

## 使用
sass提供命令把scss转换成css；sass文件扩展名有2种，sass和scss。

1. sass是以严格的语法规则来书写的，不带大括号"{}"和分号"；"

2. scss则是和css类似的语法方式书写的。

> 这里我们用学习成本低的，适合开发者入手的scss学起。

<!--more-->

### 编译
准确的说只是个转换而已，把sass或scss转换成css。
``` ruby
//通过以下命令把scss 转换成 css
sass input.scss output.css
```
但是在实际项目里面不可能只有一个css，而且css改动非常频繁，每次改动岂不是要进行转换一次？
sass提供监听命令，--watch 可以监听一个或者整个目录的scss的变化，每次保存（并且有变动scss都进行转换；通过以下命令：
``` ruby
//意思是把LearnSass目录下的scss全部转换成css，然后保存在LearnCss目录下
sass --watch LearnSass:LearnCss
```
SASS开源社区提供一个[可视化编译工具](http://koala-app.com/index-zh.html)，以下例子都可以运行。

开始学习sass的语法

### 变量
用来存储一些重复的CSS值(在整个站点中一致的值)，sass用$定义变量：
``` scss
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```
** $primary-color: ** 变量的一致性可以使得在改变颜色后不用去整个站点替换，只需要改变这个变量的值即可。

编译后，如下：
``` CSS
body {
    font: 100% Helvetica, sans-serif;
    color: #333;
}
```

### 嵌套
Sass可以明确的体现层次关系：
``` scss
nav{
    ul{
        list-style: none;
    }
    li{
        display: inline-block;
    }
    a{
        display: block;
        padding: 6px 12px;
        text-decoration: none;
    }
}
```
转换后的css:
``` css
nav ul {
    list-style: none;
}

nav li {
    display: inline-block;
}

nav a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
}
```
很明显Sass的写法更容易体现HTML的层次关系。

### Partials
有时候你也许不需要每个scss文件都去转换成css，那么Sass定义了一个规则，用"_"下划线开头的Sass文件则不会被转换。
比如用到：
``` scss
@import '_partials';
```
>** 导入一个css，_partials后缀scss可以省略，但是单引号必须要有!**

### 导入
``` scss
//_partials.scss文件
html,body,ul,ol{
    margin: 0;
    padding: 0;
}

body {
    background-color: $back-color;
    font-family: $font-stack;
}
```
``` scss
//Basics.scss文件
$font-stack: "Microsoft Yahei";
$back-color: #eee;

@import '_partials';
```
最终看到的css文件是这样的：
``` css
//Basics.css文件
html,
body,
ul,
ol {
    margin: 0;
    padding: 0;
}

body {
    background-color: #eee;
    font-family: "Microsoft Yahei";
}
```
> ** 可以看到在主scss文件中的变量在导入的scss文件中可以使用  **

### Mixin
有点像是一个函数。
现在css3各大浏览器的写法都不一样，一条样式要写3，4遍麻烦……
用Mixin生成每个浏览器供应商的前缀。
``` scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}
//使用@include命令，调用这个mixin。
.box { 
    @include border-radius(10px); 
    width: 100px;
    height: 60px;
    border: 1px solid;
}
```
> ** @include命令用来调用定义的 Mixin。 **
对应的css:
``` css
.box {
    -webkit-border-radius: 10px;
       -moz-border-radius: 10px;
        -ms-border-radius: 10px;
            border-radius: 10px; 
    width: 100px;
    height: 60px;
    border: 1px solid;
}
```
### 继承
使用@extend命令可以引用一个选择器的样式。
``` scss
.message {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
}

.success {
    @extend .message;
    border-color: green;
}

.error {
    @extend .message;
    border-color: red;
}

.warning {
    @extend .message;
    border-color: yellow;
}
```
这样的写法可以避免HTML元素上多写类名。这样合理的体现了代码重用。
转换后的css:
``` css
.message,
.success,
.error,
.warning {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
}

.success {
    border-color: green;
}

.error {
    border-color: red;
}

.warning {
    border-color: yellow;
}
```
### 计算
Sass提供简单的计算，提供标准运算符：+， -， *， /，和%。
``` scss
.container { width: 100%; }

.roleHeight{
    height: 100px;
    border: 1px solid;
    margin-top: 20px;
}

article[role="main"] {
    @extend .roleHeight;
    float: left;
    width: 600px / 960px * 100%;
}

aside[role="complementary"] {
    @extend .roleHeight;
    float: right;
    width: 300px / 960px * 100%;
}
```
这是最经典的左右布局。转换后的css:
``` css
.container {
    width: 100%;
}

.roleHeight,
article[role="main"],
aside[role="complementary"] {
    height: 100px;
    border: 1px solid;
    margin-top: 20px;
}

article[role="main"] {
    float: left;
    width: 62.5%;
}

aside[role="complementary"] {
    float: right;
    width: 31.25%;
}
```
到这里相信对Sass的用法也简单了解了，以后的文章会具体介绍下，在项目中的实际使用。

> [有关代码](https://github.com/Cyclone77/sass-example)