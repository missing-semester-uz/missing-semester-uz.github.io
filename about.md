---
layout: lecture
title: "Kursni nega o‘tayotganimiz haqida"
---

An’anaviy kompyuter fanlari ta’limi davomida siz operatsion tizimlardan tortib dasturlash tillarigacha, mashinaviy o‘rganishgacha bo‘lgan ilg‘or mavzularni o‘rgatadigan ko‘plab kurslarda qatnashasiz. Biroq, ko‘plab oliygohlarda kamdan-kam o‘tiladigan va talabalarga mustaqil o‘rganish uchun qoldiriladigan muhim bir mavzu bor: bu - kompyuter ekotizimi savodxonligi.

Yillar davomida biz Massachusets texnologiya institutida bir nechta kurslarni o‘qitishga yordamlashdik va ko‘plab talabalarning mavjud bo‘lgan vositalar haqida cheklangan bilimga ega ekanliklarini payqadik. Kompyuterlar takroriy vazifalarni avtomatlashtirish uchun yaratilgan bo‘lsa-da, talabalar ko‘pincha ularni qo‘lda bajaradi yoki versiyalarni boshqarish va matn muharrirlari kabi kuchli vositalardan to‘liq foydalana olmaydi. Eng yaxshi holatda bu samarasizlik va vaqtning behuda sarflanishi bo‘lsa, eng yomon holatda esa ma’lumotlarning yo‘qolishi yoki ayrim vazifalarni bajara olmaslik kabi muammolarga sabab bo‘ladi.

Bu mavzular universitet o‘quv dasturiga kiritilmagan: talabalarga bu vositalardan foydalanish, ayniqsa ulardan samarali foydalanish o‘rgatilmaydi. Shu sababli talabalar _oson_ bo‘lishi kerak bo‘lgan vazifalarni bajarishga ortiqcha vaqt va kuch sarflaydi. Standart kompyuter fanlari o‘quv dasturida talabalar hayotini sezilarli darajada osonlashtirishi mumkin bo‘lgan hisoblash ekotizimi haqidagi muhim mavzular yetishmaydi.

# Kompyuter fanlari ta’limingizning yetishmayotgan semestri

Buni hal qilish maqsadida, biz samarali kompyuter olimi va dasturchi bo‘lish uchun muhim deb hisoblaydigan barcha mavzularni qamrab oluvchi kurs tashkil etyapmiz. Kurs amaliy va pragmatik bo‘lib, siz duch keladigan turli vaziyatlarda darhol qo‘llay oladigan vosita va usullarni amalda o‘rgatadi. Kurs 2020-yil yanvar oyida MITning bir oylik semestr davomida talabalar tomonidan boshqariladigan qisqa kurslar ya’ni "Mustaqil faoliyat davri"da o‘tkaziladi. Ma’ruzalar faqat MIT talabalari uchun ochiq bo‘lsa-da, barcha ma’ruza materiallari va videoyozuvlarini ommaviy taqdim etamiz.

Agar sizga kurs qiziq tuyulsa, unda nimalarni bo‘lishi haqida bir necha aniq misollarni keltirib o‘tamiz.

## shell buyruqlar qatori

Alias'lar, skriptlar va tizimlarni qurish orqali keng tarqalgan va takrorlanadigan vazifalarni qanday avtomatlashtirish haqida. Endi saqlangan joydan buyruqlarni nusxalash va joylashtirish shart emas. "Bu 15 ta buyruqni ketma-ket bajaring" degan gaplar ham o‘tmishda qolishi kerak. "Siz bu narsani ishga tushirishni unutdingiz" yoki "siz bu parametrni kiritishni unutdingiz" degan xatolar ham ketmaydi.

Masalan, o‘z buyruqlar tarixingiz orasidan tezda qidirish vaqtni tejashga yordam beradi. Quyidagi misolda biz `convert` buyrug‘ini shell tarixidan topish bilan bog‘liq bir nechta foydali usullarni ko‘rsatamiz.
<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/history.mp4" type="video/mp4">
</video>

## Versiya nazorati

Versiya nazoratidan _to‘g‘ri_ foydalanish va undan foydalanib o‘zingizni falokatdan asrash, boshqalar bilan hamkorlik qilish hamda muammoli o‘zgarishlarni tezda topish va ajratib olish haqida. Endi "rm -rf; git clone" buyruqlariga hojat yo‘q. Birlashish ziddiyatlari ham kerak emas. Izohga olingan katta kod bloklariga xojat yo‘q. Kodingizni nima buzganini topish uchun bosh qotirmaysiz. "Voy, ishlaydigan kodni o‘chirib yubordikmi?!" degan savollar ham bo‘lmaydi. Hatto boshqalarning loyihalariga pull request’lar orqali hissa qo‘shishni ham o‘rgatamiz!

