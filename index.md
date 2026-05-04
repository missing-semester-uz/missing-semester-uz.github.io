---
layout: page
title: Kompyuter fanlari ta’limingizning yetishmayotgan semestri
description: >
  Sizni samaraliroq kompyuter mutaxassisi va dasturchiga aylantiruvchi kuchli vositalarni o‘rganing.
# subtitle: IAP 2026
subtitle: "2026"
nositetitle: true
---

Kompyuter fanlaridagi darslarda operatsion tizimlardan tortib mashinaviy o‘rganishgacha bo‘lgan ilg‘or mavzularni o‘rgatiladi. Biroq, talabalarga juda muhim bo‘lgan, lekin ko‘pincha mustaqil o‘rganish uchun qoldiriladigan mavzular har bor. Bu kerakli vositalaridan samarali foydalanish ko‘nikmasidir. Kurs davomida sizga buyruqlar qatorini mukammal o‘zlashtirishni, kuchli matn muharriridan foydalanishni, versiyalarni boshqarish tizimlarining ilg‘or xususiyatlarini qo‘llashni va boshqa ko‘p narsalarni o‘rgatamiz!

Talabalar bu vositalarga o‘qish davomida yuzlab soat (ish faoliyatlari davomida esa minglab soat) vaqt sarflaydilar. Shuning uchun bu tajriba iloji boricha qulay va samarali bo‘lishi kerak. Ushbu vositalarni mukammal o‘zlashtirish nafaqat ularni moslashtirish uchun kamroq vaqt sarflashga, balki ilgari hal qilib bo‘lmaydigan, murakkab tuyulgan muammolarni yechish imkonini ham beradi.

Hozirgi kunda sun’iy intellektga (SI) asoslangan va u bilan takomillashtirilgan vositalar ish jarayonini tubdan o‘zgartirmoqda. Agar bu vositalardan o‘rinli va kamchiliklarini anglagan holda foydalanilsa, ular mutaxassislarga sezilarli naf keltirishi mumkin. Shu bois ular haqida amaliy bilimlarni o‘rganishga arziydi. SI ko‘p tarmoqli texnologiya bo‘lgani uchun unga alohida ma’ruza ajratilmagan. Buning o‘rniga biz har bir ma’ruzaning o‘ziga sun’iy intellektga oid eng so‘nggi vositalar va usullardan foydalanishni bevosita kiritib bordik.

[Bu havolada](/about/) kurs ortidagi sabablar bilan tanishishingiz mumkin.

# Ma’ruzalar jadvali

