---
title: オンラインでホストされている手順
permalink: index.html
layout: home
ms.openlocfilehash: 7997ca57a9f8cff5e7f4a03c7de0eaf0c9e436ba
ms.sourcegitcommit: 2ef4734dc1cea1e600b967de23c86716f8fd779e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2022
ms.locfileid: "138095642"
---
# <a name="content-directory"></a>コンテンツ ディレクトリ

必要な演習ファイルは、[こちらからダウンロード](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/archive/master.zip)できます

各演習へのハイパーリンクを以下に一覧表示します。

## <a name="exercise"></a>演習

{% assign Exercise = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %}
| モジュール | 演習 |
| --- | --- | 
{% for activity in Exercise %}| {{ activity.Exercise.module }} | [{{ activity.Exercise.title }}{% if activity.Exercise.type %} - {{ activity.Exercise.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

