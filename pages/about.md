---
layout: page
title: About
description: 睿智与美丽并存
keywords: Liuyao
comments: true
menu: 关于
permalink: /about/
---

我是LiuYao，一直在学习中的程序媛。

努力成为自己期望的样子。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
