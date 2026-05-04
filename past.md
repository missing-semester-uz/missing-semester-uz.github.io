---
layout: page
title: Avvalgi versiyalar
description: >
  Missing Semester'ning barcha eski versiyalari.
---

{% comment %} birlamchi “posts” pop-up'ni olib tashlash {% endcomment %}
{% assign sorted_collections = site.collections | sort: 'label' | pop | reverse %}
<ul>
{% for collection in sorted_collections %}
    <li><a href="/{{ collection.label }}/">{{ collection.label }}</a></li>
{% endfor %}
</ul>

Har yilgi ma’ruzalar to‘plami yangi va avvalgilaridan mustaqildir. Shu sababli, o‘qishni materiallarning eng so‘nggi versiyasidan boshlashni tavsiya qilamiz. Yildan yilga yoritiladigan mavzularda farqlar bo‘lgani uchun, ushbu kursning avvalgi versiyalariga oid qaydlar va videolarni ham saytda saqlab qo‘yganmiz.
