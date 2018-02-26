---
layout: cndefault
permalink: /cn/
title: 无求备斋笔记
---

<article>
<blockquote><p>
机器人、计算机视觉研究者，编程爱好者，字体排印爱好者，金州勇士及斯蒂芬・库里球迷。
</p></blockquote>
</article>

<p style="text-align:left;margin-top:1.2em;margin-bottom:0;">
<b>文章 </b>
| 按<a href="/cnarchive#tags">标签</a>浏览 
<!--<span style="float:right;">按<a href="/cnarchive#tags">标签</a>浏览</span>-->
</p>
---

<article>
<table>
{% for post in site.categories.cn %}
<tr id="blog-table">
<td>{{ post.date | date: "%Y-%m-%d" }}</td>
<td><a class="post-list-item" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></td>
</tr>
{% endfor %}
</table>
</article>
<hr>
<p>博文<a href="/cnarchive">归档</a></p>
