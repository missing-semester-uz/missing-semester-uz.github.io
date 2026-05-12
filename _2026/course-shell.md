---
layout: lecture
title: "Kurs mazmuni + shell"
description: >
  Kurs ortidagi motivatsiya va shell bilan tanishuv.
thumbnail: /static/assets/thumbnails/2026/lec1.png
date: 2026-01-12
ready: true
video:
  aspect: 56.25
  id: MSgoeuMqUmU
---

# Biz kimmiz?

Bu kurs [Anish](https://anish.io/), [Jon](https://thesquareplanet.com/) va [Jose](http://josejg.com/) tomonidan birgalikda o'qitiladi. Barchamiz sobiq MIT talabalarimiz va bu MIT IAP kursini talabalik davrimizda boshlaganmiz. Biz bilan [missing-semester@mit.edu](mailto:missing-semester@mit.edu) manzili orqali bog'lanishingiz mumkin.

Biz bu kursni o'qitganimiz uchun maosh olmaymiz va kursdan hech qanday moddiy foyda ko'rmaymiz. Biz barcha [kurs materiallarini](https://missing.csail.mit.edu/) va [ma'ruza yozuvlarini](https://www.youtube.com/@MissingSemester) bepul tarzda onlayn ommaga taqdim etamiz. Agar ishimizni qo'llab-quvvatlashni istasangiz, buning eng yaxshi yo'li shunchaki kurs haqida boshqalarga aytishdir. Agar siz ushbu kontentni kattaroq guruhlarga o'qitadigan kompaniya, universitet yoki boshqa tashkilot bo'lsangiz, iltimos, bizga o'z tajribangiz/fikrlaringizni elektron pochta orqali yuboring, shunda biz bundan xabardor bo'lamiz :)

# Motivatsiya

Kompyuter mutaxassislari sifatida, kompyuterlar takroriy vazifalarni bajarishda ajoyib yordamchi ekanini bilamiz. Biroq, ko'pincha kompyuterdan _foydalanishimiz_ dasturlarimiz bajarishini xohlagan hisoblashlar kabi muhim ekanligini unutib qo'yamiz. Kompyuter bilan bog'liq har qanday muammo ustida ishlaganda unumdorlikni oshirish va murakkabroq muammolarni hal qilish imkonini beruvchi juda ko'p vositalar doimo qo'l ostimizda. Shunga qaramay, ko'pchiligimiz bu vositalarning faqat kichik bir qismidan foydalanamiz; biz faqat ishimizni bitirish uchun yetarli bo'lgan sehrli so'zlarni yoddan bilamiz va tiqilib qolganimizda internetdan buyruqlarni ko'r-ko'rona nusxalaymiz.

Ushbu kurs buni [hal qilishga](/about/) qaratilgan urinishdir.

Biz sizga o'zingiz bilgan vositalardan qanday qilib maksimal darajada foydalanishni o'rgatmoqchimiz, asboblar qutingizga qo'shish uchun yangi vositalarni ko'rsatmoqchimiz va umid qilamizki, o'zingiz ham ko'proq vositalarni o'rganish (va ehtimol yaratish) uchun sizda ishtiyoq uyg'otamiz. Bizningcha, bu ko'pgina Kompyuter fanlari o'quv dasturida yetishmayotgan semestrdir.

# Kurs tuzilmasi

Kredit olinmaydigan ushbu kurs [muayyan mavzuga](/2026/) qaratilgan to'qqizta 1 soatlik ma'ruzadan iborat. Ma'ruzalar asosan bir-biriga bog'liq emas, lekin semestr o'tgan sari siz oldingi ma'ruzalar mazmuni bilan tanishsiz deb faraz qilamiz. Bizda ma'ruza qaydlari onlayn tarzda mavjud, ammo darsda (masalan, demolar ko'rinishida) qaydlarda bo'lmagan mazmun ham qamrab olinishi mumkin. O'tgan yillardagi kabi, biz ma'ruzalarni yozib olamiz va yozuvlarni [onlayn](https://www.youtube.com/@MissingSemester) joylashtiramiz.

Biz bir nechta 1 soatlik ma'ruzalar davomida ko'p narsalarni qamrab olishga harakat qilyapmiz, shuning uchun ma'ruzalar juda zich. Kontent bilan o'z tezligingizda tanishish uchun sizga bir oz vaqt berish maqsadida, har bir ma'ruza o'zining asosiy nuqtalariga yo'naltiruvchi mashqlar to'plamini o'z ichiga oladi. Biz doimiy "office hours" (qabul soatlari) o'tkazmaymiz, lekin savollaringizni [OSSU Discord](https://ossu.dev/#community) dagi `#missing-semester-forum` kanalida berishni yoki [missing-semester@mit.edu](mailto:missing-semester@mit.edu) manziliga elektron pochta orqali yuborishni tavsiya qilamiz.

Vaqtimiz cheklanganligi sababli, keng ko'lamli sinfda bo'lgani kabi barcha vositalarni bir xil darajadagi tafsilotlar bilan qamrab ololmaymiz. Iloji bo'lsa, vosita yoki mavzuni chuqurroq o'rganish uchun manbalarga yo'naltirishga harakat qilamiz, lekin e'tiboringizni ko'proq jalb qiladigan narsa bo'lsa, biz bilan bog'lanishdan tortinmang!

Va nihoyat, kurs haqida fikr-mulohazangiz bo'lsa, uni bizga [missing-semester@mit.edu](mailto:missing-semester@mit.edu) elektron pochta orqali yuboring.

# 1-mavzu: Shell

{% comment %}
lecturer: Jon
{% endcomment %}

## Shell nima?

Bugungi kunda kompyuterlarga buyruq berish uchun turli interfeyslar mavjud; chiroyli grafik interfeyslar, sotsial interfeyslar, AR/VR va yaqinda: LLMlar. Bular 80% holatlarda ishlaydi, ammo ular ko'pincha sizga ruxsat beradigan narsalar bilan fundamental jihatdan cheklangan — siz mavjud bo'lmagan tugmani bosa olmaysiz yoki dasturlanmagan ovozli buyruqni bera olmaysiz. Kompyuteringiz taqdim etadigan vositalardan to'liq foydalanish uchun biz eskilarga qaytishimiz va matnli interfeysga o'tishimiz kerak: Shell.

Deyarli barcha platformalarda u yoki bu shaklda shell mavjud va ularning ko'pchiligida siz tanlashingiz mumkin bo'lgan bir nechta shelllar bor. Ular tafsilotlarida farq qilishi mumkin bo'lsa-da, ularning barchasi taxminan bir xil: ular dasturlarni ishga tushirish, ularga kirish malumotlarini berish va natijalarini yarim tizimli tarzda tekshirish imkonini beradi.

Shell _so'rov satrini_ (buyruqlarni yozishingiz mumkin bo'lgan joy) ochish uchun avval sizga _terminal_ kerak bo'ladi, bu shellning vizual interfeysi hisoblanadi. Qurilmangiz ehtimol o'rnatilgan terminal bilan kelgan yoki uni juda oson o'rnatishingiz mumkin:

- **Linux:**
  `Ctrl + Alt + T` tugmalarini bosing (aksariyat distributivlarda ishlaydi). Yoki ilovalar menyusidan "Terminal"ni qidirib toping.
- **Windows:**
  `Win + R` tugmalarini bosing, `cmd` yoki `powershell` yozing va Enter tugmasini bosing.
  Yoki Start menyusida "Terminal" yoki "Command Prompt"ni qidiring.
- **macOS:**
  Spotlight-ni ochish uchun `Cmd + Space` tugmalarini bosing, "Terminal" deb yozing va Enter tugmasini bosing.
  Yoki uni Ilovalar (Applications) → Utilitlar (Utilities) → Terminal qismidan toping.

Linux va macOS tizimlarida bu odatda Bourne Again SHell yoki qisqacha "bash"ni ochadi. Bu eng ko'p ishlatiladigan shelllardan biri bo'lib, uning sintaksisi boshqa ko'plab shelllarda ko'radiganga o'xshaydi. Windows tizimida, siz qaysi buyruqni ishga tushirganingizga qarab "batch" yoki "powershell" shelllari sizni kutib oladi. Bular Windowsga xos bo'lib, ushbu kursda biz ularga e'tibor qaratmaymiz, garchi unda biz o'rgatadigan narsalarning aksariyati uchun analoglar mavjud. Buning o'rniga siz [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) yoki Linux virtual mashinasidan foydalanganingiz ma'qul.

Boshqa shelllar ham mavjud, ko'pincha bashga qaraganda ergonomik jihatdan yaxshilangan (fish va zsh eng keng tarqalganlari qatoriga kiradi). Garchi bular juda mashhur bo'lsa-da (barcha o'qituvchilar undan foydalanadi), ular bash kabi keng tarqalmagan va o'xshash tushunchalarga tayanadi, shuning uchun bu ma'ruzada biz ularga to'xtalib o'tmaymiz.

## Nima uchun bu sizni qiziqtirishi kerak?

Shell nafaqat "kliklash"dan (odatda) ancha tezroq, balki u biron bir grafik dasturda osongina topa olmaydigan ifoda kuchi bilan ham keladi. Ko'rib turganimizdek, shell deyarli har qanday vazifani avtomatlashtirish uchun dasturlarni ijodiy usullar bilan _birlashtirish_ imkoniyatini beradi.

Shellni yaxshi tushunish, shuningdek, ochiq manbali dasturiy ta'minot dunyosida harakatlanish (ular ko'pincha shell talab qiladigan o'rnatish ko'rsatmalari bilan keladi), dasturiy loyihalaringiz uchun uzluksiz integratsiya (CI) o'rnatish ([Kod sifati ma'ruzasi](/2026/code-quality/)da tavsiflanganidek) va boshqa dasturlar ishlamay qolganda xatolarni tuzatish uchun juda foydali.

