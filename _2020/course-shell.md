---
layout: lecture
title: "Kurs mazmuni + shell"
date: 2020-01-13
ready: true
video:
  aspect: 56.25
  id: Z56Jmr9Z34Q
---

# Motivatsiya

Kompyuter mutaxassislari sifatida, kompyuterlarning takroriy vazifalarni bajarishda samarali ekanligini bilamiz. Biroq, ko‘pincha bu qoida nafaqat dasturlarimiz bajarishi kerak bo‘lgan hisob-kitoblarga, balki kompyuterdan _foydalanishimizga_ ham tegishli ekanligini unutib qo‘yamiz. Qo‘limizda kompyuter bilan bog‘liq har qanday muammoni yechishda unumdorligimizni oshirish va murakkab masalalarni hal qilish imkonini beruvchi ko‘plab vositalar mavjud. Ammo ko‘pchiligimiz bu vositalarning faqat kichik qismidan foydalanamiz; biz faqat kundalik ishlarimizni bajarish uchun zarur bo‘lgan bir nechta sehrli buyruqlarni yodlab olganmiz, xolos. Qiyinchilikka duch kelganimizda esa internetdan topgan buyruqlarni tushunmasdan ishlatib qo‘ya qolamiz.

Ushbu kurs aynan shu muammoni hal qilishga qaratilgan.

Biz sizga o‘zingiz bilgan vositalardan samarali foydalanishni o‘rgatishni, vositalar to‘plamingizga yangi vositalarni qo‘shishni va umid qilamizki, sizda mustaqil ravishda ko‘proq vositalarni o‘rganish (va balki yaratish) uchun qiziqish uyg‘otishni xohlaymiz. Bu kompyuter fanlari o‘quv dasturlarining aksariyatida yetishmayotgan semestr deb hisoblaymiz.

# Kurs tuzilmasi

Kurs 11 ta bir soatlik ma’ruzadan iborat bo‘lib, har biri [muayyan mavzu](/2020/)ga bag‘ishlangan. Ma’ruzalar asosan bir-biridan mustaqil, ammo avvalgi ma’ruzalar mazmunini bilasiz degan fikr asosida tuzilgan. Onlayn ma’ruza matnlari mavjud, ammo darsda yoritilgan ko‘p materiallar (masalan, ko‘rsatib bersak) matnlarda bo‘lmasligi mumkin. Biz ma’ruzalarni yozib olamiz va internetda e’lon qilamiz.

Biz atigi 11 ta bir soatlik ma’ruza davomida ko‘p ma’lumotlarni qamrab olishga harakat qilmoqdamiz, shu bois ma’ruzalar ancha ma’lumotga zich. Kursni o‘z tezligingizga o'rganishingiz uchun har bir ma’ruzada asosiy mavzularni o‘z ichiga olgan mashqlar to‘plami mavjud. Har bir ma’ruzadan so‘ng, savollaringizga javob berish uchun qabul soatlarini o‘tkazamiz. Agar siz darsga onlayn qatnashayotgan bo‘lsangiz, savollaringizni [missing-semester@mit.edu](mailto:missing-semester@mit.edu) manziliga yuborishingiz mumkin.

Vaqtimiz cheklanganligi sababli, barcha vositalarni to‘liq kurs kabi batafsil o‘rgana olmaymiz. Iloji boricha, biror vosita yoki mavzuni chuqurroq o‘rganishingiz uchun sizni kerakli manbalarga yo‘naltirishga harakat qilamiz. Ammo agar biror narsa sizni juda qiziqtirib qolsa, bizga murojaat qilishdan va qo‘shimcha ma’lumotlar so‘rashdan tortinmang!

# 1-mavzu: Shell

## Shell nima?

Bugungi kunda kompyuterlarga buyruq berish uchun turli xil interfeyslar mavjud; chiroyli grafik interfeyslar, ovozli interfeyslar, hatto AR/VR ham hamma joyda uchraydi. 80% holatlar uchun ajoyib, ammo ular ko‘pincha imkoniyatlaringizni jiddiy ravishda cheklaydi — siz mavjud bo‘lmagan tugmani bosa olmaysiz yoki dasturlashtirilmagan ovozli buyruqni bera olmaysiz. Kompyuteringiz taqdim etadigan vositalardan to‘liq foydalanish uchun biz eski usulga qaytishimiz va matnli interfeysga murojaat qilishimiz kerak: Shell.

