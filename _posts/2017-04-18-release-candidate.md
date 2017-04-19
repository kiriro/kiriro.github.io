---
layout: post
title: kiri mk.III - release candidate
excerpt: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2017-04-18
tags: [test, jekyll, github-page]
comments: true
pinned: true
image:
  feature: 72334d3ejw8erj8oskqopj2050050glw.jpg
---

# Kiri mk.III - release candidate

Hello.

This is **Kiri**. 测试一下第一个jekyll页面。

![Avator Image]({{ site.url }}/assets/img/72334d3ejw8erj8oskqopj2050050glw.jpg)
{: .pull-right}

等有了内容再正式开写吧。

### Blockquotes

> Lorem ipsum dolor sit amet, test link adipiscing elit. Nullam dignissim convallis est. Quisque aliquam.

## List Types

### Ordered Lists

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

### Unordered Lists

* Item one
* Item two
* Item three

## Tables

| Header 1 | Header 2 | Header 3 |
|:--------|:-------:|--------:|
| cell 1   | cell 2   | cell 3   |
| cell 4   | cell 5   | cell 6   |
|----
| cell 1   | cell 2   | cell 3   |
| cell 4   | cell 5   | cell 6   |
|=====
| Foot 1   | Foot 2   | Foot 3   |

## Code Snippets

{% highlight css %}
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
{% endhighlight %}

## Buttons

Make any link standout more when applying the `.btn` class.

{% highlight html %}
<a href="#" class="btn btn-success">Success Button</a>
{% endhighlight %}

<div markdown="0"><a href="#" class="btn">Primary Button</a></div>
<div markdown="0"><a href="#" class="btn btn-success">Success Button</a></div>
<div markdown="0"><a href="#" class="btn btn-warning">Warning Button</a></div>
<div markdown="0"><a href="#" class="btn btn-danger">Danger Button</a></div>
<div markdown="0"><a href="#" class="btn btn-info">Info Button</a></div>

## Notices

**Watch out!** You can also add notices by appending `{: .notice}` to a paragraph.
{: .notice}