## Shell ichida harakatlanish

Terminalni ishga tushirganingizda, siz ko'pincha quyidagiga o'xshash _so'rov satrini_ ko'rasiz:

```console
missing:~$
```

Bu shellning asosiy matnli interfeysi. Bu sizga `missing` nomli mashinada ekanligingizni va "joriy ishchi katalogingiz" yoki siz hozirda qayerda joylashganligingiz `~` ("home" yoki asosiy sahifaning qisqartmasi) ekanligini aytadi. `$` belgisi siz root foydalanuvchisi emasligingizni bildiradi (bu haqda keyinroq). Ushbu so'rov satriga _buyruq_ yozishingiz mumkin va uni shell interpretatsiya qiladi. Eng oddiy buyruq bu dasturni ishga tushirishdir:

```console
missing:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
missing:~$
```

Bu yerda biz joriy sana va vaqtni chop etadigan (kutilganidek) `date` dasturini ishga tushirdik. Shundan so'ng shell bizdan bajarish uchun boshqa buyruq so'raydi. Shuningdek, biz buyruqni _argumentlar_ bilan ishga tushirishimiz mumkin:

```console
missing:~$ echo hello
hello
```

Bu holatda biz shellga `echo` dasturini `hello` argumenti bilan ishga tushirishni buyurdik. `echo` dasturi shunchaki uning argumentlarini ekranga chiqarib beradi. Shell buyruqni probellar orqali qismlarga ajratadi va keyin birinchi so'z ko'rsatgan dasturni ishga tushiradi, qolgan har bir so'zni dastur kirishi mumkin bo'lgan argument sifatida ta'minlaydi. Agar bo'shliqlarni (probel) yoki boshqa maxsus belgilarni ("Mening rasmlarim" katalogi kabi) o'z ichiga olgan argumentni kiritmoqchi bo'lsangiz, argumentni `'` yoki `"` (`"My Photos"`) bilan tirnoqlash yoki kerakli belgilarni `\` (`My\ Photos`) bilan qochirishingiz (escape) mumkin.

Ehtimol, boshlayotganingizda eng muhim buyruq bu "manual"ning qisqartmasi bo'lgan `man`. `man` dasturi tizimingizdagi ixtiyoriy buyruq haqida ko'proq ma'lumot qidirish imkonini beradi. Masalan, agar `man date` ni ishlatsangiz, u `date` nima ekanligini va uning ishlashini o'zgartirish uchun unga qanday argumentlar uzatishingiz mumkinligini tushuntiradi. Aksariyat buyruqlarga `--help` argumentini o'tkazish orqali yordam ma'lumotining qisqacha versiyasiga ega bo'lishingiz mumkin.

> `man`dan tashqari [`tldr`](https://tldr.sh/) ni o'rnatish va foydalanishni hisobga oling, chunki u terminalning o'zida umumiy foydalanish misollarini ko'rsatadi. LLMlar ham odatda buyruqlar qanday ishlashini va maqsadlaringizga erishish uchun ularni qanday chaqirishingiz mumkinligini tushuntirishda juda yaxshi.

`man` dan keyin o'rganish kerak bo'lgan navbatdagi muhim buyruq bu `cd` yoki "change directory" (katalogni o'zgartirish). Ushbu buyruq aslida shell ichida qurilgan va alohida dastur emas (masalan, `which cd` "no cd found" deb aytadi). Siz unga yo'lni (path) berasiz va bu yo'l sizning joriy ishchi katalogingizga aylanadi. Shuningdek, joriy katalogingizni shell so'rov satrida ham ko'rishingiz mumkin:

```console
missing:~$ cd /bin
missing:/bin$ cd /
missing:/$ cd ~
missing:~$
```

> Shuni esda tutingki, shellda avtoto'ldirish mavjud, shuning uchun `<TAB>` tugmasini bosib yo'llarni tezroq to'ldirishingiz mumkin!

Ko'pgina buyruqlar, agar boshqa narsa ko'rsatilmagan bo'lsa, joriy ishchi katalogi ustida ishlaydi. Agar o'zingiz qayerdaligingizga ishonchingiz komil bo'lmasa, `pwd` ni ishga tushirishingiz yoki `$PWD` muhit o'zgaruvchisini chop etishingiz mumkin ( `echo $PWD` bilan), ikkalasi ham joriy ishchi katalogni ko'rsatadi.

Joriy ishchi katalogi *nisbiy* yo'llardan foydalanish imkoniyatini berishi bilan ham foydalidir. Shu paytgacha biz ko'rgan barcha yo'llar _mutlaq yo'l_ edi - ular `/` bilan boshlanadi va fayl tizimining boshidan (`/`) biror joyga harakatlanish uchun zarur bo'lgan barcha kataloglar ro'yxatini to'liq ko'rsatadi. Amalda, siz ko'pincha nisbiy yo'llar bilan ishlaysiz; ular joriy ishchi katalogga nisbatan bo'lgani uchun shunday ataladi. Nisbiy yo'lda (`/` bilan boshlanmagan ixtiyoriy yo'l) birinchi yo'l komponenti joriy ishchi katalogdan qidiriladi, va keyingilari ham xuddi shunday o'tadi. Masalan:

```console
missing:~$ cd /
missing:/$ cd bin
missing:/bin$
```

Har bir katalogda ikkita "maxsus" komponent (kataloglar) ham mavjud: `.` va `..`. `.` bu "shu katalog" va `..` "ota katalog" demakdir. Shunday qilib:

```console
missing:~$ cd /
missing:/$ cd bin/../bin/../bin/././../bin/..
missing:/$
```

Оdatda buyruq argumenti uchun mutlaq va nisbiy yo'llardan bir-birining o'rnida foydalanishingiz mumkin, shunchaki nisbiy yo'ldan foydalanayotganda joriy ishchi katalogingiz nima ekanligini yodda tuting!

> Kezishni yanada tezlashtirish uchun [`zoxide`](https://github.com/ajeetdsouza/zoxide) o'rnatishni o'ylab ko'ring - `z` tez-tez tashrif buyuradigan joylarni yodda saqlaydi va sizga yozishni qisqartirish imkonini beradi.

## Shell ichida nimalar mavjud?

Ammo shell `date` yoki `echo` kabi dasturlarni topishni qayerdan biladi? Agar shell buyruqni bajarishga so'ralsa, u `$PATH` deb ataladigan _muhit o'zgaruvchisini_ so'raydi, bu esa, buyruq berilganda shell qayerni qidirish kerakligi haqidagi kataloglarni ko'rsatadi:

```console
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