Siz foydalanadigan deyarli barcha platformalarda u yoki bu ko‘rinishdagi shell mavjud va ko‘pchiligida hatto bir nechta shell'lar bor. Ular ba'zi tafsilotlarda farq qilishi mumkin, ammo aslida barchasi taxminan bir xil: ular dasturlarni ishga tushirish, ularga ma’lumot kiritish va natijalarini muayyan tizimli usulda tekshirish imkonini beradi.

Ushbu ma’ruza "Bourne Again SHell" yoki qisqacha "bash" deb ataladigan shell asosida o'tiladi. Bu eng keng qo‘llaniladigan shell'lardan biri bo‘lib, uning sintaksisi boshqa ko‘plab shell'lardagi sintaksisga o‘xshaydi. Shell _so‘rov_ini (buyruqlarni kiritadigan joyni) ochish uchun avval _terminal_ kerak bo‘ladi. Qurilmangizda terminal allaqachon o‘rnatilgan yoki uni osonlik bilan o‘rnatishingiz mumkin.

## Shell'dan foydalanish

Terminalingizni ishga tushirganingizda, odatda quyidagiga o‘xshash _so‘rov satrini_ ko‘rasiz:

```console
missing:~$ 
```

Bu shell'ning asosiy matn interfeysidir. U sizga `missing` nomli mashinada ekanligingizni va "joriy ish katalogingiz", ya’ni hozirgi joylashgan o‘rningiz ’~’ ("uy" so‘zining qisqartmasi) ekanligini ko‘rsatadi. `$` belgisi bosh foydalanuvchi emasligingizni bildiradi (bu haqda keyinroq batafsil to‘xtalamiz). Ushbu so‘rovda siz _buyruq_ kiritishingiz mumkin, so‘ng uni shell talqin qiladi. Eng sodda buyruq - dasturni ishga tushirish:

```console
missing:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
missing:~$ 
```

Bu yerda biz `date` dasturini ishga tushirdik. Bu dastur (kutilganidek) joriy sana va vaqtni chop etadi. Shundan so‘ng terminal bizdan boshqa buyruqni kiritishimizni so‘raydi. Biz buyruqlarni _argumentlar_ bilan ham bajarishimiz mumkin:

```console
missing:~$ echo hello
hello
```

Bu holda, biz shell'qa `echo` dasturini `hello` argumenti bilan bajarishni buyurdik. `Echo` dasturi shunchaki o‘ziga berilgan argumentlarni chop etadi. Shell buyruqni bo‘sh joylar bo‘yicha ajratib tahlil qiladi, so‘ngra birinchi so‘z bilan ko‘rsatilgan dasturni ishga tushiradi va har bir keyingi so‘zni dastur foydalanishi mumkin bo‘lgan argument sifatida taqdim etadi. Agar siz bo‘sh joylar yoki boshqa maxsus belgilarni o‘z ichiga olgan argumentni (masalan, "My Photos" nomli katalog) taqdim etmoqchi bo‘lsangiz, argumentni `'` yoki `"` (`"My Photos"`) bilan qo‘shtirnoq ichiga olishingiz yoki faqat tegishli belgilarni `\` (`My\ Photos`) bilan saqlashingiz mumkin.

Lekin shell `date` yoki `echo` dasturlarini qanday topishni qayerdan biladi? Aslida, shell Python yoki Ruby kabi dasturlash muhitidir, shuning uchun unda o‘zgaruvchilar, shartli operatorlar, sikllar va funksiyalar mavjud (keyingi ma’ruzada ko‘rib chiqamiz!). Shell'da buyruqlarni bajarganda, siz aslida shell talqin qiladigan kichik bir kod yozayotgan bo‘lasiz. Agar shell'ga o‘zining dasturlash kalit so‘zlaridan biriga mos kelmaydigan buyruqni bajarish topshirilsa, u `$PATH` deb nomlangan _muhit o‘zgaruvchisi_ga murojaat qiladi. Bu o‘zgaruvchi buyruq berilganda shell qaysi kataloglardan dasturlarni qidirishi kerakligini ko‘rsatib beradi.


