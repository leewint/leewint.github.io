---
layout: page
title: About
description: 未来无人知晓，一起有知有行。
keywords: Apelove
comments: true
menu: 关于
permalink: /about/
---

## Introduction

Read-Think-Write-Code

## Find Me

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
<li>
邮箱：<a href="mailto:apelove@foxmail.com" alt="apelove@foxmail.com">apelove@foxmail.com</a>
</li>
<li>
微信公众号：<br />
<img style="height:192px;width:192px;border:1px solid lightgrey;" src="{{ assets_base_url }}/assets/images/qrcode.jpg" alt="猿恋的公众号" />
</li>
</ul>

## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