`echo` buyrug'ini bajarganimizda, shell `echo` dasturini ishga tushirishi kerakligini ko'radi va bu nomli faylni izlash uchun `$PATH` dagi `:` bilan ajratilgan kataloglar ro'yxatini tekshirib chiqadi. Qachonki topilsa, u uni ishga tushiradi (fayl _bajariladigan_ bo'lsa; bu haqda keyinroq). `which` dasturi yordamida berilgan dastur nomi uchun aynan qaysi fayl bajarilayotganligini aniqlashimiz mumkin. Biz shuningdek, ijro etishni istagan faylning _yo'lini_ ko'rsatish orqali `$PATH`ni chetlab o'tishimiz mumkin.

Bu, shuningdek, shell ichida qanday dasturlarni bajarish imkoniyatimiz borligini tekshirish uchun barcha dasturlarni ko'rsatish yo'lini ham bildiradi: `$PATH` da sanab o'tilgan barcha kataloglarning kontentini ko'rsatish orqali. Buning uchun ma'lum bir katalog yo'lini `ls` dasturiga o'tkazish orqali amalga oshirishimiz mumkin, u fayllar ro'yxatini ko'rsatadi:

```console
missing:~$ ls /bin
```

> Ko'proq odam-o'qiy oladigan (human-friendly) `ls` tajribasi uchun [`eza`](https://eza.rocks/) dasturini o'rnatishni va undan foydalanishni sinab ko'ring.

Bu, aksariyat kompyuterlarda, _juda ko'p_ dasturlarni chop etadi, ammo biz bu yerda eng muhimlariga to'xtalamiz. Birinchi, ba'zi oddiylari:

- `cat file`, `file` kontentini ekranga chop etadi.
- `sort file`, `file` dagi qatorlarni tartiblangan holda chop etadi.
- `uniq file`, `file` dagi ketma-ket takrorlanadigan qatorlarni olib tashlaydi.
- `head file` va `tail file`, mos ravishda `file` ning birinchi va oxirgi bir necha qatorlarini chop etadi.

> Sintaksis yoritilishi va sahifani aylantirish (scrolling) uchun `cat` o'rniga [`bat`](https://github.com/sharkdp/bat) ni o'rnatishni o'ylab ko'ring.

Shuningdek, `grep pattern file` mavjud bo'lib, u `file` da `pattern` ga mos qatorlarni topadi. Buning uchun biroz ko'proq e'tibor talab qilinadi, chunki bu _juda_ foydali va kutganidan ko'ra kengroq funktsiyalarga ega. `pattern` aslida juda murakkab naqshlarni ifodalay oladigan _regulyar ifoda_ hisoblanadi - biz bularni [kod sifati](/2026/code-quality/#regulyar-ifodalar) ma'ruzasida ko'rib chiqamiz. Bundan tashqari, faylning o'rniga katalog nomini ko'rsatishingiz (yoki joy joriy ishchi katalog ekanligini ko'rsatish uchun uni qoldirmaslik) va qidirishni bir katalogdagi barcha fayllar ichidan rekursiv amalga oshirish uchun `-r` parametrini (flag) uzatish kabi imkoniyatlar mavjud.

> Tezkor va ko'proq o'qilishi oson bo'lgan, ammo boshqa tizimlar bilan kamroq mos keluvchi alternativa o'rniga `grep` o'rniga [`ripgrep`](https://github.com/BurntSushi/ripgrep) ni o'rnatish va ishlatishni ko'rib chiqing. Odatda `ripgrep` o'zining standart holatida joriy katalogni rekursiv qidiradi!

Interfeysi biroz murakkab bo'lgan ayrim foydali dasturlar ham mavjud. Ulardan birinchisi `sed` - u o'zining dasturlash tili bo'lgan fayl muharriri hisoblanadi va fayllarni avtomatik tarzda tahrirlovchi o'ziga xos tahrirchi. Biroq, uni eng ko'p ishlatadigan holat quyidagichadir:

```console
missing:~$ sed -i 's/pattern/replacement/g' file
```

Bu `file` dagi barcha `pattern` ko'rinishlarni `replacement` ga almashtiradi. `-i` argumentining (flag) ifodalashi, o'zgaruvchilar bevosita ichida o'zgartirilishini bildiradi (ya'ni `file`ni o'zgartirilmagan qoldirib, o'zgartirilgan natijalarni nashr etish o'rniga). `s/` - bu sed dasturlash tilida (substitute) qidirib almashtirishini bilish usulidir. `/` ikkinchi yarmini birinchisidan ajratib turadi. Va orqadagi `/g` argumenti - boshida emas, balki _har bir qatorning_ barcha mosliklarini o'zgartirish kerakligini aytadi. Xудди `grep` kabi `pattern` shu yerdagi regulyar ifoda bo'lib, bu sizga ajoyib ifoda kuchini taqdim etadi. Regulyar ifoda orqasiga `replacement` moslashtirilgan namunaning qismlariga qaytish uchun yana havola imkoniyatini beradi - biz buni qisqa fursatda misol sifatida tushunib olamiz.

Keyingi qatorda bizda `find` bor, u ma'lum talablarga javob beradigan (rekursiv suratda topadigan) fayllarni topishga imkon beradi. Misol uchun:

```console
missing:~$ find ~/Downloads -type f -name "*.zip" -mtime +30
```

Downloads katalogidagi 30 kundan eski ZIP fayllarini topadi.

```console
missing:~$ find ~ -type f -size +100M -exec ls -lh {} \;
```

Asosiy ishchi kataloingizdagi (home folder) hajmi 100M dan katta fayllarni topadi va ularni ro'yxatlaydi. Shuni ham aytish o'tish joizki `-exec` oxirgi bajariladigan `;` buyrug'i (probellerdek qochirilishi zarur bo'lgan mustaqil ish) bilan birgalikda so'raladi va qayerdaki `{}` `find` tomonidan har bir mos keladigan faylning yo'li bilan (fayl nomlariga qarab) almashtiriladi.

```console
missing:~$ find . -name "*.py" -exec grep -l "TODO" {} \;
```

Barcha joriy `.py` fayllari ichidagi "TODO" ro'yxatlarini tekshirib topadi.

`find` dasturining sintaksisi biroz qiyin bo'lishi mumkin, ammo ishonamizki bu misol sizga bu vositaning qanchalik foydali bo'lishini bildiradi!

> `find` vositasidan foydalanishda sizga kerakli natijani (portable lekin unchalik ko'p imkoniyatlarga ega bo'lmagan) tezroq keltirib beradigan [`fd`](https://github.com/sharkdp/fd) dasturidan foydalanishni ham o'ylab ko'rishingiz mumkin.

Endi navbat `awk` tizimiga u ham, xuddi `sed` singari, o'zining dasturlash tili bo'lgan. `sed` fayllarni faol ko'rinishda tahrirlashga moslashtirilgan holda `awk` esa ularni tahlil etadi. `awk`ning ko'p maishiy vazifalari orasida CSV fayl kabi har qanday jadval, tahliliy sintaksisni ochish jarayonida va asosiy satrlarga tegishli aniq qiymatlarni (ma'lum qismlarni) tanlab berishi ko'proq ishlatiladi:

```console
missing:~$ awk '{print $2}' file
```

`file` da probelar bilan ajratiladigan barcha qatorlardagi ikkinchi qiymatni chop etadi. Qo'shimcha ravishda, agar siz `-F,` qo'shsangiz u barchasini vergul bilan ajralib beriladigan satrdan boshlab keyingi yozuvda chiqaradi (masalan, csv dan olingan). `awk` dasturi bundanda yaxshiroq ishlay oladi — barcha qatorlarni filtrlashi va boshqa maxsus vazifalarni (aggregatlar kabi) yig'ishini aytish joiz - uni mashqlarda isbot etishingiz mumkin.

Ushbu asbob-uskunalarni qo'shish bilan quyidagi vazifalarni birin-ketin tez va qulay o'zgartirib amalga oshiramiz:

```console
missing:~$ ssh myserver 'journalctl -u sshd -b-1 | grep "Disconnected from"' \
  | sed -E 's/.*Disconnected from .* user (.*) [^ ]+ port.*/\1/' \
  | sort | uniq -c \
  | sort -nk1,1 | tail -n10 \
  | awk '{print $2}' | paste -sd,
postgres,mysql,oracle,dell,ubuntu,inspur,test,admin,user,root
```

Bu server uzoq masofali SSH jurnallari matnidan ma'lum qismlarni uzish va bog'lash natijasida ulardan tahrir qilib ("ssh" bilan navbatdagi darsda tanishamiz), Disconnected yozuvlariga moslashib foydalanuvchilar ismlarini ajratadi, top 10 vergul qo'yilgan barcha nomlarni yig'ib beradi. Hamma ish bitta buyruq! Buni sinab o'tish uchun yana biz mashqlarga tashrif buyuramiz.

## Shell tili (bash)

Avvalgi darsimiz - yozuvimiz yangi tushuncha - pipe (`|`) haqida gapirgan holda ochilgan edi. Bular bitta dasturning chiqqan matni - standart chiqish (stdout) natijalarini keyingi ikkinchi matnga kiruvchi - standart kirish (stdin) vazifalarini qondiruvchi zanjirga tiqadi. Ko'pchilik komandali dasturlar, masalan xatolar bermay faqat interfeysi o'zini normal ko'rinishida siz terayotgan narsalar, ya'ni fayllarsiz yoki o'zgachaliklarga ko'ra ishlatsa xuddi ushbuga moslaydi. `|` ikkita chiziq oralarida standart natija va kiritmalarni bog'lasak ushbu jarayon zanjir sifatida aylanib (qarab chiqa oling), ijroli - tez hamda samarali imkoniyat hisoblangan eng mahsuldor funksiyalarni ishlatishingiz uchun eng asosiysilaridan bo'ladi. Shell ni bunchalik ish muhitiga mos ishlab turishiga sababidir!

Amalda, ko'pchilik shell'lar ham odatdagi to'liq dasturlarni o'zi ustida ishlashi bashga kabi imkon yaratadi va ham pythonga, ham ruby tiliga boyroq ishlashda katta turtki bo'lgan. Variables(o'zgaruvchilari), shartli (if-else), cikl (loop) operator va yana boshqa qo'shimchasi ham ko'pdir. Buyruq bergan vaqtdagi sizni kod yoki dasturlaringiz shu yerda kichik bir tarjima orqali shellga yetkazuvchi - muhim ishlayotgan shaxslar deb hisoblay qiling. Bu yerda hamma bash vaziyatlarini o'rgatip bermaymiz ammo asosiy holatlarga diqqat axtaramiz:

Birinchisi - qayta yo'naltirish ( redirects): >`file` bir kod xulosasini oldindan ma'lum xuddi matnlarga `file` larga oddiy qilib tushirmay natijani `file` yozishingiz haqidadir. Bu uni ancha vaqt tekshirib-chiqishga eng samarali natijalayapti, ko'pincha . . `>>file` ga yozishda ustidan urub ishlash emasu barcha ma'lumot qoldirilganda oxirigisidan boshlayapti. Bundanda oshib <`file` buyrug'i tizimi uni yana odatdagi ( klaviatura orqada) vazifasiga emas , ma'lum `file` larga qaratishda birinchi navbatdagi tushunchalarni ( standart qabul qilib) beradi.

> `tee` ni eslab utiladigan yaxshi joyga kelidk. `tee` funksiyasi siz aytgan fayl va oynani parallel chiqarib olib biluvchi ko'pgina funksiyalar hisobiga o'tilganini kuzatsa — fayl va terminallaringiz ham o'z vaziyatini bajarib yo'qovlarsiz ishlab tura olmoqda. . shuning bilan `verbose cmd | tee verbose.log | grep CRITICAL` siz terminallariga kerakli xabarni bermaydida , barcha verbose jarayonining boshidan oxirini ehtiyotan alohida jurnalga yoza oladi.

Navbat - shartlar (conditionals ) uchun kelib qoldi : `if command1; then command2; command3; fi` qachonki `command1` yechila olsa , siz kutgan aytilingan command 2 hamcommand 3 bo'lganini ishonch qilib ko'rasiz va xatosiz yurishlarini bilsa birin - ketin ishlatsada ular shu yerda tushunchalari ustuvor ro'l deb sanaladi. Siz buni else qator bo'limlari ishtirokidasining xohishi bo'lishida ishga tushura olishiz lozim , eng oddiy funksiyalardagi command 1 siz ehtiyotlab test so'rovidan so'ray biluvchi hisobida bir qatorda yordam bilimi . Ushbu `[ file - f ]` faylni bor yoki bo'sh joy kabilarni ham , yoki teng miqdori ( `' [ "$var" = "string" ] '` ) ekanlik vaziyalar ham test yordamchisi ko'rinishi hisobidan eng kerakdir. . Bashta, yana ham yaxshiroq ko'rinishi bo'lmish ikki talik `[[ ]]` test xavfsiz holatdagi bo'lib kotirovka (qo'shtirnoqlardagi) chokib qolmaydigan xossalidir.

Shuningdek. bash ikkita bo'limidan iborat bo'lgan while va cikl (loop ) uchun `while command1; do command2; command3; done` ishlatsa birinchi jarayoni har doim takrror yuritib va shu bo'yicha ishlanmasini ko'rish bilan command1 o'zi xato (error) qoldirmasa u ish davomiy holati bilan qolajak, - qachon ki command 1 ga tugallash oydin bo'lgunicha deb qo'llamiz, keyin bo'limlari - "a,b,c va d kabi so'zlari aytilgan for orqali `for varname in a b c d; do command; done` - navbati bn har biri 4marta aylanuvchisidan hisoblanib oladi .. Eng avvallo item (qisimlarini ) so'zlarga bermay maxsus vaziyatlaridan foydalanganimiz kabi yana bir xossa command substitution o'rnida `$()` dir. Ya'ni:

```bash
for i in $(seq 1 10); do
```

Ushbu jarayonda (seq 1 dan to 10 ) sanash ketma-ketliklarni bir vaqtda bajarilgan (buyruqni) aylanmada va keyin hammadan oshirib shu raqamlargacha natijasi aylanuvchi - $() ni shu javob bilan bo'lishiga o'zgartiradi , siz uni endi for tsiklini ( 10 martta sikl) olgan ko'rashingiz mumkin. Siz eski skriptlarga qaragan paytida odatdagi oddiy tirnoq (' ') ko'rsa agar bunisi ``for i in `seq 1 10`; do`` , lekin endigilikda u ichkari qo'llangani ko'p tarqalgan degani uchun $ () ko'rinishidan afzal qilasiz!

Shuningdek uzoq scriptlardan ko`pida tildan uzoq paytlari shellda foydalana olish maqul kelsa endigi hollardi, alohoda bittaga saqlab chiqilgan yaxshiroq holidir .sh deb - ushbu yerda ishlamagan fayllardagi CPU ishini xatosini qulatishga takror tekshirtiladigan script :

```bash
#!/bin/bash
set -euo pipefail

# Start CPU stress in background
stress --cpu 8 &
STRESS_PID=$!

# Setup log file
LOGFILE="test_runs_$(date +%s).log"
echo "Logging to $LOGFILE"

# Run tests until one fails
RUN=1
while cargo test my_test > "$LOGFILE" 2>&1; do
    echo "Run $RUN passed"
    ((RUN++))
done

# Cleanup and report
kill $STRESS_PID
echo "Test failed on run $RUN"
echo "Last 20 lines of output:"
tail -n 20 "$LOGFILE"
echo "Full log: $LOGFILE"
```

Bunda anchagina foydali vositalar qamrab olinganini sezasiz. Tavsiyam uni fon vazifasi (`&`) kabi, bir vaqtning o'zida bir nechta dasturlarni ishlatish uchun kerakli chaqiriqlar, yanada murakkab [shell qayta yo'naltirishlari](https://www.gnu.org/software/bash/manual/html_node/Redirections.html), va [arifmetik kengaytirish](https://www.gnu.org/software/bash/manual/html_node/Arithmetic-Expansion.html) tushunchalari ustida chuqurroq ko'rib chiqishingiz kerak bo'ladi.

Biroz e'tiborni dasturning dastlabki ikki qatoriga qaratish muhimdir. Birinchisi — bu “shebang”; siz uni ko'pincha boshqa shell skriptlarining yuqori qismida ko'rishingiz mumkin. Qachonki `#!/path` sehrli yo'li bilan boshlangan fayl bajarilganda, shell `/path` yo'li ko'rsatgan dasturni ishga tushiradi va shu faylning mazmunini unga kiritma (input) tarzida beradi. Boshqacha qilib aytganda bu `bash` deb tushuntirilishini anglatadi, masalan Python uchun esa u shebang sifatida `#!/usr/bin/python` shaklida ifodalanadi!

Ikkinchi qator shell skriptida yozilganda, unga yuzaga kelishi mumkin bo'lgan tasodifiy xatoliklarni oldini olish (mitigate footguns) usulidir, shuningdek skriptimiz "qattiq" yozilishini ta'minlab bash sintaksisiga bog'lanishida katta o'rin beradi: masalan, `-e` parametrini qo'shsangiz buyruq ishlamasa bas deb tez tugashga undaydi; `-u` o'ylangan bo'sh argument borishi bo'lsa uni tekshirishdagi qiynalish o'rnida ish bermay skript xato deb topib tez qulatishi, va`-o pipefail` buyruq bo'limlari - `|` (pipe) quvuringiz ichida ishlayotganda ham barchasini birdan to'xtata olishiga sharoit ko'rsatib xatolik o'yiga ota oladi.

> Shell dasturlashi — xuddi boshqa har qanday dasturlash tili singari ancha chuqur o'rganilishini talab qiladigan keng soha. Lekin ehtiyot bo'ling: boshqa tildan ko'ra bu tildan topiladigan va yo'lda chalg'ish (gotchas) deyiluvchi - uning ayb-kamchiliklariga ko'p xos saytlar ham bor  [(ularni shu yerda ham topsa bo'ladi)](https://mywiki.wooledge.org/BashPitfalls). Shunday skriptlar yozaotganda  [shellcheck](https://www.shellcheck.net/)-dan unumdorlikda ham katta kuch olishini unutmang. Bundan tashqari, Shell kodlaringiz ( 100 qatordan ham asosiydir ) qanchalik qimmatlasha ketishi ulug'landi, shunda LLM lar kodlar va xatolarni tekshiradigan yagona bo'la oladigan, real "til" bashkabi bo'lishlarda, masalaning halini topar darajada yodlanib tez ishlatishlari ham - ancha foydali isbotan tavsiya beradi.


# Keyingi qadamlar

Shu nuqtadagi xolosamizga ko'ra, siz shell sohasida ishlarni amalga oshirishda baza bilmlarni egallaganingizdan dalolat. Bu fayllarni toparligida bilimlari, navigatsiya ishlatiqlariga katta turtki deb umidimizda bilmoq kerakdir. Navbatdagi biz o'zbek ma'ruzamiz - siz o'rgatiluvchi dasturlardan avtomatlashtirishning o'zga va yangi sirlari bilan yanada chuqurlashib  shell bo'limida ish yuritadigan yaxshi bo'limlarda tushuntirib otamiz.

# Mashqlar

Shu va boshqa barcha darslarda ishtirokingizni sinab bilish uchun doim asos bo'luvchi qiziqarli ko'rinishdagi o'z maxsus vazifa sinovlari ketma-ketliklarda tavsiya ko'chadi. Ulardan biz kerakligicha har bir amaliyot-topshiriq usullarida qiziqtirishga , qattiq topshiriq yo "X va Y ish ismilariga bo'ling",  ishlanmalarning doim sinab-ishlarida va bajarilganda o'z kuchisini toparkan eng ko'p javobi aytilar ekan!

Biz bu savollarga javoblarni to'g'ri bo'lsa, maxsus manbadan yo boshqa tur sahifasi qoldirmaganmiz, ular alohida qo'lda chiqarilgan ish natijani bersinda! Tushunish uchun bosh ko'rinmay turilgan savollar bormi? u holda siz undan savolni - [ Discorddagi ](https://ossu.dev/#community) kanali `#missing-semester-forum` kanalidan ulashishlaga unutmangiz, ulardan  va  qo'shiluvchilar orasida pochta orqali biz bilgan ekanligi-  sinab qidiruvchi, shuning uchun boshqalar fikri natija topshiriqni bersin - ham biz iloji bor  (emailni ko'ring), qo'llashib o'yinlarni boshqotirman holatini sizga qo'ldirib yo'naltamiz. Bundan qanchasi-yoki faqat qidirilsada tushunchalar interaktiv yondashivlarni qo'lidan tutish - yaxshi variant va doim o'rganilgan darslarda "nima uchun/ qanday qilib'ga , qisqacha marshrutli  javoblardanda eng samarali "sayohatli qadamga boylikdir ! 

1. Ushbu kurs uchun siz Bash yoki ZSH kabi Unix shellidan foydalanishingiz kerak. Agar siz Linux yoki macOS tizimida bo'lsangiz, alohida biror narsa qilishingiz shart emas. Agar siz Windows tizimida bo'lsangiz, cmd.exe yoki PowerShell orqali kirmaganlikni tekshirishingiz zarur, ularning o'rniga u asosida qurilgan [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) dan Unix turiga xos xizmat funksiyasini ta'minlashingiz yohud linux terminali operatsion tizimlari orqali ishlating. Dastur to'g'ri ishlayotganiga ishonch hosil qilish maqsadida, `echo $SHELL` dan tekshirib foydalaning va `/bin/bash` yoki `/usr/bin/zsh` chiqqani, bu jarayon yaxshi ekanini ko'rsatadi.

1.  `ls` da ko'rsatgan  `-l` bayrog'i (flag) nimani berishi kerak? `ls -l /`ni ishlatish orqali uning qatordagi kelgan har 10 ma'lumotlarini kuzating.   Bular nima degani?  (`man ls` dan mosligini o'rganing)

1.  Quyidagi `find ~/Downloads -type f -name "*.zip" -mtime +30` -da keltirilgan `*.zip` bu - "glob". O'zi Glob nima degani? Shu turini ishga tushirishda shunday jism bo'lishi - `ls *.txt`, `ls file?.txt`, and `ls {a,b,c}.txt` ekanining turli  fayllarini o'rnatilgan tajriba yaratish bilan sinab biling. .Bashning ruxsatli tushuntirish va ma'lumotda qarang: [Pattern
   Matching](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html).

1. Bularning - `'single quotes'` (bitta tirnoqlar), `"double quotes"`(ikkilangan) va `$'ANSI quotes'` ( ANSI tur) uchchasida barcha  fayllari orasida farq  haqidagi nimalar bor? Maxsus tirnoqli ($), maxsus  yangi qatordagi aylanuvchi satrdan kelib chiqadigan qator ishonchini hosil qilishdagi yozuv tayyorlang va shuni ifodalashdagi ko'rinish va o'zgarishini ko'rsating - [Quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html) mavzusidan olishni ko'rinib  biling.

1. Shell uch xil oqimga (stream) o'z yondashuvini qaratadi : standart kirish (stdin)  -( 0)  , standart chiqish (stdout ) - (1)  , va xatolik standart chiqishi (stderr) - (2) .   `ls /nonexistent /tmp` yozing hamda unisini  standart chiqish sifatida, ya'ni fayllardan va bu bilan xatolik uchun bashqalar turida uzating . Qanday qilib fayldagi oqim ko'rinishlari qayta maqsadni qo'ldamchi bittagina  fayliga qاراتsa bo'ladi  ? Shu jarayon haqidan ma'lumoti : [Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html).

1. `$?` bo'lgan belgi orqali oldingi operatsiyalardagi (ish tugaganga qayishning- status kabi) eng so'ngi chiqish holati ko'rinishi (ya'ni `0` tengligi yaxshidan chiqqani ko'paygan natijada beriladi) ushbu `&&` shunda oldin yaxshi deb topilgan xulosa bilan yurib  va `||` usullari keyingilarga ulasha olish haqida edi , siz ko'p marotaba sinagan bo'lsangiz agar, - /tmp/mydir' yaratuvchiga yangi yozuvi o'z vaqtda bo'lmagani yo'q qilish, borasida oddiy bir  ishga tushirivchi amallar kodini (one-liners ni) yoza chiqing ([ Exit status ma'nosidan ](https://www.gnu.org/software/bash/manual/html_node/Exit-Status.html)) qarab bajarishingiz .

1. `cd` katalogini o'zi maxsus ishlab kelishga - nega uni umuman faqat yangi joydagi shirin joy dasturlardan deb atalar? Buning asosiya sababi- `cd`ning nima asosi yo nimasi bo`lishidadir? (Yordam - bola jarayon nimalargani, ixtiyoriy vazifani - oldin yurgizgan ota jarayon - shunday tushuncha atvorlariga e'tibor)

1. Bizga siz orqali - bir argument uchun yozuv(`$1`) berilib va ular orqali agar rostan ham ko`rmagan ishlarni yo o'sha test - (`test -f` yoxud usul   `[ -f ... ]`) da bo`lgan holat deb bilingchi . Bu xatolik yo'riqnomasida ikki turlicha shartli narsa topilsami fayl o'zi shular kabi harakatlanganga turishidagi bosib olsin  o'z fikrini - ko'rinuvchilarni topib chiqsin. Qarang; -( [Bash Conditional Expressions](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html). )

1. Siz o'zingiz yozgan shu tayyor qo'llanmadagi kod ishtrokchi dasturni (sh) ni saqlash kerak bo'ladi ( masalan - olingda shunga: `check.sh`). Endi sinov tarzidan boshqarsakchi  `./check.sh somefile`. Ana, qanday o'zgarish guvohi bo'ldingiz ? Uni   `chmod +x check.sh` ko'rinishda o'z qadamini qaytadan ko'ring. Ana , Nima u sababi bu holga kirdi ?  ( Maslahat! fayllarga kelgan shundan `ls -l check.sh` ni ishiga kiradigan oldingisinikiy yo`lini `chmod` ga solishtirib chiqaring . ) 

1. Skriptdagi flaglar ishorasi qachonki `set` bo'linishiga `-x`ni qo`shsangiz ishida , keyin usha amaliy holida natijalarda qayta nimalarda sinaysiz va nimagaga farqlanar ? Bundaylar izlanuvni  oddilashidan (kodlari ko`rinish  beriluvini ) - tushintirish - [ Set  buyruqlari  asosidan bilish mumkin ](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html) .

1. Har qanday va bugungi kundan ( yozib) ishlarga moslangan "bugungi maxsus vaqt sanali " ism ko'pinchalik asosiy qo'shilmasi  (Masalan: `notes.txt` dan to'g'irlab  → `notes_2026-01-12.txt`) deb chiqarishi aytish qilib aylanmasini ko`ring - shunday nusxa (backup ) berish kodi borasida buyruq yarating. (Yordamchi variant - `$(date +%Y-%m-%d)`). Bundan , maxsus  [ Buyruq almashtirish  xulosa bo'limi - ](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html)'ni bilsak ish osonlashadi .

1.  Ma'ruzalardagi oldin esda qoldirmagan holda sinab korgan ko'rib qat'iy tekshirgan testimiz - skriptlarning xatosin tuzatib qattiqlanganidan - uni maxsus berilgan xato kodida " cargo test my_test " nomi qaytada hardcoded turgani asno xuddi oldingi ishiga endi o`zingiz argument sifatida aylaninishda berin ko`ringчи :  ( Maslahat  - bu yer `$1` , yoki `$@`) . Maxsus joy [ Parametrlardagini olishda ](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html).

1. Umumiy ishlashingizga tegishlii `|` xususiyatlik , asosiy `home` nomdagi papkada shunda asosan uchradigan qo'shilgan fayllarni nomini topilishda yordamlashadigan qidiruv zanjiri. (Yordam -  aralash o`rniga qo`ysa ishdir . . `find`, `grep` turidek , yo bo`lmasa `sed` va shuningdek `awk`, keyin ham `sort`, `uniq -c` lari va `head` kabi tildan eng zarurlarga maxsus kombinatsiyalashni amalda - natijasidagini topish ).

1. `xargs` - standart oqimdagi satrdan chiqarib olishdan oldingi buyruqlarga kerak argument yo'rig'ini qo'yishga . Ikkalsi ham alal oqibat eng yordamli  `find` yo shu bilan `xargs` buyruqlari qiling  -( -exec dan qochgan bo'ling) bu bir katalog joy bo`limini `.sh` ni hammalaridan chiqarib bering-  chi shunda va undagi fayilning hamyoni qatori orqasida  -`wc -l` so'ralishlarining sonlarisiga javob topsin. Endi, katta sovg'asini bilmoq - yo`l oralariga ko'plab boshliqli interval qoldirish nomiga kiradi  : ularga shular qollanisi -- (-print0 ni - va maxsusiga -0 kiriting - yana bir ma'lumot)  -- yordam : `man xargs`.

1. Asosiysi uni to'g'ri bajaring - `curl` ishi . Siz darslikdagi  shunday onlayn maxsus (saytdagi HTML ko'rinishi ) ma'lumoti orqasini (`https://missing.csail.mit.edu/`) shu zanjir `| `ga grep qo'shib yuborish bilangina uni natijasida usha nomidan biror ismini oqinib "barcha dars - lekitsiyalarni jami-i nechtalik qatorgacha chiqqani haqidan ' hisoblanadigan funksiyasin topishingizdir.   (Maslahat -  ma'ruzalar deb atalib boradi qanday ifodada ; ehtiyot yo`rganda shunday - xabarlar chiqmasga sukut bermoq- `curl -s`).

1. [`jq`](https://jqlang.github.io/jq/) -JSON kabi hujjatini ko`ruvchi asosiy asbobi. Bu "namuna" yozilma- malumot bilan  (`https://microsoftedge.github.io/Demos/json-dummy-data/64KB.json` dagi manzildada  ) va qilib oling `curl` va albatta yana uni  `jq` orqali- tahliladi - keyin aynan "versiyasi- 6 - kattasi bo'lgan fuqolardan " (person nomlarigani) - ajratsin deydi deganini. . Yana: zanjir `|` va yordam  : `jq .`   qayrilib (struktrasini ko'ring) va maslahatdan (`jq '.[] | select(...) | .name'` deb qidirib barchasining boshqa jarayonlarisiga yo'lni qo'ling).  

1. Navbat esa : -  `awk` unga mos filtorli ko'rinish , masalanga asoslanuvchi - shunday ma'lum qatordagilarning ko'rsatilishi haqida  qabuldir . shunda misolni - `awk '$3 ~ /pattern/ {$4=""; print}'` - ushbudan izlanilgan pattern' lari uning uchunchu ustuunligdagi shartnomani yo unisanisini va keyingi qismi  - oxirgisidagi (4 ) ustuunda chiqarilmayotgani holdada. Shuni o'zingzda amaliyot - " faqat va faqat `ikkinchi qiymat-  ikkinchi qator ustuni ' - katta - 100 ligini bilishi holdagani , ammo boshqa o'zgarishda (birinichi bn -3) almashtilishga ish ko`radigan kodga olib chiqaring  . sinoviya kodi bu : `printf 'a 50 x\nb 150 y\nc 200 z\n' `!

1. Ma'ruzadagi biz yaratganday bo'lgan o'sha ko'rgazmada keltirgan (SSH qatorni matnlilari) buyruqdagi ishimizdagi har joyidan- nimasi nechinchi-va sabab nimasligini bo'lishni aytib -o'tib yechilishini tahliling . Lekin ham ishni va o'zi topkan yangilikdek oddiy bilminigzni : siz ish yurotiydigan komandalarga sharoitiy tarix( log /history) va ya'ni aynan shellga maxsus ekani " eng kop buyuruqiingiz "- asosoiy (  `~/.bash_history`yoxud u yerdagi- `~/.zsh_history`. )  bo'lichiga oxshatib yarata chiqishingizmi - kutingan savollaridan biri !