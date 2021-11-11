+++
title = "HTML 教程笔记"
description = ""
weight = 1
+++

- [HTML Attributes](#html-attributes)
  - [The href Attrribute](#the-href-attrribute)
  - [The src Attribute](#the-src-attribute)
  - [The width and height Atrributes](#the-width-and-height-atrributes)
  - [The alt Attribute](#the-alt-attribute)
  - [The style Attribute](#the-style-attribute)
  - [The title Attribute](#the-title-attribute)
- [HTML Headings](#html-headings)
  - [HTML Headings](#html-headings-1)
  - [Bigger Headings](#bigger-headings)
- [HTML Paragraphs](#html-paragraphs)
  - [HTML Paragraphs](#html-paragraphs-1)
  - [THML Horizontal Rules](#thml-horizontal-rules)
  - [HTML Line Breaks](#html-line-breaks)
  - [The Poem Problem](#the-poem-problem)
- [HTML Styles](#html-styles)
  - [Syntax](#syntax)
  - [Backgroud Color](#backgroud-color)
  - [Text Color](#text-color)
  - [Fonts](#fonts)
  - [Text Size](#text-size)
  - [Text Alignment](#text-alignment)
- [HTML Formatting](#html-formatting)
  - [HTML `<b>` and `<strong>` Elements](#html-b-and-strong-elements)
  - [HTML `<i>` and `<em>` Elements](#html-i-and-em-elements)
  - [HTML `<small>` Elements](#html-small-elements)
  - [HTML `<mark>` Elements](#html-mark-elements)
  - [HTML `<del>` Element](#html-del-element)
  - [THML `<ins>` Element](#thml-ins-element)
  - [THML `<sub>` Element](#thml-sub-element)
  - [HTML `<sup>` Element](#html-sup-element)
- [HTML Quotation and Citation Element](#html-quotation-and-citation-element)
  - [HTML `<blockquote>` For Quotations](#html-blockquote-for-quotations)
  - [HTML `<q>` for Short Quotations](#html-q-for-short-quotations)
  - [HTML `<abbr>` for Abbreviations](#html-abbr-for-abbreviations)
  - [HTML `<address>` for Contact](#html-address-for-contact)
  - [HTML `<cite>` for Work Title](#html-cite-for-work-title)
  - [HTML `<bdo>` for Bi-Derectional Override](#html-bdo-for-bi-derectional-override)
- [THML Comments](#thml-comments)
  - [Hide Content](#hide-content)
  - [Hide Inline Content](#hide-inline-content)
- [HTML Colors](#html-colors)
  - [Colors](#colors)
  - [RGB](#rgb)
  - [HEX](#hex)
  - [HSL](#hsl)
- [HTML Links](#html-links)
  - [Links](#links)
    - [The target Attribute](#the-target-attribute)
    - [Use an Image as a Link](#use-an-image-as-a-link)
    - [Link to an Email Address](#link-to-an-email-address)
    - [Button as a Link](#button-as-a-link)
    - [Link Titles](#link-titles)
  - [Link Colors](#link-colors)
  - [Link Buttons](#link-buttons)
  - [Link Bookmarks](#link-bookmarks)
- [HTML Images](#html-images)
  - [Width and Height, or Style?](#width-and-height-or-style)
  - [Image Floating](#image-floating)
  - [Image Map](#image-map)
  - [Backgroud Images](#backgroud-images)

## HTML Attributes

{{< lead >}}
HTML attributes provide additional information about HTML elements.
{{< /lead >}}

### The href Attrribute

{{< doublecode lang="html" >}}
<a href="https://www.shisanshiji.com">十三时记</a>
{{< /doublecode >}}

### The src Attribute

{{< doublecode lang="html" >}}
<img src="/img/favicon.png">
{{< /doublecode >}}

### The width and height Atrributes

{{< doublecode lang="html" >}}
<img src="/img/favicon.png" width="100" height="100">
{{< /doublecode >}}

### The alt Attribute

{{< doublecode lang="html" >}}
<img src="/img/notfound.png" alt="Favicon">
{{< /doublecode >}}

### The style Attribute

{{< doublecode lang="html" >}}
<p style="color:darkorange">十三时记</p>
{{< /doublecode >}}

### The title Attribute

{{< doublecode lang="html" >}}
<p title="个人博客">十三时记</p>
{{< /doublecode >}}

## HTML Headings

{{< lead >}}
HTML headings are titles or subtitles that you want to display on a webpage.
{{< /lead >}}

### HTML Headings

{{< doublecode lang="html" >}}
<h1>标题 1</h1>
<h2>标题 2</h2>
<h3>标题 3</h3>
<h4>标题 4</h4>
<h5>标题 5</h5>
<h6>标题 6</h6>
{{< /doublecode >}}

### Bigger Headings

{{< doublecode lang="html" >}}
<h1 sytle="font-size:60px;">标题 1</h1>
{{< /doublecode >}}

## HTML Paragraphs

{{< lead >}}
A paragraph always starts on a new line, and is usually a block of text.
{{< /lead >}}

### HTML Paragraphs

{{< doublecode lang="html" >}}
<p>第一段落。</p>
<p>第二段落。</p>
{{< /doublecode >}}

### THML Horizontal Rules

{{< doublecode lang="html" >}}
<h1>侠客行（节选）</h1>
<p>十步杀一人，千里不留行。</p>
<hr>
<p>事了拂衣去，深藏功与名。</p>
{{< /doublecode >}}

### HTML Line Breaks

{{< doublecode lang="html" >}}
<p>十步<br>杀<br>一人。</p>
{{< /doublecode >}}

### The Poem Problem

{{< doublecode lang="html" >}}
<p>
十步杀一人，
千里不留行。
事了拂衣去，
深藏功与名。
</p>
{{< /doublecode >}}

**Solution - The HTML pre Element**

{{< doublecode lang="html" >}}
<pre>
十步杀一人，
千里不留行。
事了拂衣去，
深藏功与名。
</pre>
{{< /doublecode >}}

## HTML Styles

{{< lead >}}
The HTML `style` attribute is used to add styles to an element, such as color, font, size, and more.
{{< /lead >}}

### Syntax

```html
<tagname style="property:value;">
```

### Backgroud Color

{{< doublecode lang="html" >}}
<div style="background-color:powderblue;">
    <h1>可见页面内容背景色</h1>
    <p>浅蓝色</p>
</div>
{{< /doublecode >}}

{{< alert style="info">}}
**注意**  
因防止全局渲染，因此将 `body` 元素换成 `div` 元素。
{{< /alert >}}

{{< doublecode lang="html" >}}
<body>
    <h1 style="background-color:powderblue;">标题背景为浅蓝</h1>
    <p style="background-color:tomato;">段落背景为番茄色</p>
</body>
{{< /doublecode >}}

### Text Color

{{< doublecode lang="html" >}}
<h1 style="color:blue;">字体蓝色</h1>
<p style="color:red">字体红色</p>
{{< /doublecode >}}

### Fonts

{{< doublecode lang="html" >}}
<h1 style="font-family:Simsun;">宋体</h1>
<p style="font-family:FangSong">仿宋</p>
{{< /doublecode >}}

### Text Size

{{< doublecode lang="html" >}}
<h1 style="font-size:300%">字体放大 3 倍</h1>
<p style="font-size:160%">放大 1.6 倍</p>
{{< /doublecode >}}

### Text Alignment

{{< doublecode lang="html" >}}
<h1 style="text-align:center">居中</h1>
<p style="text-align:right">居右</p>
{{< /doublecode >}}

## HTML Formatting

{{< lead >}}
HTML contains several elements for defining text with a special meaning.
{{< /lead >}}

### HTML `<b>` and `<strong>` Elements

{{< doublecode lang="html" >}}
<b>粗体，没有额外的重要性</b>
<hr>
<strong>重要性文本，以粗体显示</strong>
{{< /doublecode >}}

### HTML `<i>` and `<em>` Elements

{{< doublecode lang="html" >}}
<i>斜体，通常使用在技术术语，或其他语言的短语、思想等</i>
<hr>
<em>强调文本，以斜体显示</em>
{{< /doublecode >}}


### HTML `<small>` Elements

{{< doublecode lang="html" >}}
<small>更小的文本</small>
{{< /doublecode >}}

### HTML `<mark>` Elements

{{< doublecode lang="html" >}}
<p><mark>标记</mark>或<mark>突出显示</mark>的文本</p>
{{< /doublecode >}}


### HTML `<del>` Element

{{< doublecode lang="html" >}}
<p>即将要<del>删除</del>的文本</p>
{{< /doublecode >}}

### THML `<ins>` Element

{{< doublecode lang="html" >}}
<p>即将要<ins>插入</ins>的文本</p>
{{< /doublecode >}}

### THML `<sub>` Element

{{< doublecode lang="html" >}}
<p>H<sub>2</sub>O</p>
{{< /doublecode >}}

### HTML `<sup>` Element

{{< doublecode lang="html" >}}
<p>脚注<sup>[1]</sup></p>
{{< /doublecode >}}

## HTML Quotation and Citation Element

### HTML `<blockquote>` For Quotations

{{< doublecode lang="html" >}}
<p>引用自《卖油翁》：</p>
<blockquote cite="https://zh.m.wikisource.org/zh-hans/%E8%B3%A3%E6%B2%B9%E7%BF%81">
我亦无他，惟手熟尔。
</blockquote>
{{< /doublecode >}}

### HTML `<q>` for Short Quotations

{{< doublecode lang="html" >}}
<p>鲁迅说过：<q>愿中国青年都摆脱冷气，只是向上走，不必听自暴自弃者流的话。</q></p>
{{< /doublecode >}}


### HTML `<abbr>` for Abbreviations

{{< doublecode lang="html" >}}
<p><abbr title="此缩写的描述信息">缩写</abbr>，将鼠标悬停在上面会显示描述信息。</p>
{{< /doublecode >}}

### HTML `<address>` for Contact

{{< doublecode lang="html" >}}
<address>
剑十三<br>
shisanshiji.com<br>
中国
</address>
{{< /doublecode >}}

### HTML `<cite>` for Work Title

{{< doublecode lang="html" >}}
<p><cite>齐白石虾图</cite> 由齐白石于 1954 年创作。
<p><cite>肖申克的救赎</cite> 由弗兰克·德拉邦特导演于 1994 年上映。
{{< /doublecode >}}

### HTML `<bdo>` for Bi-Derectional Override

{{< doublecode lang="html" >}}
<bdo dir="rtl">这个文本会从右往左写</bdo>
{{< /doublecode >}}

## THML Comments

{{< lead >}}
HTML comments are not displayed in the browser, but they can help document your HTML source code.
{{< /lead >}}

### Hide Content

{{< doublecode lang="html" >}}
<p>十三时记</p>
<!-- <p>这里是被注释掉的段落</p> -->
<p>无他，惟手熟尔。</p>
{{< /doublecode >}}

{{< doublecode lang="html" >}}
<p>十三时记<p>
<!--
<p>这里是被注释掉的段落和一张图片：</p>
<img border="0" src="/img/favicon.png" alt="Trulli">
-->
<p>无他，惟手熟尔。</p>
{{< /doublecode >}}

### Hide Inline Content

{{< doublecode lang="html" >}}
<p>你看不到<!-- 注释 -->这两个字</p>
{{< /doublecode >}}


## HTML Colors

### Colors

{{< doublecode lang="html" >}}
<p style="color:Tomato;">番茄色</p>
<p style="color:DodgerBlue;">道奇蓝</p>
<p style="color:MediumSeaGreen;">暗海藻绿</p>
{{< /doublecode >}}

### RGB

{{< doublecode lang="html" >}}
<p style="background-color:rgb(60, 179, 113);">rgb(60, 179, 113)</p>
<p style="background-color:rgb(255, 165, 0);">rgb(255, 165, 0)</p>
<p style="background-color:rgb(106, 90, 205);">rgb(106, 90, 205)</p>
<p style="background-color:rgb(60, 60, 60);">rgb(60, 60, 60)</p>
<p style="background-color:rgb(100, 100, 100);">rgb(100, 100, 100)</p>
<p style="background-color:rgb(140, 140, 140);">rgb(140, 140, 140)</p>
<p style="background-color:rgb(180, 180, 180);">rgb(180, 180, 180)</p>
<p style="background-color:rgb(220, 220, 220);">rgb(220, 220, 220)</p>
<p style="background-color:rgba(255, 99, 71, 0);">rgba(255, 99, 71, 0)</p>
<p style="background-color:rgba(255, 99, 71, 0.2);">rgba(255, 99, 71, 0.2)</p>
<p style="background-color:rgba(255, 99, 71, 0.4);">rgba(255, 99, 71, 0.4)</p>
<p style="background-color:rgba(255, 99, 71, 0.6);">rgba(255, 99, 71, 0.6)</p>
<p style="background-color:rgba(255, 99, 71, 0.8);">rgba(255, 99, 71, 0.8)</p>
<p style="background-color:rgba(255, 99, 71, 1);">rgba(255, 99, 71, 1)</p>
{{< /doublecode >}}

### HEX

{{< doublecode lang="html" >}}
<p style="background-color:#3cb371;">#3cb371</p>
<p style="background-color:#ffa500;">#ffa500</p>
<p style="background-color:#6a5acd;">#6a5acd</p>
<p style="background-color:#404040;">#404040</p>
<p style="background-color:#686868;">#686868</p>
<p style="background-color:#a0a0a0;">#a0a0a0</p>
<p style="background-color:#bebebe;">#bebebe</p>
<p style="background-color:#dcdcdc;">#dcdcdc</p>
<p style="background-color:#f8f8f8;">#f8f8f8</p>
{{< /doublecode >}}

### HSL

{{< doublecode lang="html" >}}
<p>调整饱和度：</p>
<p style="background-color:hsl(0, 100%, 50%);">hsl(0, 100%, 50%)</p>
<p style="background-color:hsl(0, 80%, 50%);">hsl(0, 80%, 50%)</p>
<p style="background-color:hsl(0, 60%, 50%);">hsl(0, 60%, 50%)</p>
<p style="background-color:hsl(0, 40%, 50%);">hsl(0, 40%, 50%)</p>
<p style="background-color:hsl(0, 20%, 50%);">hsl(0, 20%, 50%)</p>
<p style="background-color:hsl(0, 0%, 50%);">hsl(0, 0%, 50%)</p>
<p>调整亮度：</p>
<p style="background-color:hsl(0, 100%, 0%);">hsl(0, 100%, 0%)</p>
<p style="background-color:hsl(0, 100%, 25%);">hsl(0, 100%, 25%)</p>
<p style="background-color:hsl(0, 100%, 50%);">hsl(0, 100%, 50%)</p>
<p style="background-color:hsl(0, 100%, 75%);">hsl(0, 100%, 75%)</p>
<p style="background-color:hsl(0, 100%, 90%);">hsl(0, 100%, 90%)</p>
<p style="background-color:hsl(0, 100%, 100%);">hsl(0, 100%, 100%)</p>
<p>灰色阴影：</p>
<p style="background-color:hsl(0, 0%, 20%);">hsl(0, 0%, 20%)</p>
<p style="background-color:hsl(0, 0%, 30%);">hsl(0, 0%, 30%)</p>
<p style="background-color:hsl(0, 0%, 40%);">hsl(0, 0%, 40%)</p>
<p style="background-color:hsl(0, 0%, 60%);">hsl(0, 0%, 60%)</p>
<p style="background-color:hsl(0, 0%, 70%);">hsl(0, 0%, 70%)</p>
<p style="background-color:hsl(0, 0%, 90%);">hsl(0, 0%, 90%)</p>
<p>HSLA：</p>
<p style="background-color:hsla(9, 100%, 64%, 0);">hsla(9, 100%, 64%, 0)</p>
<p style="background-color:hsla(9, 100%, 64%, 0.2);">hsla(9, 100%, 64%, 0.2)</p>
<p style="background-color:hsla(9, 100%, 64%, 0.4);">hsla(9, 100%, 64%, 0.4)</p>
<p style="background-color:hsla(9, 100%, 64%, 0.6);">hsla(9, 100%, 64%, 0.6)</p>
<p style="background-color:hsla(9, 100%, 64%, 0.8);">hsla(9, 100%, 64%, 0.8)</p>
<p style="background-color:hsla(9, 100%, 64%, 1);">hsla(9, 100%, 64%, 1)</p>
{{< /doublecode >}}

## HTML Links

{{< lead >}}
Links are found in nearly all web pages. Links allow users to click their way from page to page.
{{< /lead >}}

### Links

#### The target Attribute

The `target` arrtibute can have one of the following values:
- `_self` - Default. Opens the document in the same window/tab as it was clicked
- `_blank` - Opens the document in a new window or tab
- `_parent` - Opens the document in the parent frame
- `_top` - Opens the document in the full body of the window

{{< doublecode lang="html" >}}
<a href="https://www.google.com/" target="_blank">十三时记</a>
{{< /doublecode >}}

#### Use an Image as a Link

{{< doublecode lang="html" >}}
<a href="https://www.google.com/" target="_blank">
<img src="/img/favicon.png" alt="主宰" style="width:42px;height:42px;">
</a>
{{< /doublecode >}}

#### Link to an Email Address

{{< doublecode lang="html" >}}
<a href="mailto:jugg.gao@qq.com">点击发送邮件</a>
{{< /doublecode >}}

#### Button as a Link

{{< doublecode lang="html" >}}
<button onclick="location='https://www.google.com'">谷歌搜索</button>
{{< /doublecode >}}

#### Link Titles

{{< doublecode lang="html" >}}
<a href="https://www.google.com" title="点击去往谷歌搜索">谷歌搜索</a>
{{< /doublecode >}}

### Link Colors

{{< doublecode lang="html" >}}
<iframe src="/learn-notes/html/code/link_colors.html" name="link_colors" style="border:none;" height="30px" width="100%" title="Link Colors"></iframe>
{{< /doublecode >}}

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      a:link {
        color: green;
        background-color: transparent;
        text-decoration: none;
      }
      a:visited {
        color: pink;
        background-color: transparent;
        text-decoration: none;
      }
      a:hover {
        color: red;
        background-color: transparent;
        text-decoration: underline;
      }
      a:active {
        color: yellow;
        background-color: transparent;
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <a href="https://www.google.com" target="_blank">谷歌搜索</a>
  </body>
</html>
```

### Link Buttons

{{< doublecode lang="html" >}}
<iframe src="/learn-notes/html/code/link_buttons.html" name="link_colors" style="border:none;" height="80px" width="100%" title="Link Colors"></iframe>
{{< /doublecode >}}

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      a:link,
      a:visited {
        background-color: #f44336;
        color: white;
        padding: 15px 25px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
      }

      a:hover,
      a:active {
        background-color: red;
      }
    </style>
  </head>
  <body>
    <a href="https://www.google.com" target="_blank">谷歌搜索</a>
  </body>
</html>
```

### Link Bookmarks

{{< doublecode lang="html" >}}
<a href="#links">跳转至 Links</a>
{{< /doublecode >}}

## HTML Images

### Width and Height, or Style?

However, we suggest using the `style` attribute. It prevents styles sheets from changing the size of images:

{{< doublecode lang="html" >}}
<iframe src="/learn-notes/html/code/image_style.html" name="link_colors" style="border:none;" height="160px" width="100%" title="Link Colors"></iframe>
{{< /doublecode >}}

```html
<!DOCTYPE html>
</html>
<head>
  <style>
    img {
      width: 100%;
    }
  </style>
</head>
<body>
  <img src="/img/favicon.png" alt="主宰" width="64" height="64">
  <img src="/img/favicon.png" alt="主宰" style="width:64px;height:64px;">
</body>
<html>
```

### Image Floating

{{< doublecode lang="html" >}}
<p><img src="/img/favicon.png" alt="主宰" style="float:right;width:42px;height:42px;">主宰</p>

<p><img src="/img/favicon.png" alt="主宰" style="float:left;width:42px;height:42px;">主宰</p>
{{< /doublecode >}}

### Image Map

- `rect` - defines a rectangular region
- `circle` - defines a circular region
- `poly` - defines a polygonal region
- `default` - defines the entire region

{{< doublecode lang="html" >}}
<img src="/img/workplace.jpg" alt="Workplace" usemap="#workmap">

<map name="workmap">
  <area shape="rect" coords="34,44,270,350" alt="Computer" href="https://en.wikipedia.org/wiki/Computer" target="_blank">
  <area shape="rect" coords="290,172,333,250" alt="Phone" href="https://en.wikipedia.org/wiki/Mobile_phone" target="_blank">
  <area shape="circle" coords="337,300,44" alt="Coffee" href="https://en.wikipedia.org/wiki/Coffee" target="_blank">
</map>
{{< /doublecode >}}


{{< doublecode lang="html" >}}
<img src="/img/frenchfood.jpg" alt="French Food" usemap="#foodmap" width="450" height="675">

<map name="foodmap">
  <area shape="poly" coords="140,121,181,116,204,160,204,222,191,270,140,329,85,355,58,352,37,322,40,259,103,161,128,147" href="https://en.wikipedia.org/wiki/Croissant" target="_blank">
</map>
{{< /doublecode >}}

{{< doublecode lang="html" >}}
<img src="/img/workplace.jpg" alt="Workplace" usemap="#workplace" width="400" height="379">

<map name="workplace">
  <area shape="circle" coords="337,300,44" onclick="myFunction()">
</map>

<script>
function myFunction() {
  alert("You clicked the coffee cup!");
}
</script>
{{< /doublecode >}}

### Backgroud Images