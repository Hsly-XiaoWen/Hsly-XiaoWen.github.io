---
layout: page
title: About
description: 海上捞盐
keywords: XiaoWen
comments: true
menu: 关于
permalink: /about/
---

海上捞盐，少许文艺气息的程序员。

心若猛虎，细吻蔷薇。

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