```console
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

`echo` buyrug‘ini berganimizda, shell `echo` dasturini bajarish kerakligini aniqlaydi va so‘ng `$PATH` dagi ikki nuqta bilan ajratilgan kataloglar ro‘yxati orqali shu nomdagi faylni qidiradi. Fayl topilgach, uni ishga tushiradi (faylni _bajarilishi mumkin_ deb hisoblagan holda; bu haqda keyinroq batafsil to‘xtalamiz). Berilgan dastur nomi uchun qaysi fayl bajarilishini `which` dasturi yordamida aniqlay olamiz. Shuningdek, bajarmoqchi bo‘lgan faylga to‘liq _yo‘l_ ko‘rsatish orqali `$PATH` ni butunlay chetlab o‘tishimiz mumkin.

## Shell'da harakatlanish

Shell'da yo‘l - bu kataloglarning ajratilgan ro‘yxati bo‘lib, Linux va macOS tizimlarida `/`
bilan, Windows tizimida esa `\` bilan ajratiladi. Linux va macOS’da `/` yo‘li fayl tizimining "boshi" hisoblanadi, uning ostida barcha kataloglar va fayllar joylashgan. Windows’da esa har bir disk bo‘limi uchun alohida bosh mavjud (masalan, `C:\`). Ushbu kursda biz asosan Linux fayl tizimidan foydalanamiz. `/` bilan boshlanadigan yo‘l _mutlaq_ yo‘l deb ataladi. Boshqa har qanday yo‘l esa _nisbiy_ yo‘l hisoblanadi. Nisbiy yo‘llar joriy ishchi katalogga nisbatan aniqlanadi. Joriy ishchi katalogni `pwd` buyrug‘i bilan ko‘rish va `cd` buyrug‘i bilan o‘zgartirish mumkin. Yo‘lda `.` belgisi joriy katalogni, `..` esa uning yuqori katalogini bildiradi.

```console
missing:~$ pwd
/home/missing
missing:~$ cd /home
missing:/home$ pwd
/home
missing:/home$ cd ..
missing:/$ pwd
/
missing:/$ cd ./home
missing:/home$ pwd
/home
missing:/home$ cd missing
missing:~$ pwd
/home/missing
missing:~$ ../../bin/echo hello
hello
```

E’tibor bering, shell so‘rovimiz bizni joriy ishchi katalogimiz haqida doimiy ravishda xabardor qilib turdi. Siz so‘rov qatoringizni turli foydali ma’lumotlarni ko‘rsatadigan qilib sozlashingiz mumkin. Biz bu haqda keyingi ma’ruzada batafsil to‘xtalamiz.

Odatda, dasturni ishga tushirganimizda, agar biz boshqacha ko‘rsatma bermasak, u joriy katalogda ishlaydi. Masalan, u ko‘pincha fayllarni shu yerdan qidiradi va zarur bo‘lsa, yangi fayllarni ham shu yerda yaratadi.

Biror katalogda nimalar mavjudligini ko‘rish uchun biz `ls` buyrug‘idan foydalanamiz:

```console
missing:~$ ls
missing:~$ cd ..
missing:/home$ ls
missing
missing:/home$ cd ..
missing:/$ ls
bin
boot
dev
etc
home
...
```

Agar birinchi argument sifatida katalog ko‘rsatilmagan bo‘lsa, `ls` buyrug‘i joriy katalog tarkibini chop etadi. Ko‘pchilik buyruqlar o‘z faoliyatini o‘zgartirish uchun ’-’ bilan boshlanadigan bayroqlar va parametrlarni (qiymatli bayroqlar) qabul qiladi. Odatda, dasturni ’-h’ yoki ’--help’ bayrog‘i bilan ishga tushirish mavjud bayroqlar va parametrlar haqida ma’lumot beruvchi yordam matnini chop etadi. Misol uchun, ’ls --help’ buyrug‘i bizga quyidagilarni ko‘rsatadi:

```
  -l                         uzun ro‘yxat formati
