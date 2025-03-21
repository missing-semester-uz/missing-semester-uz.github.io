---
layout: page
title: Kompyuter fanlari ta’limingizning yetishmayotgan semestri
nositetitle: true
---

Kompyuter fanlaridagi darslarda operatsion tizimlardan tortib mashinaviy o‘rganishgacha bo‘lgan ilg‘or mavzularni o‘rgatiladi. Biroq, talabalarga juda muhim bo‘lgan, lekin ko‘pincha mustaqil o‘rganish uchun qoldiriladigan mavzular har bor. Bu kerakli vositalaridan samarali foydalanish ko‘nikmasidir. Kurs davomida sizga buyruqlar qatorini mukammal o‘zlashtirishni, kuchli matn muharriridan foydalanishni, versiyalarni boshqarish tizimlarining ilg‘or xususiyatlarini qo‘llashni va boshqa ko‘p narsalarni o‘rgatamiz!

Talabalar bu vositalarga o‘qish davomida yuzlab soat (ish faoliyatlari davomida esa minglab soat) vaqt sarflaydilar. Shuning uchun bu tajriba iloji boricha qulay va samarali bo‘lishi kerak. Ushbu vositalarni mukammal o‘zlashtirish nafaqat ularni moslashtirish uchun kamroq vaqt sarflashga, balki ilgari hal qilib bo‘lmaydigan, murakkab tuyulgan muammolarni yechish imkonini ham beradi.

[Bu havolada](/about/) kurs ortidagi sabablar bilan tanishishingiz mumkin.

{% comment %}
# Ro‘yxatdan o‘tish

[Ro‘yxatdan o‘tish shakli](https://forms.gle/TD1KnwCSV52qexVt9)ni to‘ldirib, IAP 2020 kursiga yoziling
{% endcomment %}

# Ma’ruzalar jadvali

{% comment %}
**Ma’ruza**: 35-225 xona, soat 14:00 dan 15:00 gacha<br>
**Qabul vaqti**: 32-G9 dam olish xonasi, soat 15:00 dan 16:00 gacha (har kuni, ma’ruzadan keyin)
{% endcomment %}

<ul>
{% assign lectures = site['2020'] | sort: 'date' %}
{% for lecture in lectures %}
    {% if lecture.phony != true %}
        <li>
        <strong>{{ lecture.date | date: '%-m/%d/%y' }}</strong>:
        {% if lecture.ready %}
            <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% else %}
            {{ lecture.title }} {% if lecture.noclass %}[no class]{% endif %}
        {% endif %}
        </li>
    {% endif %}
{% endfor %}
</ul>

Ma’ruzalarning videoyozuvlari [YouTube'ga](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J) joylangan.

# Kurs haqida

**O‘qituvchilar**: Bu kursni [Anish](https://www.anishathalye.com/), [Jon](https://thesquareplanet.com/) va [Jose](http://josejg.com/) hamkorlikda olib boradi.<br>
**Savollar**: Bizga [missing-semester@mit.edu](mailto:missing-semester@mit.edu) elektron manzili orqali murojaat qilishingiz mumkin.

# MIT'dan tashqarida

Ushbu resurslardan MIT talabalaridan boshqalar ham foydalanishi uchun uni turli joylarga tarqatdik. Postlar va muhokamalarni quyidagi manzillarda topishingiz mumkin.

 - [Hacker News](https://news.ycombinator.com/item?id=22226380)
 - [Lobsters](https://lobste.rs/s/ti1k98/missing_semester_your_cs_education_mit)
 - [/r/learnprogramming](https://www.reddit.com/r/learnprogramming/comments/eyagda/the_missing_semester_of_your_cs_education_mit/)
 - [/r/programming](https://www.reddit.com/r/programming/comments/eyagcd/the_missing_semester_of_your_cs_education_mit/)
 - [Twitter](https://x.com/jonhoo/status/1224383452591509507)
 - [YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J)

 {% comment %}
Ba’zi qo‘shimcha URL manzillar:

- https://news.ycombinator.com/item?id=27154577
- https://news.ycombinator.com/item?id=34934216
- https://www.reddit.com/r/learnprogramming/comments/nca1v3/mit_the_missing_semester_of_your_cs_education/
- https://www.reddit.com/r/compsci/comments/eyywv8/the_missing_semester_of_your_cs_education_from_mit/
- https://www.reddit.com/r/programming/comments/io7nq3/the_missing_semester_of_your_cs_education_mit/
- https://x.com/MIT_CSAIL/status/1349766980413263873
- https://x.com/MIT_CSAIL/status/1481676163491659780
- https://x.com/MIT_CSAIL/status/1581313961093484545
{% endcomment %}

# Tarjimalar

- [Xitoycha (Osonlashtirilgan)](https://missing-semester-cn.github.io/)
- [Xitoycha (An’anaviy)](https://missing-semester-zh-hant.github.io/)
- [Yaponcha](https://missing-semester-jp.github.io/)
- [Koreyscha](https://missing-semester-kr.github.io/)
- [Portugalcha](https://missing-semester-pt.github.io/)
- [Ruscha](https://missing-semester-rus.github.io/)
- [Serbcha](https://netboxify.com/missing-semester/)
- [Ispancha](https://missing-semester-esp.github.io/)
- [Turkcha](https://missing-semester-tr.github.io/)
- [Vetnamcha](https://missing-semester-vn.github.io/)
- [Arabcha](https://missing-semester-ar.github.io/)
- [Italyancha](https://missing-semester-it.github.io/)
- [Forscha](https://missing-semester-fa.github.io/)
- [Nemischa](https://missing-semester-de.github.io/)
- [Bengalcha](https://missing-semester-bn.github.io/)

Eslatma: bu hamjamiyat a’zolari tomonidan qilingan tarjimalar. Biz ularni tekshirib chiqmaganmiz.

Ushbu kursning ma’ruza matnlarini tarjima qildingizmi? Uni ro‘yxatga qo‘shishimiz uchun [pull request](https://github.com/missing-semester/missing-semester/pulls) yuborishingizni so‘raymiz!

## Minnatdorchilik

Biz ma’ruza videolarini yozib olishimiz uchun imkoniyat yaratgan Eleyn Mello, Jim Keyn va [MIT Open Learning](https://openlearning.mit.edu/)ga; audio-vizual jihozlar uchun Entoni Zolnik va [MIT AeroAstro](https://aeroastro.mit.edu/)ga; hamda ushbu kursni qo‘llab-quvvatlagani uchun Brandi Adams va [MIT EECS](https://www.eecs.mit.edu/)ga o‘z minnatdorchiligimizni bildiramiz.

---

<div class="small center">
<p><a href="https://github.com/missing-semester-uz/missing-semester-uz.github.io">Manba kodi</a>.</p>
<p>CC BY-NC-SA litsenziyasi asosida tarqatildi.</p>
<p>Hissa qo‘shish va tarjima bo‘yicha ko‘rsatmalarni ko‘rish uchun <a href="/license/">shu yerga</a> qarang.</p>
</div>