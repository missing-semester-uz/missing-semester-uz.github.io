---
layout: page
title: "2026-yilgi ma’ruzalar"
description: >
  Missing Semester 2026 ma’ruza matnlari va videolari
permalink: /2026/
phony: true
---

<ul class="double-spaced">
  {% assign lectures = site['2026'] | sort: 'date' %}
  {% for lecture in lectures %}
    {% if lecture.phony != true %}
      <li>
        <strong>{{ lecture.date | date: '%-m/%-d' }}</strong>:
        {% if lecture.ready %}
          <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% elsif lecture.noclass %}
          {{ lecture.title }} [no class]
        {% else %}
          {{ lecture.title }} [coming soon]
        {% endif %}
        {% if lecture.details %}
          <br>
          ({{ lecture.details }})
        {% endif %}
      </li>
    {% endif %}
  {% endfor %}
</ul>

Ma’ruzalarning videoyozuvlari <a href="https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L">YouTube'ga</a> joylangan.

# MIT'dan tashqarida

Ushbu resurslardan MIT talabalaridan boshqalar ham foydalanishi uchun uni turli joylarga tarqatdik. Postlar va muhokamalarni quyidagi manzillarda topishingiz mumkin.

- [Hacker News](https://news.ycombinator.com/item?id=47124171)
- [Lobsters](https://lobste.rs/s/q4ykw7/missing_semester_your_cs_education_2026)
- [r/learnprogramming](https://www.reddit.com/r/learnprogramming/comments/1r93yk6/the_missing_semester_of_your_cs_education_2026/)
- [X](https://x.com/anishathalye/status/2024521145777848588)
- [Bluesky](https://bsky.app/profile/jonhoo.eu/post/3mfa2bhyuj22i)
- [Mastodon](https://fosstodon.org/@jonhoo/116098318361854057)
- [LinkedIn](https://www.linkedin.com/posts/anishathalye_i-returned-to-mit-during-iap-january-term-activity-7430285026933522433-Ehr9)
- [YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L)

# Minnatdorchilik

Biz ma’ruza videolarini yozib olishimiz uchun imkoniyat yaratgan Eleyn Mello va [MIT Open Learning](https://openlearning.mit.edu/)ga; va kursning 2026-yilgi variantini [SIPB IAP 2026](https://sipb.mit.edu/iap/) doirasida qo'llab quvvatlagani uchun Luis Turino / [SIPB](https://sipb.mit.edu/)'larga minnatdorchilik bildiramiz.