Quyidagi misolda `git bisect` buyrug‘i yordamida qaysi commit unit testini buzganini aniqlaymiz va keyin uni `git revert` bilan tuzatamiz.
<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/git.mp4" type="video/mp4">
</video>

## Matn tahriri

Buyruqlar qatoridan fayllarni mahalliy va masofaviy tarzda qanday samarali tahrirlash hamda muharrirning ilg‘or xususiyatlaridan qanday foydalanish haqida. Endi fayllarni u yoqdan bu yoqqa ko‘chirish shart emas. Takroriy fayl tahrirlash ham o‘tmishda qoldi.

Quyidagi misolda Vim makrolari eng ajoyib xususiyatlardan biri bo‘lgan ichma-ich joylashgan Vim makrosidan foydalanib, HTML jadvalini CSV formatiga tez va oson o‘tkazish usulini ko‘rib chiqamiz.
<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/vim.mp4" type="video/mp4">
</video>

## Masofaviy qurilmalar

SSH kalitlari va terminal multipleksorlash yordamida masofaviy mashinalar bilan ishlash haqida. Endi bir vaqtning o‘zida ikkita buyruqni bajarish uchun ko‘plab terminallarni ochishga hojat yo‘q. Har safar ulanganingizda parol kiritish shart emas. Internet uzilib qolsa yoki noutbukni qayta ishga tushirishga to‘g‘ri kelganda barcha ma’lumotlaringizni yo‘qotmaysiz.

Quyidagi misolda biz masofaviy serverlardagi sessiyalarni faol saqlash uchun `tmux` dasturidan va tarmoq roumingi hamda uzilishlarni qo‘llab-quvvatlash uchun `mosh` dasturidan foydalanamiz.
<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/ssh.mp4" type="video/mp4">
</video>

## Fayllarni topish

O‘zingizga kerak bo‘lgan fayllarni tezda qanday topish haqida. Endi loyihangizdagi kerakli kodi bor faylni topguncha boshqa fayllarga kirib chiqishingiz shart emas.

Quyidagi misolda biz `fd` yordamida fayllarni va `rg` yordamida kod parchalarini tezda qidiramiz. Shuningdek, `fasd` yordamida so‘nggi yoki tez-tez ishlatiladigan fayl va papkalarni `cd` va `vim` buyruqlari orqali tezda ochishimiz mumkin.
<video autoplay="autoplay" loop="loop" controls muted playsinline  oncontextmenu="return false;"  preload="auto"  class="demo">
  <source src="/static/media/demos/find.mp4" type="video/mp4">
</video>

## Ma’lumotlarni qayta ishlash

Ma’lumotlar va fayllarni buyruqlar qatoridan bevosita qanday tez va oson o‘zgartirish, ko‘rish, tahlil qilish, vizualizatsiya qilish va hisoblash haqida. Endi har safar log fayllaridan nusxa ko‘chirish shart emas. Ma’lumotlar bo‘yicha statistikani qo‘lda hisoblash zarurati yo‘q. Elektron jadvallar yordamida grafik chizishga ham ehtiyoj qolmadi.

## Virtual mashinalar

Yangi operatsion tizimlarni sinab ko‘rish, bir-biriga aloqador bo‘lmagan loyihalarni alohida saqlash va asosiy kompyuteringizni toza hamda tartibli tutish uchun virtual mashinalardan foydalanish haqida. Endi xavfsizlik amaliyotini bajarayotib kompyuteringizni tasodifan ishdan chiqarib qo‘yish xavfi yo‘q. Turli versiyalardagi millionlab tasodifiy o‘rnatilgan dasturiy paketlar muammosi ham bartaraf etiladi.

## Xavfsizlik

Internetda barcha ma'lumotlaringizni ochiqlab yubormagan holda bo‘lish haqida. Endi murakkab talablarga mos keladigan parollarni o‘ylab topish sizning zimmangizda bo‘lmaydi. Himoyalanmagan, ochiq Wi-Fi tarmoqlari muammosi hal bo‘ladi. Shiflanmagan xabarlar yuborish xavfi ham yo‘qoladi.

# Xulosa

Shu va boshqa mavzular 12 ta ma’ruzalar davomida yoritiladi. Har bir ma’ruzada vositalar bilan mustaqil ravishda chuqurroq tanishishingiz uchun mashqlar berilgan. Agar yanvar oyigacha kutolmasangiz, o‘tgan yili IAP davomida o‘tkazilgan [Hacker Tools](https://hacker-tools.github.io/lectures/) ma’ruzalarini ham ko‘rib chiqishingiz mumkin. U shu kursning debochasi bo‘lib, bu yilgi ko‘plab mavzularni qamrab olgan.

Umid qilamizki, yanvar oyida siz bilan virtual yoki shaxsan ko‘rishamiz!

Xakerlikka xush kelibsiz,<br>
Anish, Jose va Jon
