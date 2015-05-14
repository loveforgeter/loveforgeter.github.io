---
layout: post
title: "Jekyll使用Pygments代码高亮"
categories: blog
tags: jekyll
---

Jekyll自带语法高亮功能，是由[Pygments](http://pygments.org/)实现的，Pygments支持多种语言。Jekyll也支持[Rouge](http://rouge.jneen.net/)代码高亮

一、安装
----
> 系统
> <ul>
>   <li>Ubuntu 14.04</li>
> </ul> 
> 软件环境
> <ul>
>   <li>Jekyll 2.5.3</li>
>   <li>Python 2.7.8</li>
>   <li>Ruby 2.2.1</li>
> </ul>

####1.安装pygments
通过pip安装pygments

```sh
$ pip install pygments
```

####2.安装pygments.rb
```
$ gem install pygments.rb
```

<br>
二、使用
-------

####1.更改配置
首先配置jekyll的，使其使用pygments语法高亮，在`_config.yml`中加入

```
highlighter: pygments
```

####2.生成样式文件
使用pygmentize生成pygments.css（以monokai样式为例）：

```sh
$ pygmenize -S monokai -f > pygments.css
```

生成的样式文件如下：

```css
.hll { background-color: #49483e }
.c { color: #75715e } /* Comment */
.err { color: #960050; background-color: #1e0010 } /* Error */
.k { color: #66d9ef } /* Keyword */
.l { color: #ae81ff } /* Literal */
.n { color: #f8f8f2 } /* Name */
.o { color: #f92672 } /* Operator */
.p { color: #f8f8f2 } /* Punctuation */
.cm { color: #75715e } /* Comment.Multiline */
.cp { color: #75715e } /* Comment.Preproc */
.c1 { color: #75715e } /* Comment.Single */
.cs { color: #75715e } /* Comment.Special */
.gd { color: #f92672 } /* Generic.Deleted */
.ge { font-style: italic } /* Generic.Emph */
.gi { color: #a6e22e } /* Generic.Inserted */
.gs { font-weight: bold } /* Generic.Strong */
.gu { color: #75715e } /* Generic.Subheading */
.kc { color: #66d9ef } /* Keyword.Constant */
.kd { color: #66d9ef } /* Keyword.Declaration */
.kn { color: #f92672 } /* Keyword.Namespace */
.kp { color: #66d9ef } /* Keyword.Pseudo */
.kr { color: #66d9ef } /* Keyword.Reserved */
.kt { color: #66d9ef } /* Keyword.Type */
.ld { color: #e6db74 } /* Literal.Date */
.m { color: #ae81ff } /* Literal.Number */
.s { color: #e6db74 } /* Literal.String */
.na { color: #a6e22e } /* Name.Attribute */
.nb { color: #f8f8f2 } /* Name.Builtin */
.nc { color: #a6e22e } /* Name.Class */
.no { color: #66d9ef } /* Name.Constant */
.nd { color: #a6e22e } /* Name.Decorator */
.ni { color: #f8f8f2 } /* Name.Entity */
.ne { color: #a6e22e } /* Name.Exception */
.nf { color: #a6e22e } /* Name.Function */
.nl { color: #f8f8f2 } /* Name.Label */
.nn { color: #f8f8f2 } /* Name.Namespace */
.nx { color: #a6e22e } /* Name.Other */
.py { color: #f8f8f2 } /* Name.Property */
.nt { color: #f92672 } /* Name.Tag */
.nv { color: #f8f8f2 } /* Name.Variable */
.ow { color: #f92672 } /* Operator.Word */
.w { color: #f8f8f2 } /* Text.Whitespace */
.mb { color: #ae81ff } /* Literal.Number.Bin */
.mf { color: #ae81ff } /* Literal.Number.Float */
.mh { color: #ae81ff } /* Literal.Number.Hex */
.mi { color: #ae81ff } /* Literal.Number.Integer */
.mo { color: #ae81ff } /* Literal.Number.Oct */
.sb { color: #e6db74 } /* Literal.String.Backtick */
.sc { color: #e6db74 } /* Literal.String.Char */
.sd { color: #e6db74 } /* Literal.String.Doc */
.s2 { color: #e6db74 } /* Literal.String.Double */
.se { color: #ae81ff } /* Literal.String.Escape */
.sh { color: #e6db74 } /* Literal.String.Heredoc */
.si { color: #e6db74 } /* Literal.String.Interpol */
.sx { color: #e6db74 } /* Literal.String.Other */
.sr { color: #e6db74 } /* Literal.String.Regex */
.s1 { color: #e6db74 } /* Literal.String.Single */
.ss { color: #e6db74 } /* Literal.String.Symbol */
.bp { color: #f8f8f2 } /* Name.Builtin.Pseudo */
.vc { color: #f8f8f2 } /* Name.Variable.Class */
.vg { color: #f8f8f2 } /* Name.Variable.Global */
.vi { color: #f8f8f2 } /* Name.Variable.Instance */
.il { color: #ae81ff } /* Literal.Number.Integer.Long */
```

> 可以输入以下命令查看有哪些主题可用
> 
> ```python
> >>> from pygments.styles import STYLE_MAP
> >>> STYLE_MAP.keys()
> ['monokai', 'manni', 'rrt', 'perldoc', 'borland', 'colorful', 'default', 'murphy', 'vs', 'trac', 'tango', 'fruity', 'autumn', 'bw', 'emacs', 'vim', 'pastie', 'friendly', 'native']
> ```

####3.样式微调
修改pygments.css样式文件,将第一行

```css
.hll { background-color: #49483e }
```
修改为

```css
.highlight pre,.highlight code,.hll pre,.hll code { background-color: #49483e }
```

####4.更多的样式设置
代码样式code.css：

```css
/* code.css */
.highlight pre code * {
    white-space: nowrap;
}

.highlight pre {
    overflow-x: auto;
}

.highlight pre code {
    white-space: pre;
}
```

####5.使用样式文件
首先将样式文件***pygments.css***和***code.css***拷贝到`path/to/yourblog/css`目录<br>
然后修改`path/to/yourblog/_includes/head.html`，在`{% raw %}<link rel="stylesheet" href="{{ "/css/main.css" | prepend: site.baseurl }}">{% endraw %}`后面加入

```
{% raw %}
<link rel="stylesheet" href="{{ "/css/code.css" | prepend: site.baseurl }}">
<link rel="stylesheet" href="{{ "/css/pygments.css" | prepend: site.baseurl }}">
{% endraw %}
```
####6.使用代码高亮
是时候使用代码高亮了

```
{% raw %}

{% highlight c %}
#include <stdio.h>
int main(int argc,const char* args[]){
    printf("Hello World!");
}
{% endhighlight %}

{% endraw %}
```
效果如下
![c demo](/images/jekyll-pygments-c-demo.png)

参考
---

- [Jekyll 中的语法高亮：Pygments](http://havee.me/internet/2013-08/support-pygments-in-jekyll.html)
- [用Jekyll和Pygments配置代码高亮](http://zyzhang.github.io/blog/2012/08/31/highlight-with-Jekyll-and-Pygments/)
- [MY JEKYLL BLOG SETUP WITH BOOTSTRAP, SASS AND PYGMENTS](http://kvurd.com/blog/my-jekyll-blog-setup-bootstrap-sass-pygments/)