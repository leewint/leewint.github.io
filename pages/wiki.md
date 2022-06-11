---
layout: wiki
title: Wiki
description: 人越学越觉得自己无知
keywords: 维基, Wiki
comments: false
copyright: false
menu: 维基
permalink: /wiki/
---

> 记多少命令和快捷键会让脑袋爆炸呢？

#### Notion Share

* [每年读完](https://leewint.notion.site/22cb813e41d44738bc827c147b44691f)
* [读过的书（豆瓣自动备份）](https://leewint.notion.site/becb541367ad401282e3ce0e8228d674?v=2c9c24609dbc41df9f97d5a476b002d8)
* [面试话术](https://leewint.notion.site/22d60c1fd61f485aa0f48c86254d5123)
* [我的人生信念](https://leewint.notion.site/fa424dbb2375475e8cb36922b8139c5d)

{% case site.components.wiki.view %}

{% when 'list' %}

<ul class="listing">
{% for wiki in site.wiki %}
{% if wiki.title != "Wiki Template" and wiki.topmost == true %}
<li class="listing-item"><a href="{{ site.url }}{{ wiki.url }}"><span class="top-most-flag">[置顶]</span>{{ wiki.title }}</a></li>
{% endif %}
{% endfor %}
{% for wiki in site.wiki %}
{% if wiki.title != "Wiki Template" and wiki.topmost != true %}
<li class="listing-item"><a href="{{ site.url }}{{ wiki.url }}">{{ wiki.title }}<span style="font-size:12px;color:red;font-style:italic;">{%if wiki.layout == 'mindmap' %}  mindmap{% endif %}</span></a></li>
{% endif %}
{% endfor %}
</ul>

{% when 'cate' %}

{% assign item_grouped = site.wiki | where_exp: 'item', 'item.title != "Wiki Template"' | group_by: 'cate1' | sort: 'name' %}
{% for group in item_grouped %}
###### {{ group.name }}
{% assign cate_items = group.items | sort: 'title' %}
{% assign item2_grouped = cate_items | group_by: 'cate2' | sort: 'name' %}
{% for sub_group in item2_grouped %}
{% assign name_len = sub_group.name | size %}
{% if name_len > 0 -%}
<i>{{ sub_group.name }}: <sup>{{ sub_group.items | size }}</sup></i>
{%- endif -%}
{%- assign item_count = sub_group.items | size -%}
{%- assign item_index = 0 -%}
{%- for item in sub_group.items -%}
{%- assign item_index = item_index | plus: 1 -%}
<a href="{{ site.url }}{{ item.url }}" style="display:inline-block;padding:0.5em">{{ item.title }}<span style="font-size:12px;color:red;font-style:italic;">{%if item.layout == 'mindmap' %}  mindmap{% endif %}</span></a>{%- if item_index < item_count -%}<span> <b>·</b></span>{%- endif -%}
{%- endfor -%}
{% endfor %}
{% endfor %}

{% endcase %}
