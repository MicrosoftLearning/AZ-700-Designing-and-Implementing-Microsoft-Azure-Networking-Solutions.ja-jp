---
title: オンラインでホストされる手順
permalink: index.html
layout: home
---

# コンテンツ ディレクトリ

必要な演習ファイルは、[こちらからダウンロード](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/archive/master.zip)できます

デモは下にスクロールしてください。 

## 演習

{% assign Exercise = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %}
| モジュール | 演習 |
| --- | --- | 
{% for activity in Exercise  %}| {{ activity.Exercise.module }} | [{{ activity.Exercise.title }}{% if activity.Exercise.type %} - {{ activity.Exercise.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

<!-- ## Demonstrations (under construction)

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Demonstration |
| --- | 
{% for activity in demos  %}| [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
-->