{% comment %}
**Ma’ruza**: [35-225](https://whereis.mit.edu/?go=35), 13:30--14:30 (_istisno_: Juma 16-yanvar kuni 15:00--16:00)<br>
**Muhokama**: [OSSU Discord](https://ossu.dev/#community) (`#missing-semester-forum` kanali va `#missing-semester` chati)
{% endcomment %}

<ul>
{% assign lectures = site['2026'] | sort: 'date' %}
{% for lecture in lectures %}
    {% if lecture.phony != true %}
        <li>
        <strong>{{ lecture.date | date: '%-m/%-d/%y' }}</strong>:
        {% if lecture.ready %}
            <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% else %}
            {{ lecture.title }} {% if lecture.noclass %}[no class]{% endif %}
        {% endif %}
        </li>
    {% endif %}
{% endfor %}
</ul>

## Special topics from previous years

Biz yoritadigan mavzular yildan yilga o‘zgarib turadi. Yillar davomida o‘tilgan mavzularimizning to‘liq ro‘yxati bilan tanishmoqchi bo‘lgan talabalar uchun avvalgi yillarda o‘tilgan, lekin 2026-yilda o‘tilmagan mavzularni alohida keltirib o‘tamiz.

{% comment %} pop to remove default "posts" collection {% endcomment %}
{% assign sorted_collections = site.collections | sort: 'label' | pop | reverse %}
<ul>
{% for collection in sorted_collections %}
    {% assign grouped_lectures = site[collection.label] | group_by: 'date' | sort: 'name' %}
    {% for group in grouped_lectures %}
        {% assign sorted_lectures = group.items | sort: 'order' %}
        {% for lecture in sorted_lectures %}
            {% if lecture.special == true %}
                <li>
                    <strong>{{ lecture.date | date: '%-m/%-d/%y' }}</strong>:
                    <a href="{{ lecture.url }}">{{ lecture.title }}</a>
                </li>
            {% endif %}
        {% endfor %}
    {% endfor %}
{% endfor %}
</ul>

{% comment %}
Ma’ruza videolari MIT talabalariga darsdan so‘ng darhol (Panopto orqali) taqdim etiladi. Tizimda cheklov mavjud bo‘lib, unga ko‘ra faqat MIT Kerberos hisobiga ega bo‘lganlargina to‘liq videodan foydalana oladi. Biz ma’ruza videolarini tahrirlab, YouTube’ga yuklash ustida ishlayapmiz. Bir nechtasi allaqachon yuklangan; qolganlarini esa fevral o‘rtalarigacha yuklab bo‘lishni rejalashtiryapmiz.

Agar 2026-yil yanvarigacha kuta olmasangiz, ko‘plab o‘xshash mavzularni o‘z ichiga olgan [kursning avvalgi dasturi](/2020/)dagi ma’ruzalarni ham ko‘rib chiqishingiz mumkin.
{% endcomment %}

# Kurs haqida

**O‘qituvchilar**: Bu kursni [Anish](https://www.anishathalye.com/), [Jon](https://thesquareplanet.com/) va [Jose](http://josejg.com/) hamkorlikda olib boradi.<br>
**Savollar**: [missing-semester@mit.edu](mailto:missing-semester@mit.edu) emailiga orqali murojaat qiling.
**Muhokama**: [OSSU Discord](https://ossu.dev/#community) (`#missing-semester-forum` kanali va `#missing-semester` chati)

## Special topics from previous years

# MIT'dan tashqarida

Ushbu resurslardan MIT talabalaridan boshqalar ham foydalanishi uchun uni turli joylarga tarqatdik. Postlar va muhokamalarni quyidagi manzillarda topishingiz mumkin.

 - Hacker News ([2026](https://news.ycombinator.com/item?id=47124171), [2020](https://news.ycombinator.com/item?id=22226380), [2019](https://news.ycombinator.com/item?id=19078281))
 - Lobsters ([2026](https://lobste.rs/s/q4ykw7/missing_semester_your_cs_education_2026), [2020](https://lobste.rs/s/ti1k98/missing_semester_your_cs_education_mit), [2019](https://lobste.rs/s/h6157x/mit_hacker_tools_lecture_series_on))
 - r/learnprogramming ([2026](https://www.reddit.com/r/learnprogramming/comments/1r93yk6/the_missing_semester_of_your_cs_education_2026/), [2020](https://www.reddit.com/r/learnprogramming/comments/eyagda/the_missing_semester_of_your_cs_education_mit/), [2019](https://www.reddit.com/r/learnprogramming/comments/an42uu/mit_hacker_tools_a_lecture_series_on_programmer/))
 - r/programming ([2020](https://www.reddit.com/r/programming/comments/eyagcd/the_missing_semester_of_your_cs_education_mit/), [2019](https://www.reddit.com/r/programming/comments/an3xki/mit_hacker_tools_a_lecture_series_on_programmer/))
 - X ([2026](https://x.com/anishathalye/status/2024521145777848588), [2020](https://twitter.com/jonhoo/status/1224383452591509507), [2019](https://x.com/jonhoo/status/1090323977766137858))
 - Bluesky ([2026](https://bsky.app/profile/jonhoo.eu/post/3mfa2bhyuj22i))
 - Mastodon ([2026](https://fosstodon.org/@jonhoo/116098318361854057))
 - LinkedIn ([2026](https://www.linkedin.com/posts/anishathalye_i-returned-to-mit-during-iap-january-term-activity-7430285026933522433-Ehr9))
 - YouTube ([2026](https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L), [2020](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J), [2019](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuiujH1lpn8cA9dsyulbYRv))

# Tarjimalar

{% comment %} alifbo tartibida joylashtirilgan {% endcomment %}

- [Arabcha](https://missing-semester-ar.github.io/)
- [Bengalcha](https://missing-semester-bn.github.io/)
- [Xitoycha (Osonlashtirilgan)](https://missing-semester-cn.github.io/)
- [Xitoycha (An’anaviy, Taivan))](https://missing-semester-tw.github.io/)
- [Nemischa](https://missing-semester-de.github.io/)
- [Italyancha](https://missing-semester-it.github.io/)
- [Yaponcha](https://missing-semester-jp.github.io/)
- [Kannada](https://missing-semester-kn.github.io/)
- [Koreyscha](https://missing-semester-kr.github.io/)
- [Forscha](https://missing-semester-fa.github.io/)
- [Portugalcha](https://missing-semester-pt.github.io/)
- [Ruscha](https://missing-semester-rus.github.io/)
- [Serbcha](https://netboxify.com/missing-semester/)
- [Ispancha](https://missing-semester-esp.github.io/)
- [Shvedcha](https://den-saknade-terminen.l10n.se/)
- [Tailandcha](https://missing-semester-th.github.io/)
- [Turkcha](https://missing-semester-tr.github.io/)
- [Vetnamcha](https://missing-semester-vn.github.io/)

Eslatma: bu hamjamiyat a’zolari tomonidan qilingan tarjimalar. Biz ularni tekshirib chiqmaganmiz.

Ushbu kursning ma’ruza matnlarini tarjima qildingizmi? Uni ro‘yxatga qo‘shishimiz uchun [pull request](https://github.com/missing-semester/missing-semester/pulls) yuborishingizni so‘raymiz!

## Minnatdorchilik

{% comment %}
2026 yilgisi. Avvalgi minnatdorchiliklar o'z sahifalarida
{% endcomment %}

Biz ma’ruza videolarini yozib olishimiz uchun imkoniyat yaratgan Eleyn Mello va [MIT Open Learning](https://openlearning.mit.edu/)ga; va kursning 2026-yilgi variantini [SIPB IAP 2026](https://sipb.mit.edu/iap/) doirasida qo'llab quvvatlagani uchun Luis Turino / [SIPB](https://sipb.mit.edu/)'larga minnatdorchilik bildiramiz.

---

<div class="small center">
<p><a href="https://github.com/missing-semester-uz/missing-semester-uz.github.io">Manba kodi</a>.</p>
<p>CC BY-NC-SA litsenziyasi asosida tarqatildi.</p>
<p>Hissa qo‘shish va tarjima bo‘yicha ko‘rsatmalarni ko‘rish uchun <a href="/license/">shu yerga</a> qarang.</p>
</div>