+++
title = "HTML 教程笔记"
description = ""
weight = 1
+++

{{< doublecode lang="html" >}}
{{< /doublecode >}}

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
    <h1>十三时记</h1>
    <p>无他，惟手熟尔</p>
</div>
{{< /doublecode >}}

{{< alert style="info">}}
**注意**  
因防止全局渲染，因此将 `body` 元素换成 `div` 元素。
{{< /alert >}}