```

```console
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```

Bu bizga har bir fayl yoki katalog haqida ko‘proq ma’lumot beradi. Avvalo, satr boshidagi `d` belgisi `missing` katalog ekanligini bildiradi. Keyin uchta guruhga uchtadan belgilar (`rwx`) keladi. Bular fayl egasi (`missing`), egalik qiluvchi guruh (`users`) va boshqalarning tegishli element bo‘yicha ruxsatlarini ko‘rsatadi. `-` belgisi ma’lum ruxsat berilmaganini anglatadi. Yuqorida faqat katalog egasiga `missing` katalogini o‘zgartirish (`w`) ruxsati berilgan (ya’ni, u fayllar qo‘shish yoki o‘chirish imkoniyatiga ega). Katalogga kirish uchun katalogning (va uning yuqori kataloglarida) "qidirish" (ya’ni "bajarish": `x`) ruxsatlariga ega bo‘lishi kerak. Uning ichini ko‘rish uchun esa foydalanuvchi ushbu katalogda o‘qish (`r`) ruxsatlariga ega bo‘lishi lozim. Fayllar uchun ruxsatlar ham yuqoridagidek. E’tibor bering, `/bin` dagi deyarli barcha fayllarda oxirgi guruh, ya’ni "boshqalar" uchun `x` ruxsati o‘rnatilgan, shu sababli har kim bu dasturlarni ishlata oladi.

Ushbu bosqichda bilishingiz kerak bo‘lgan yana ba’zi foydali dasturlar quyidagilar: `mv` (faylni qayta nomlash yoki ko‘chirish), `cp` (faylni nusxalash) va `mkdir` (yangi katalog yaratish).

Agar biror dasturning argumentlari, kirish va chiqish ma’lumotlari yoki umuman qanday ishlashi haqida _batafsil_ ma’lumot olishni istasangiz, `man` dasturini ishlatib ko‘ring. U dastur nomini argument sifatida qabul qiladi va sizga shu dasturning _qo‘llanma sahifasini_ ko‘rsatadi. Chiqish uchun `q` tugmasini bosing.

```console
missing:~$ man ls
```

## Dasturlarni ulash

Shell'da dasturlar bilan bog‘liq ikkita asosiy "oqimlar" mavjud: kirish oqimi va chiqish oqimi. Dastur ma’lumot o‘qimoqchi bo‘lganda, u kirish oqimidan o‘qiydi va biror narsani chop etganda, chiqish oqimiga yozadi. Odatda, dasturning kirishi ham, chiqishi ham sizning terminalingizdir. Ya’ni, klaviaturangiz kirish vositasi, ekraningiz esa chiqish vositasi sifatida xizmat qiladi. Ammo biz bu oqimlarni boshqacha yo‘naltirish imkoniyatiga ham egamiz!

Qayta yo‘naltirishning eng oddiy shakli `< fayl` va `> fayl` buyruqlaridir. Ular dasturning kirish va chiqish oqimlarini mos ravishda faylga yo‘naltirish imkonini beradi:

```console
missing:~$ echo hello > hello.txt
missing:~$ cat hello.txt
hello
missing:~$ cat < hello.txt
hello
missing:~$ cat < hello.txt > hello2.txt
missing:~$ cat hello2.txt
hello
```

Yuqoridagi misolda ko‘rsatilganidek, `cat` fayllarni birlashtiruvchi dasturdir. Unga argument sifatida fayl nomlari berilsa, har bir faylning mazmunini ketma-ket ravishda o‘zining chiqish oqimiga chop etadi. Biroq `cat`ga hech qanday argument berilmasa, u o‘zining kirish oqimidan olingan ma’lumotlarni chiqish oqimiga chop etadi (yuqoridagi uchinchi misol).

Shuningdek, faylga ma’lumot qo‘shish uchun `>>` belgisidan foydalanishingiz mumkin. Kiritish/chiqarishni qayta yo‘naltirishning bu turi ayniqsa _pipeline'lar_ yordamida yanada samaraliroq ishlaydi. `|` operatori sizga dasturlarni "zanjir" shaklida bog‘lash imkonini beradi, bunda bir dasturning chiqishi boshqasining kirishi bo‘ladi:

```console
missing:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```

Ma’lumotlarni qayta ishlash bo‘yicha ma’ruzada pipeline'lardan qanday foydalanish haqida batafsil to‘xtalamiz.

## Ko‘p qirrali va kuchli vosita

Unix'ga o‘xshash tizimlarning aksariyatida bitta foydalanuvchi alohida ahamiyatga ega: "root". Uni yuqoridagi fayllar ro‘yxatida ko‘rgan bo‘lishingiz mumkin. root foydalanuvchisi (deyarli) barcha kirish cheklovlaridan ustun bo‘lib, tizimdagi har qanday faylni yaratishi, o‘qishi, yangilashi va o‘chirishi mumkin. Biroq, tizimingizga odatda root foydalanuvchi sifatida kirmaysiz, chunki biror narsani tasodifan buzib qo‘yish juda oson. Buning o‘rniga `sudo` buyrug‘idan foydalanasiz. Nomidan anglaganingizdek, u sizga "su" (ya’ni "super user" yoki "root") sifatida biror narsani "do" (bajarish) imkonini beradi. Ruxsat rad etilganlik haqida xato chiqqanida, odatda root sifatida biror narsa qilishingiz kerak bo‘ladi. Ammo haqiqatan ham shunday qilishga ikki karra ishonchingiz komil bo'lishi kerak!

root huquqiga ega bo‘lishingiz talab etiladigan ishlardan biri - bu `/sys` katalogida joylashgan `sysfs` fayl tizimiga yozishdir. `sysfs` bir qator kernel parametrlarini fayllar ko‘rinishida taqdim etadi, shu tufayli siz maxsus vositalardan foydalanmasdan kernelni tezda qayta sozlashingiz mumkin. **E’tibor bering: Windows yoki macOS tizimlarida sysfs mavjud emas.**

Masalan, noutbukingiz ekranining yorqinligi `brightness` nomli fayl orqali boshqariladi, u quyidagi joyda joylashgan:

```
/sys/class/backlight
```

Ushbu faylga qiymat yozish orqali ekran yorqinligini o‘zgartirishimiz mumkin. Sizga birinchi keladigan fikr:

```console
$ sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
/sys/class/backlight/thinkpad_screen/brightness
$ cd /sys/class/backlight/thinkpad_screen
$ sudo echo 3 > brightness
An error occurred while redirecting file 'brightness'
open: Permission denied
```

Bu xato sizga qiziq tuyulishi mumkin. Axir, buyruqni `sudo` bilan ishga tushirdik-ku! Bu shell haqida bilish kerak bo‘lgan muhim narsa. `|`, `>` va `<` kabi operatsiyalar _shell tomonidan_ bajariladi, alohida dastur tomonidan emas. `echo` va unga o‘xshash buyruqlar `|` haqida "bilishmaydi". Ular nima bo‘lishidan qat’i nazar o‘z kirishlari o‘qiydi va chiqishlariga yozadi. Yuqoridagi holatda, _shell_ (xuddi foydalanuvchingiz kabi qayd qilingan) `sudo echo`ning chiqishini qabul qilishdan oldin brightness fayliga yozishga harakat qiladi, lekin shell root sifatida ishlamagani uchun bunga ruxsat berilmaydi. Bundan foydalanib, biz quyidagicha yechim qila olamiz:

```console
$ echo 3 | sudo tee brightness
```

`/sys` faylini yozish uchun `tee` dasturi ochganligi va u `root` huquqlari bilan ishlayotganligi sababli, barcha ruxsatlar to‘g‘ri ishlaydi. Siz `/sys` orqali turli qiziqarli va foydali narsalarni boshqarishingiz mumkin. Masalan, tizimning turli chiroqli indikatorlari (LED) holatini nazorat qila olasiz (sizdagi yo‘l boshqacha bo‘lishi mumkin):

```console
$ echo 1 | sudo tee /sys/class/leds/input6::scrolllock/brightness
```

# Keyingi qadamlar

Shu yerga kelib, siz asosiy vazifalarni bajarish uchun shell'da bemalol harakat qila olasiz. Kerakli fayllarni topish va ko‘pchilik dasturlarning asosiy funksiyalaridan foydalanish uchun erkin harakatlana olishingiz lozim. Keyingi ma’ruzada biz shell va mavjud ko‘plab qulay buyruq qatori dasturlaridan foydalanib, murakkab vazifalarni qanday bajarish va avtomatlashtirish haqida gaplashamiz.

# Mashqlar

Ushbu kursdagi barcha darslarda bir qator mashqlar bor. Ba’zilarida sizga aniq vazifa beriladi, boshqalari esa ochiq turdagi, masalan, "X va Y dasturlaridan foydalanib ko‘ring" kabi. Biz bu mashqlarni albatta bajarib ko‘rishga undaymiz.

Biz mashqlarning yechimlarini yozmaganmiz. Agar biror narsada qiynalsangiz, shu paytgacha nimalarni sinab ko‘rganingizni bizga xat sifatida yuboring va biz sizga yordam berishga harakat qilamiz.

 1. Ushbu kurs uchun Bash yoki ZSH kabi Unix shell'laridan foydalanishingiz lozim. Linux yoki macOS tizimida bo‘lsangiz, qo‘shimcha hech narsa qilishingiz shart emas. Windows tizimida ishlayotgan bo‘lsangiz, cmd.exe yoki PowerShell ishlatmayotganingizga ishonch hosil qilishingiz kerak. Unix uslubidagi buyruq satri vositalaridan foydalanish uchun [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) yoki Linux virtual mashinasidan foydalanishingiz mumkin. To'g'ri shellni ishga tushirayotganingizni tekshirish uchun `echo $SHELL` buyrug‘ini kiritishingiz mumkin. Agar natija `/bin/bash` yoki `/usr/bin/zsh` bo‘lsa, demak to‘g‘ri dasturni ishga tushiryapsiz.
 1. `/tmp` katalogi ostida `missing` nomli yangi katalog yarating.
 1. `touch` dasturini qarab ko'ring. `man` sizning do'stingiz.
 1. `missing` katalogida `semester` nomli yangi fayl yaratish uchun `touch` buyrug‘idan foydalaning.
 1. Quyidagilarni ushbu faylga bir qatordan yozing:
    ```
    #!/bin/sh
    curl --head --silent https://missing.csail.mit.edu
    ```
    Birinchi qatorni ishlatish qiyin bo‘lishi mumkin. Bash tilida `#` belgisi izohni boshlaydi, `!` belgisi esa hatto qo‘sh tirnoq (`"`) ichida ham o‘ziga xos ma’noga ega. Buni bilish foydali. Bash bir tirnoqli satrlarni (`'`) boshqacha ko‘rib chiqadi: ular bu holatda yechim bo‘ladi. Batafsil ma’lumot olish uchun Bash [iqtiboslash](https://www.gnu.org/software/bash/manual/html_node/Quoting.html) qo‘llanmasi sahifasiga murojaat qiling.

 1. Faylni ishga tushirishga urinib ko‘ring, ya’ni shell'ingizga skript qatorida (`./semester`) kiriting va Enter tugmasini bosing. Nima uchun ishlamasligini tushunish uchun `ls` buyrug‘ining natijasini ko‘zdan kechiring (maslahat: faylning ruxsatlariga e’tibor bering).
 1. Buyruqni `sh` interpretatorini aniq ko‘rsatib ishga tushiring va unga birinchi argument sifatida `semester` faylini bering, ya’ni `sh semester` shaklida. Nima uchun bu usul ishladi, lekin `./semester` ishlamadi?
 1. `chmod` dasturini qarab ko‘ring (masalan, `man chmod` buyrug‘idan foydalaning).
 1. `./semester` buyrug‘ini ishga tushirish uchun `sh semester` yozish o‘rniga `chmod` dan foydalaning. Sizning shell'ingiz faylni `sh` yordamida talqin qilish kerakligini qanday biladi? Bu haqda ko‘proq ma’lumot olish uchun [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) qatori haqidagi sahifani ko‘ring.
 1. `|` va `>` belgilaridan foydalanib, `semester` tomonidan chiqarilgan "last modified" (oxirgi o‘zgartirilgan) sana ma’lumotini bosh katalogingizda joylashgan `last-modified.txt` nomli faylga yozing.
 1. `/sys` katalogidan noutbukingiz batareyasining quvvat darajasini yoki kompyuteringiz protsessorining haroratini o‘qib beradigan buyruq yozing. Eslatma: agar siz macOS foydalanuvchisi bo‘lsangiz, operatsion tizimingizda sysfs mavjud emas, shuning uchun bu topshiriqni bajarmasligingiz mumkin.
