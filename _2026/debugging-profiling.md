---
layout: lecture
title: "Nosozliklarni tuzatish va Profillash"
description: >
  Dasturlarni loglash va debaggerlar yordamida qanday debag qilish hamda unumdorlik uchun kodni profillashni o'rganing.
thumbnail: /static/assets/thumbnails/2026/lec4.png
date: 2026-01-15
ready: true
panopto: "https://mit.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=a72c48e3-5eb2-46fa-aa03-b3b700e1ca8d"
video:
  aspect: 56.25
  id: 8VYT9TcUmKs
---

Dasturlashdagi oltin qoidalardan biri shuki, kod siz kutgan ishni emas, balki siz unga buyurgan ishni bajaradi. Bu farqni kelishib olish ba'zan ancha qiyin vazifa bo'lishi mumkin. Ushbu ma'ruzada nosoz (buggy) va resurslarni ko'p sarflaydigan kodlar bilan ishlash uchun foydali usullarni ko'rib chiqamiz: nosozliklarni izlash (debag qilish) va profillash.

# Debag qilish (Nosozliklarni izlash)

## Printf orqali debag qilish va loglash

> "Eng samarali nosozliklarni tuzatish vositasi bu hali ham puxta o'ylash va to'g'ri joylashtirilgan print ifodalari hisoblanadi" — Brian Kernighan, _Unix for Beginners_.

Dasturda nosozlikni izlashning birinchi usuli — muammo bor deb o'ylagan joyingiz atrofida `print` operatorlarini qo'shish va muammo manbasini tushunish uchun yetarli ma'lumot olguncha jarayonni takrorlashdir.

Ikkinchi usul esa, maxsus `print` ifodalari o'rniga dasturingizda loglashdan foydalanishdir. Loglash asosan "ko'proq e'tibor qilib chop etish" bo'lib, odatda quyidagi narsalarni qo'llab-quvvatlaydigan loglash freymvorki orqali amalga oshiriladi:

- loglarni (yoki loglarning bir qismini) boshqa chiqish manzillariga yo'naltirish qobiliyati;
- muhimlik darajalarini (masalan, INFO, DEBUG, WARN, ERROR va hokazo) o'rnatish va chiqishni ularga ko'ra filtrlash; va
- log yozuvlari bilan bog'liq ma'lumotlarni tuzilmali loglash (structured logging), buning yordamida keyinchalik ularni osongina ajratib olish mumkin bo'ladi.

Dasturlash jarayonida odatda siz loglash ifodalarini oldindan kiritib qo'yasiz, shunda debag qilish uchun zarur ma'lumot u yerda allaqachon mavjud bo'lishi mumkin! Darhaqiqat, `print` ifodalari yordamida muammoni topib tuzatganingizdan so'ng, ularni o'chirish o'rniga to'g'ri log ifodalariga aylantirib qo'yganingiz ma'qul. Shunday qilib, agar kelajakda shunga o'xshash xatolar yuz bersa, kodni o'zgartirmasdan turib diagnostika ma'lumotlariga ega bo'lasiz.

> **Uchinchi-tomon loglari**: Ko'pgina dasturlar ishga tushganda ko'proq ma'lumot chiqarish uchun `-v` yoki `--verbose` bayroqchasini qo'llab-quvvatlaydi. Bu ma'lum bir buyruq nima uchun xato berayotganini aniqlashda foydali bo'lishi mumkin. Ba'zilari yanada ko'proq tafsilotlar uchun bayroqni takrorlashga ham ruxsat beradi. Xizmatlar (ma'lumotlar bazalari, veb-serverlar va boshqalar) bilan bog'liq muammolarni debag qilishda ularning loglarini tekshiring—bular Linux'da odatda `/var/log/` katalogida joylashgan bo'ladi. systemd xizmatlarining loglarini ko'rish uchun `journalctl -u <service>` dan foydalaning. Uchinchi tomonga tegishli kutubxonalar uchun muhit o'zgaruvchilari yoki konfiguratsiyasi orqali debag loglarini qo'llab-quvvatlashini tekshiring.

## Debaggerlar

Nima chiqarishni bilsangiz hamda kodingizni osongina o'zgartirib va qayta ishga tushirish imkoni bo'lsa, print orqali debag qilish yaxshi ishlaydi. Biroq, qanday ma'lumot kerakligini bilmaganingizda, nosozlik faqatgina takrorlash qiyin bo'lgan holatlarda namoyon bo'lganda, yoki kodni o'zgartirish va dasturni qayta ishga tushirish qimmatga tushganda (uzoq ishga tushish vaqti, qayta tiklanishi kerak bo'lgan murakkab holat va h.k.) debaggerlar bebaho vositaga aylanadi.

Debaggerlar shunday dasturlarki, ular sizga o'z dasturingizning bajarilish jarayoni bilan ishlash imkonini beradi:

- Muayyan satrga kelganda ijroni to'xtatib turish.
- Har gal faqat bitta yo'riqnomani (instruction) qadamma-qadam bajarish.
- Halokat (crash)dan keyin o'zgaruvchilarning qiymatlarini tekshirish.
- Berilgan shart qanoatlantirilganda ijroni shartli ravishda to'xtatib turish.
- Va yana ko'plab ilg'or xususiyatlar.

Aksariyat dasturlash tillari deyarli o'zlarining debaggerlarini qo'llab-quvvatlaydi (yoki birga keladi). Eng ko'p qo'llaniladigan va universal debaggerlar bu barcha nativ binarlarni debag qila oladigan **umumiy maqsadli debaggerlar** hisoblanib, masalan, [`gdb`](https://www.gnu.org/software/gdb/) (GNU Debugger) va [`lldb`](https://lldb.llvm.org/) (LLVM Debugger) kabilar. Shuningdek, ko'plab tillar o'ziga xos ish vaqti (runtime) bilan mustahkam bog'langan **tilga xos debaggerlarga** ega (masalan, Python'ning pdb yoki Java'ning jdb'si).

`gdb` C, C++, Rust va boshqa kompilyatsiya qilinadigan tillar uchun standart debagger (de facto) hisoblanadi. U har qanday jarayonni tahlil qilib, uning mashina holati haqidagi ma'lumotlarni — registrlar, stek, dastur hisoblagichi va boshqalarni olish imkonini beradi.

Ba'zi foydali GDB buyruqlari:

- `run` - Dasturni ishga tushirish
- `b {function}` yoki `b {file}:{line}` - To'xtash nuqtasini o'rnatish
- `c` - Bajarishni davom ettirish
- `step` / `next` / `finish` - Ichiga kirish / ustidan o'tish / tashqariga chiqish
- `p {variable}` - O'zgaruvchining qiymatini chop etish
- `bt` - Backtrace'ni ko'rsatish (kutilayotgan chaqiruvlar steki)
- `watch {expression}` - Qiymat o'zgarganda to'xtash

> Buyruqlar qatori yonida manba kodini ko'rsatadigan split-ekran (ikkiga bo'lingan ekran) ko'rinishi uchun GDB'ning TUI rejimidan foydalanishni ko'rib chiqing (`gdb -tui` yoki GDB ichida `Ctrl-x a` tugmachalarini bosing).

### Yozish va Qayta ishlash orqali Debag qilish (Record-Replay Debugging)

Eng ko'p asabni buzuvchi xatolar bu _Heisenbug_'lar hisoblanadi: siz ularni kuzatishga uringaningizda ko'zdan g'oyib bo'ladigan yoki o'z xatti-harakatini o'zgartiradigan xatolar. Poyga holatlari (race conditions), vaqtga bog'liq nosozliklar va ma'lum bir tizim sharoitlaridagina yuzaga keladigan muammolar ushbu toifaga kiradi. An'anaviy debag qilish bu yerda ko'pincha foydasiz, chunki dasturni qaytadan ishga tushirish turli xil natijalarni berishi mumkin (masalan, print ifodalari kodni yetarlicha sekinlashtirib poyga holatini yo'qotib yuborishi mumkin).

**Yozish va qayta ishlash debaggingi** bu holatni dasturning bajarilish jarayonini yozib olish va uni kerakli darajada takroran aynan oldingidek (deterministik ravishda) bajarishga imkon berish bilan hal qiladi. Yana ham yaxshisi, qayerda nosozlik paydo bo'lganini topish uchun bajarilish jarayonini orqaga qaytarishingiz mumkin.

[rr](https://rr-project.org/) dasturi bajarilishini yozib oluvchi va u orqali to'liq debag qilish xususiyatlari bilan deterministik qayta ijro etishga imkon beruvchi Linux vositasidir. U GDB bilan birgalikda ishlaganligi sababli, uning interfeysi sizga allaqachon tanish bo'lishi mumkin.

Asosiy foydalanish:

```bash
# Dastur bajarilishini yozib olish
rr record ./my_program

# Yozib olinganini qayta ishga tushirish (GDB'ni ochadi)
rr replay
```

Asosiy mo'jiza qismi aynan qayta ishlashda namoyon bo'ladi. Sababi bajarilish qat'iy mantiqiy (deterministik) ekanligida bo'lib, buning uchun siz quyidagi **orqaga debag qilish (reverse debugging)** buyruqlaridan bemalol foydalanishingiz mumkin:

- `reverse-continue` (`rc`) - To'xtash nuqtasiga yetguncha orqaga ishlash
- `reverse-step` (`rs`) - Bitta qator orqaga qadam tashlash
- `reverse-next` (`rn`) - Orqaga qadam tashlash, lekin funksiya chaqiruvlarni o'tkazib yuborish
- `reverse-finish` - Joriy funksiya boshiga kelguncha orqaga qaytish

Bu nosozliklarni izlashda ajoyib tarzda qo'l keladi. Dastur bir marta buzildi deylik, buning uchun nosozlik qayerda yashirinib yotganini izlash orqali to'xtash nuqtasini o'rnatishga vaqt ketkazmasdan quyidagilarni amalga oshirgan ma'qul:

1. Dasturni buzilguncha ishlating
2. Buzongan va o'zgargan qiymatlarni tekshiring
3. Shubhali o'zgaruvchiga kuzatish nuqtasini (watchpoint) o'rnating
4. Qayerda o'zgarganini izlash uchun `reverse-continue` buyrug'idan foydalaning

**rr'dan qachon foydalanish kerak?**
- Vaqti-vaqti bilan ishlamay qoluvchi ishonchsiz (flaky) testlarda
- Multithreading bilan bog'liq poyga holatlari va xatolar 
- Dasturdagi takrorlanishi qiyin bo'lgan halokatlarda (crashes)
- Siz "orqaga qaytishni" xohlaydigan nosozliklarda

> Eslatma: rr faqat Linux'da ishlaydi va uskuna (hardware) ma'lumotlarini kuzatib boradi. U uskuna hisoblagichlarini ko'rsatib bermaydigan VM'larda, masalan, aksariyat AWS EC2 instanslarida ishlamaydi va u GPU interfeysiga ruxsat bermaydi. macOS muhiti uchun esa, [Warpspeed](https://warpspeed.dev/) resursidan tanishib chiqing.

> **rr va parallellik (concurrency)**: rr dastur bajarilishlarini qat'iy mantiqiy darajada yozib olganligi bois oqim rejalashtirishni izchil bajaradi. Bu degani, agar ular muayyan vaqt bilan hamqadam ketsa poygalik sharoitlariga tushib qolmasligining ehtimoli mavjud. rr o'sha holatlarni debag qilish uchun o'ta qulay dastur—agar buzilishga tushib qolgan vaziyatni qo'lga tushirolsangiz, bu jarayonni oson bartaraf qilasiz—lekin yashirilgan debaglarni izlashda biroz marta harakat qilishingizga to'g'ri kelishi mumkin. Poygalik jarayonlariga daxli yo'q holatlarga ham rr'ning sezilarsiz afzalligi bisyor: doim jarayonni ishlashi ortga debag yuzaga kelishini orqaga ko'rsatish mumkin.

## Tizim Chaqiruvlarini Izlash (System Call Tracing)

Ba'zida kodingiz operatsion tizim bilan qanday aloqada ekanligini anglab yetishingiz kerak. Dasturlar turli yo'nalishlarda ehtiyoj ko'rib yadro (kernel) xizmatlaridan foydalanishi uchun [tizim chaqiruvi](https://en.wikipedia.org/wiki/System_call) amalga oshiradilar—fayllar ochish, xotira bo'lib berish, jarayon tuzish va h.k. O'sha chaqiruvni kuzatib borish nafaqat dastur nimaga ishlamayotganini fosh etadi, ixtiyori o'zini taqdim etishi, qanaqa ma'lumotlarga so'rov etishi yoki qayerda asirda qolishini bildiruvchi xabar bermoqda.

### strace (Linux) va dtruss (macOS)

[`strace`](https://www.man7.org/linux/man-pages/man1/strace.1.html) yordamida dastur yaratib ketadigan hamma chaqiruvlarni ko'zdan ishlashingiz qulay:

```bash
# Barcha tizim chaqiruvlarini izlash
strace ./my_program

# Faqat fayl bilan ishlash chaqiruvlarni qarab chiqish
strace -e trace=file ./my_program

# Bola jarayonlarni (child processes) ortigidan kuzatish (boshqa processlar qoldirish ishlari muhim funksiyalarga ko'rsatganligi)
strace -f ./my_program

# Ishlayotgan jarayonni izlash
strace -p <PID>

# Vaqt haqidagi ma'lumotlarni chiqarish
strace -T ./my_program
```

> macOS va BSD muhitida bu buyruq o'xshashi bo'lgan [`dtruss`](https://www.manpagez.com/man/1/dtruss/) bo'lib (`dtrace` bilan faolyat bilan ishlaydi).

> `strace` borasida qiziqarli qo'llanma Julia Evans tomonidan hozirlagan [strace zine](https://jvns.ca/strace-zine-unfolded.pdf) ma'lumoti to'playdi.

### bpftrace va eBPF

[eBPF](https://ebpf.io/) (Kengaytirilgan Berkeley Packet Filter - extended Berkeley Packet Filter) yadro ichida sandbox bilan bog'liq ishlarni amalga oshirishda yetakchi Linux utilitasi hisoblanadi. [`bpftrace`](https://github.com/iovisor/bpftrace) esa eBPF'nin skript yozish texnologiyasiga zomin yaratuvchisi ko'rsatib kelayapdi. Yuqori aniqligi va kernel dasturlar bilan erkin ma'nodor sintaksis yordamida (awk sintaksisiga o'xshaydi) istalgancha erkin kod bera olasiz. Bosh maqsad hamma o'ta chaqirilgan ishlardagi ko'rsatkichlarga tahlil o'tsazishi kabi (latency hisobin berishi/sanab o'tishi).

```bash
# Barcha ochilgan olingan tizim bo'ylab fayllarini xaritasi chiqsin (o'sha zahoti o'zida ko'rsatadi)
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s %s\n", comm, str(args->filename)); }'

# Nom berib ularni hisob-kitob qilish (Ctrl-C bosingach sanash to'xtab chiqatadi)
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* { @[probe] = count(); }'
```

Xohishingizga qarab eBPF dasturlaringizni C tilida bemalol qo'llanuvchi [`bcc`](https://github.com/iovisor/bcc) texnologiyasi bilan va u bergan [foydali vosita](https://www.brendangregg.com/blog/2015-09-22/bcc-linux-4.3-tracing.html) ko'magidan unumdor ifodasita bo'lishiz qulay, chunki operatsiya bilan ishlatib berishi bilib `biosnoop` hisoblar latensiyasi chiqarishni ishlatsz. Ochiq holatlarda bo'lgan bo'lsangiz `opensnoop` tavsiya etildi.

`strace` "oddiy ishga hozir aytganingizda ketuvchi" holatligini ajaratsakda, `bpftrace` dasturi kamroq qo'shimcha yuklama (overhead) qidirishni yadro ichidan bo'yovchi chaqiruvli yozishingiz, tahlillar yaratmoqchi bo'lsangiz o'sha joyini almashtiradi. Lekin bilingki `bpftrace` da ishlash `root` huquqidan, faqat belgilangan fayllar bilan emas balki hamma yadro bo'ylab bo'lmog'i aytilgan holda bo'ladi. Dastur jarayonlarini kommand nomiga qarab filtrlashni buyursa bo'ladi:

```bash
# Kommand nomini filter qilish (Ctrl-C bilan tugatilishda ko'rinadi)
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* /comm == "bash"/ { @[probe] = count(); }'

# -c argumentiga qarata chaqirilgan kommandani bolayi kodi qidiriladi (cpid = bola jarayonni kodi)
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* /pid == cpid/ { @[probe] = count(); }' -c 'ls -la'
```

`-c` parametrini ko'rsatishingiz uni tezlik bilan cpid nomida ishlashi jarayon ishini ko'rsatgan holni xabar etishdi. Agar u bajarilib bitgan bo'lsa uni ustidagi statistikani hisobin yakunida ma'lum etadi.

### Tarmoqni Debag qilish

Tarmoqdagi nosozliklarini kuzatishga [`tcpdump`](https://www.man7.org/linux/man-pages/man1/tcpdump.1.html) va [Wireshark](https://www.wireshark.org/) yordamchilari yaxshigana paketni hisobotini olib keladi:

```bash
# Xabar oqim paketlarni manzil 80 dan olish
sudo tcpdump -i any port 80

# Wireshark ustidan ishlashiga pcap qilib yordamlashib qo'yish
sudo tcpdump -i any -w capture.pcap
```

HTTPS uchun ulangan ma'lumotlar bilan ma'lumki uncha emas, tcpdump o'qilmaydi. Ana o'sha yerda [mitmproxy](https://mitmproxy.org/) ishqida proxy kabi uskunalar HTTPS parollangan xatolar uchun ajoyib ishlarni topib ko'rsatadi. Internet tarmoq qatlamiga qaramligi qarab vebdasturlash vositalari HTTPS tarmoq joriy qismini tahlili tez so'rov/kodlari va natijalarini izlashingizda qulay bo'lishi aytib ketiladi.

## Xotira xatolarini Izlash

Xotira bilan bog'liq bo'lgan nosozliklar—bufer to'lib ketishi (buffer overflow), freydan o'tgandan kiyingiga bozorish (use-after-free) yoki xotira sizib chiqishi (memory leak)—debaga qila olinishi qiyin hal qilinuvchi narsa. Ular shunchaki buzilmaydi aksincha ularni vaqt ko'p ishlaganidan so'ng u yerlari ayblari xatolari boshlangandi.

### Sanitayzerlar (Sanitizers)

Qiziqarli xatolar usullaridan topish vositachisi aynan **Sanitayzerlar (sanitizers)** uskunaga kiradi, yozganingizni runtime tekshiradigan qismlari orqasida bajarib chiqadi. Namuna uchun, dunyoga mashqur bo'lib avjga olgan **AddressSanitizer (ASan)** dasturi borasida ko'rsatish mumkin:
- Bufer to'lib ketish xatolari (bu stek, xotira ajramasi doirasi bo'lishidir)
- Ajratilgan xotirani ishlatilmasidan xatolar (Use-after-free)
- Qaytish qiymatidan debag qilishni holati (Use-after-return)
- Xotira sizib chiqishi

```bash
# AddressSanitizer yordamida ishlash
gcc -fsanitize=address -g program.c -o program
./program
```

Yana bitta aytib o'tilishga kerakli jihati mavjud:

- **ThreadSanitizer (TSan)**: Bunga Thread muammoligini izlash qolgan (`-fsanitize=thread`)
- **MemorySanitizer (MSan)**: Qiymat bermagan bo'lsa nosog'ligini topadi (`-fsanitize=memory`)
- **UndefinedBehaviorSanitizer (UBSan)**: Aniq qilib aytmaganida arifmetikalikka nisbatan nosozligini hisoblaydi (`-fsanitize=undefined`)

Kompilyatsiyaga keraklikni topadi shuningdek rivojla-tish o'tirishga CI va uzluksiz o'z ishlarini davomiga to'liq ish tutishingiz ma'muru sanalardi.

### Valgrind: Qayta Kompilyatsiya qilish Ilojisiz bo'lganda

[Valgrind](https://valgrind.org/) xotirani buzadiganlar ustida turuvchi bo'lib vm darajasida yodini yoqishni talab bo'lmay kompilyatsiyasi ham bermay ko'rib chikqoladi:

```bash
valgrind --leak-check=full ./my_program
```

Uni keraklik ishlarini qila olish xulosalarini topishi:
- Tizimingda source xatosi ochmaganda
- Qayta qura bo'la ishlashingiz mumkini
- Sanitaserda hamma imkoni yo'q vositachi qidirilmay qolinsa o'shayni

Valgrind amaliyot olib yomonliklari izlab topsada siz ko'rgan profillarni ham barcha yollaridayin yaxshisini anglitishga imkoni juda bisyor bo'lgan!

## Debag qilishda SI (AI) dan foydalanish

LLM qiyinchiligi kam va oson kiritganligimiz bilan shundaki oddiy yozilgan debagchilarni ishlarini topila asboblarni kuchga ko'chardi.

**Suhbat model (LLM) lari yaxshi bajaradigan joyi:**

- **Tushunarsiz xatolarini ayta qilishi**: Qator va hato kodlarda yordim izlangandi maslahat tilini ochqoyishi tushuntirmoqda.
- **Til ustida til va ustidan chigilini debaga qo'shishingiz**: Ish bilan farqli muhitlar bo'lganlar (misoli C yoki Python daraja) shunga oiddan LLM izlashi juda qiyinib ko'rsatkichlar deb o'tgangin. Qisqa o'tqanda qaramligimiz ustalaridan bilishingiz masalan.
- **Nihoyatlar manbasigada muqarrarliga ko'ra ochiqlamochi**: "Men ishim joyidagi tizim shular shunaqa ekanligidan o'sha debog qilsam" - shu holati shunchaki ommabob narsaga boqilib berib qo'yilsachi yozishlar kelmoqda.
- **Avariyani ko'ra boshgan stack o'zining tahlilga (Analyzing crash dumps and stack traces)**: Stack'dan xabar bo'lsada xatoni so'rov qilib ko'ring nimalar bo'lganni bilinganida ko'rishdi.

> **Debag symbollar shartimi deysizmi**: Stack deb qo'yishlaringiz haqiqiy qo'llashingizda (binoq binarga -g bilan tayyor ko'rish uchun maxsusligi bilan shugʻurlanardi. O'tkaziladigan debug yordam symbollari odatda DWARF yozuvida bo'lishdi. Shu darajala ko'rsatkichi profilator (`-fno-omit-frame-pointer`) shundan to'g'ri bo'ladi agarda qilsak shundan. Ular yo'q u shuncha sonlar borasida (xotira joylaridan iboratligin aytmasa faqat bo'ladi). Albatta Compiled kodlarda bo'lganligik kabi talabi berilganiga (C++, Python).

**Ularni kamchilik va muommoligizni e'tidan bering:**
- Modellar to'g'ri gaplarni ham noo'rin va o'tkazib ishla-ko'payga yolg'onsi aytishi bililadi.
- Noto'g'ri ishlashgan muqobillash bilan taklif qilishi imkoni ko'p yozilib o'tganda borildi.
- Tavsiyasin qaytadan real bor narsadan debag jarayonlarini amaliyroqda bering izlashingizni tayinlay.
- Qilgan ishingizning unchalikga zo'rga ishlardan bo'lgan hisob oling izlashingiz emas kodsiz asbob bo'linganda o'tilmadi

> Aslida yuqoridia narsalardan [sun'iy idrok bo'limilarida](/2026/development-environment/#ai-powered-development) shug'urlanma vositasi kursni aytadi emas. Bunda qismi bilan alohida o'z ishi va yondashgani haqda izlandi.

# Profillash (Profiling)

Kodingiz siz kutgandek yaxshi natijada o'sha jarayonlarda qilib chiqilsada bu ko'p prosessordan, xotiradan sarfida olindish bo'lsangiz yomon odat bo'ldi. Dasturlash kurslariga nisbatan _O_ moshinasi dars kabi ishlagan qismi faqat ularga profaylligini bo'liniga o'tmaydigan ekan, izlar o'tolmadingizdan deyilishi muammoladi. Buning sababi o'zi, [vaqtli optimallashtirgan yomon qator asosiya ekaniga izlanmish](https://wiki.c2.com/?PrematureOptimization) asos bo'lib kelganda profiling bo'lim va tekshirgan monitorlardan bilimingiz ko'p darajada yoziladi asboblar berib keldi. Siz bu ustunli va joying qismini to'g'irlab optimal qilinishingizni taqdim qilguvshdiradi.

## Vaqtni Hisoblash

Ko'rsatish ishida unumdorlik aniq debag qilish bo'limidan usularini ajratish qismidir bo'lganda, ularning oddigina joy qilib ish bajariligacha nechtaligini `print` da olish ehtimoli kamligicha u yordamini qilib beringani qulaydir!

Lekin muammolarni ba'zida soatni hisoblash holatiga o'zingizda adashiga kompyuterlar yechishga jarayon ishdan yoki muommalarga qo'shimchisida muvafqqiyatiga tayyordilar u ishlardi so'rab boqiladi qismlar `time` ko'ratgiga qo'shi:

- **Real** - Real hisoblash u kompyuter uchun jarayongachi ishdan tugatilganini qilib kutib turishingisani bo'ldi.
- **User** - Barcha kodi user tomondan bo'ladi protsessor ishlani bajarib qaytadi.
- **Sys** - Yadroda sistem tomondan (kernel code) ishlagani protsessor tomonidan yozilmadi.

```bash
$ time curl https://missing.csail.mit.edu &> /dev/null
real	0m0.272s
user	0m0.079s
sys	    0m0.028s
```

Hozirgi bu komanda so'rovi borligi natijani 300 millisoniyadan tezda qo'l ishini yakunlagandir lekin aniq 107ms hiso CPU'dan to'liqlardi. Boshqa farqi tarmog'ni ulangan deb xulosa bilan qoldi!

## Resurslarni Kuzatish (Resource Monitoring)

Performansen unumdorligi sabab ba'zi ustidan maqsad ishlarining aniqligin dasturni asl yeb olinishi kuzatish birda muhim hisobladi. Manba qo'ymaganga yoki kamligi sababli yomon izlay o'tandi u bo'lish o'zi sekinlikda.

- **Umumiy Kuzatish**: [`htop`](https://htop.dev/) ishning turlari qulay qilib statistik o'zini yaxshilanmish `top` holatli. Buniki bindlar: `<F6>` ro'yxati sort qilishga jarayolari uchun ketyapdi, `t` bilan treega, `h` kelsa bo'lsa iplarni o'chiq-yonasiz. Bizga ham yana o'sha kabi unumlar bo'lgan [`btop`](https://github.com/aristocratos/btop) ham yoqadi o'rtidan ishlarga ko'ra hamma.
- **I/O Operatsiyalari**: Kuzatishi tezligi tiriklarcha ustida ma'luot yo'li [`iotop`](https://www.man7.org/linux/man-pages/man8/iotop.8.html) da tasnif izlay ko'radi.
- **Xotira o'lsami (Memory Usage)**: Xotiraga [`free`](https://www.man7.org/linux/man-pages/man1/free.1.html) o'zi umumqilib topa oldik ishlayapti qoldi qilingnchi.
- **Ochiq ishlangan fayllari (Open Files)**: [`lsof`](https://www.man7.org/linux/man-pages/man8/lsof.8.html) vositasiz qanaqa narsa nimagada kodi qo'yilganligi haqila. Foydali jihati ayirib olarimizni bo'lib ishimiz aniqlasish yozilar ishlarni ochgani sanalasi u.
- **Tarmoq usullari (Network Connections)**: Qayerga borilishi o'ylaydiga o'ringa [`ss`](https://www.man7.org/linux/man-pages/man8/ss.8.html) turlarni u qidirganda tarmonq holid qolar u asan keladiga shudur ko'p ma'lumga joyini `ss -tlnp | grep :8080`.
- **Tarmoqning Ishlatilishi (Network Usage)**: Boshqarish interfeys ishladi terminal ish bo'lishi bu ustidan qo'shimcha u process hisobida yaxshisini [`nethogs`](https://github.com/raboof/nethogs) va u bopni [`iftop`](https://pdw.ex-parrot.com/iftop/) foydasi yordamidir uslublarni kuzatadi bo'lgan.

## Unumdorlik ma'lumotlarini viziullashtirish (Visualizing Performance Data)

Odamlar ro'yxati ko'rganida ular graph holati raqamlashda ishlarni tahlil oladi qilib sekin yoki yuqorilashi qismini ishlaganda chizma tuzishini foydasiz hisoblab qoldik o'rtasida yaxshilik namoyon borildi yo'q joylariga qolib yo'qildi farqi topiladi raqamdan xulosam.

**Tahlilchilar qulay bo'ldimi ish qilish maqsad qilishida**: Dasturlarga jadvallab bera qilib tahlillar ko'rish qismida qolib izohta izlash oson o'tqizi unga log joyidaga print qo'shila ularga debagchimi oson bo'lgandi ishlandi ko'rinsligi tushunish. `1705012345,42.5` yozishingiz farqli so'z bilan joydilarida ish bo'lgandan csv ga. U yerini izlanmasa formatdan JSON hisobi ko'ngilcha tushirila tushish u borishini qilinur [ma'lumotidan farqiga chiroyli yuz](https://vita.had.co.nz/papers/tidy-data.pdf)

**Quvvatli qilishta gnuplot**: U osongina aslini olardi holatlarni interfeys yozish bilan [`gnuplot`](http://www.gnuplot.info/) hisob fayli grafik o'qilgagina chiqqan holdan ish bajarib u:

```bash
# Oddiy tarzda chizma ustida comma va ishlarni yozasi bilan chiqarib bera qiling u
gnuplot -e "set datafile separator ','; plot 'latency.csv' using 1:2 with lines"
```

**Matplotlib va ggplot2 tahlildagi iterativ jarayonlar (Iterative exploration)**: U yanada ko'ra u ulanishga yechim hisobi unga qarab uzga tilining [`matplotlib`](https://matplotlib.org/) (Pythondaginichi) bilan o'sha tilde [`ggplot2`](https://ggplot2.tidyverse.org/) izlasih maqsad bera xulosa hisobga aylanadi bo'lgan bo'lib ish. Tasvir qilgani ko'ra joylangan shartda tezda toifada fasetlar ishlanga tahlilchilarida ehtimolda izlab vaqti uchi (latensiyali qolib holatlari ishini boshlanma va kategoriysi tahlil bo'yicha) ustini o'rganilishi yasharin ochiq topilshi.

**Foydalanish namunasi bo'lgan:**
- Chiqarib olish tezligi yozilgan uzilishingni topa olmaganda yomonlash hisobi sekinligini aytmaymikaniga qarab ko'rib bo'linishingan ish u kelsa sababligacha kelindi.
- Massiv va element oshigach farqi borilgan kompleks holati ishidan katta hajmidagi vektorga ko'paygacha graf xabar chiqildi
- Kategoriyalik dimension hisobdada kelingansicha uni har xil yechish ishlangan turlicha jarayondayda o'sha joydan katta ishni hisobi yoq deb o'zinga ajrasib bo'linib turardi.

## CPU profayllash (CPU Profilers)

Barcha vaqt bilan holati biron kishi aytgandaga _profillash_ qilinadi u maqsada izlasiz demishdi o'zi _CPU profiler_ bo'lmoqga to'gri hisobiga:

- **Izlashda qarayotgan izlaydiganlar (Tracing profilers)** izlayman o'zi yordam bor chaqirsa to'plam bilan barini har birisini ko'rib yozasan borasan deya topdi kodi u dasturingdan.
- **Qisqa vaqtli ajratadi ko'rishda (Sampling profilers)** holati ma'lum vaqtning har daqiqigidiga namuna olish qilini unga dasturning qanday vaqtdagi ishidagini (stackini) oladi namuna qo'shilish hisobi ko'rgani ish yozdi ko'paydi qachonga!

Ishning samarali ekanida oddiy bo'lgangalar unumiga yengilligi hisoblanganga bo'ladi pastligi ishlari ustida ish berar u ishda ish ko'rinishi tavsiyamiz ishlanish u namunalidirish bo'lmoqchi deb!

### perf: namunaviy ish o'zi

Qidiruv tizimi bo'lgancha uni qolib aslahali uzatish bo'ldigacha [`perf`](https://www.man7.org/linux/man-pages/man1/perf.1.html) Linux o'tishi asosiga qo'yildi kompilyatsiyasini qaytalasiya uni boshida debishni tekshirilmay yozsa bo'ldi.

Tahlil vaqtlarning umumiy ishlari unumiga qayeriga etiladi yozilgan degan haqiqatin ko'rinib `perf stat` chiqib u holatidan u izlatdi:

```bash
$ perf stat ./slow_program

 Performance counter stats for './slow_program':

         3,210.45 msec task-clock                #    0.998 CPUs utilized
               12      context-switches          #    3.738 /sec
                0      cpu-migrations            #    0.000 /sec
              156      page-faults               #   48.587 /sec
   12,345,678,901      cycles                    #    3.845 GHz
    9,876,543,210      instructions              #    0.80  insn per cycle
    1,234,567,890      branches                  #  384.532 M/sec
       12,345,678      branch-misses             #    1.00% of all branches
```

Bu tahlillar insonlarga real deb topilagan narsalar katta raqamni hisobda qarib bo'lganda o'qishi yomon qoldi deya ma'noniga qarshi ishlangandi raqamlarga vizual holati yo'q u tabiatan bor. Shuning [flame graphlarlik (Flame graphs)](https://www.brendangregg.com/flamegraphs.html) vizuallashtirma yuzasida yaxshiga kelgan chiroyli bo'lishi ishlay topganga sababi tez ish. Flame graph o'zi iyerarxiyalik ishga y o'si bo'ylab funksiya deb hisoblari u orqasidandan ish bajarsa u o'shanga x bo'linganda qaram ish holatini moliqda vaqt proporsiyaniga chiqardi izlab ishida topildi. Ular orqasigaga uzoom hisobingda ishkana izlay oling qo'l urishda ishga yoqin!

[![FlameGraph](https://www.brendangregg.com/FlameGraphs/cpu-bash-flamegraph.svg)](https://www.brendangregg.com/FlameGraphs/cpu-bash-flamegraph.svg)

Tasavvur chizma `perf` tahlillash asosiy maqsad topilishida qiling:

```bash
# Ma'lumoti joyi profile ga kirar saqlashi
perf record -g ./my_program

# flame graphni tizimidan topilsa (flamegraph dagi uni skriptni berish yozishi kerekliga bo'lganligi kerak deb bo'lsinda ishidan borib qilinajak u)
perf script | stackcollapse-perf.pl | flamegraph.pl > flamegraph.svg
```

> Ko'rib web-darsturga hisob izlani chiqardi ishta [Speedscope](https://www.speedscope.app/) qarab interactiv ayti asbobi yo unversal tahlili [Perfetto](https://perfetto.dev/) bopda tavsiya ko'rishlaga yoyilib bilar ko'rilsin deb.

### Valgrind'ning Callgrind dagi vositasi: the tracing profiler ekani

Biz hisobin callarni dasturni buyruqigida raqamlari soni aytib ketadigan xizmat qo'shimchasi borki uni [`callgrind`](https://valgrind.org/docs/manual/cl-manual.html) berolishi unumli holatan profilator u uchin bo'ladi va shundan yozadiganlari qilinganiga namunalarga nisbati u qo'shimchasi izlab u yozuv ko'rdi chiquvchi chaqirishlarda aniqlaga (caller, callee) bir ishini ko'rib qat'iy yozadi izladi uni bo'la chiqar u!

```bash
# Dastur ishidan uni qo'shilgan hollangan yozgandi u qilib 
valgrind --tool=callgrind ./my_program

# uni ko'rib chaqqan asbobni (GUI bo'ladan callgrind_annotate (text) yo ishlangana borgan o'ziniki uning u kcachegrind)
callgrind_annotate callgrind.out.<pid>
kcachegrind callgrind.out.<pid>
```

U sekin ishidan profiling (sampling) bo'lib ketsa-da to'gri topilsa ishga kerak ishiga ajoyib callarni haqiqatdan agar deb bilsa u nima ish bilan qilib uni asbob o'zingacha kesh ishlarga moslashtiryaptan `--cache-sim=yes` qilishdan u yoq ishonchga ishondi chindan!

> Ko'plabi tilini ishlattirsagiz ko'proq u yerga o'zlashtirmasga vositasi u o'zi qo'shilgan profillerlar ishida uchib ko'rinmagandigacha bo'lar ekan. Uni o'yish qisinda Pithon o'zi uchun bor bo'lgani [`cProfile`](https://docs.python.org/3/library/profile.html) va [`py-spy`](https://github.com/benfred/py-spy), Goga debaglanga yozgan bu bilan [`go tool pprof`](https://pkg.go.dev/cmd/pprof), hamda hamma ishi ehtimolli xohish kompilyatsiya topa barcha `[cargo-flamegraph](https://github.com/flamegraph-rs/flamegraph) (Rustdanga!).

## Xotira profillatorlari

Odamlarda xotiralarni uzilishi hisobi xotira sizib chiqishi qaerdin buzilganini o'tkan ishda ma'lum u qilib profillator sizboga topayab ishida o'rganilagan holga oqlandi.

### Valgrind Massifi

Heap ishi xotiraini yodlanganiga maqoladiki ish qilishini o'zni masslab profilla unida [`massif`](https://valgrind.org/docs/manual/ms-manual.html):

```bash
valgrind --tool=massif ./my_program
ms_print massif.out.<pid>
```

Orqali xotirasini kattalashtirib yodi joyidagi vaqtda izlab tekshirganligin tahlildari ishidami bu u holatini yoq joy qilib ko'rib bering!

> Pithonga deb unum topmochi xotira ustidagi bu line-by-line ish ko'rib ajralishlilar profilator [`memory-profiler`](https://pypi.org/project/memory-profiler/) bu asbobi ekan u zo'rgina tahlil.

## Benchmark qilish (Benchmarking)

Ikkita alohidadagin yoki ish ko'rinishi nimasidan unumini izlar uni turdagi [`hyperfine`](https://github.com/sharkdp/hyperfine) orqasiki benchmark xabar tahlili command-line kodi unga o'rin olab u ko'ratish qoldi bor o'zi!

```bash
$ hyperfine --warmup 3 'fd -e jpg' 'find . -iname "*.jpg"'
Benchmark #1: fd -e jpg
  Time (mean ± σ):      51.4 ms ±   2.9 ms    [User: 121.0 ms, System: 160.5 ms]
  Range (min … max):    44.2 ms …  60.1 ms    56 runs

Benchmark #2: find . -iname "*.jpg"
  Time (mean ± σ):      1.126 s ±  0.101 s    [User: 141.1 ms, System: 956.1 ms]
  Range (min … max):    0.975 s …  1.287 s    10 runs

Summary
  'fd -e jpg' ran
   21.89 ± 2.33 times faster than 'find . -iname "*.jpg"'
```

> Agarki u brauzering u yo joylari Web rivoji vositaridan ajoyibni uni topilar ishdan ishlab izla bor! Uniki u `[Firefox Profiler](https://profiler.firefox.com/docs/)` u shuningda dev yo qo'ya aslahini ko'rib bo'lib `[Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/rendering-tools)`.

# Mashqlar

## Nosozliklarni izlash (Debugging)

1. **Saralash algoritmini debag qiling**: Quyidagi psevdokodda `merge_sort` yozilgan, ammo noto'g'ri ishlash xatosi u erda yotdi tahriri. Tilingizga yoqqanidagini implement qilasizmi keyin ishlatgan qo'lli debagger yordamcha uni (ide, pdb, gdb, lldb u kabi bironnisi) shundan foydalanb kodingni to'ziligi qilib u xatosin topishinga u ko'proq yoram qilganga izlardi topisang u bo'ldi.

   ```
   function merge_sort(arr):
       if length(arr) <= 1:
           return arr
       mid = length(arr) / 2
       left = merge_sort(arr[0..mid])
       right = merge_sort(arr[mid..end])
       return merge(left, right)

   function merge(left, right):
       result = []
       i = 0, j = 0
       while i < length(left) AND j < length(right):
           if left[i] <= right[j]:
               append result, left[i]
               i = i + 1
           else:
               append result, right[i]
               j = j + 1
       append remaining elements from left and right
       return result
   ```

   `merge_sort([3, 1, 4, 1, 5, 9, 2, 6])` uning masalasining qatiy ish qiymatlar vektori u hisoblida natija olish `[1, 1, 2, 3, 4, 5, 6, 9]` aylandi chiqadi qo'yganini qiling teshirilarishini to'xtash nuqtasididan yurgizganda ustini qadamini bosing bu u muammosisini izlashiga ko'zlani.

1. Nosozlik izlashning orqasiqidan yozib holatga `rr` bop ishlayu u borman uni buzingach u ko'ring o'zi uchun fayli bilan `corruption.c` ga u holatdan o'zinging u saqlab olinga bu bilar!
    [`rr`](https://rr-project.org/):

   ```c
   #include <stdio.h>

   typedef struct {
       int id;
       int scores[3];
   } Student;

   Student students[2];

   void init() {
       students[0].id = 1001;
       students[0].scores[0] = 85;
       students[0].scores[1] = 92;
       students[0].scores[2] = 78;

       students[1].id = 1002;
       students[1].scores[0] = 90;
       students[1].scores[1] = 88;
       students[1].scores[2] = 95;
   }

   void curve_scores(int student_idx, int curve) {
       for (int i = 0; i < 4; i++) {
           students[student_idx].scores[i] += curve;
       }
   }

   int main() {
       init();
       printf("=== Initial state ===\n");
       printf("Student 0: id=%d\n", students[0].id);
       printf("Student 1: id=%d\n", students[1].id);

       curve_scores(0, 5);

       printf("\n=== After curving ===\n");
       printf("Student 0: id=%d\n", students[0].id);
       printf("Student 1: id=%d\n", students[1].id);

       if (students[1].id != 1002) {
           printf("\nERROR: Student 1's ID was corrupted! Expected 1002, got %d\n",
                  students[1].id);
           return 1;
       }
       return 0;
   }
   ```

   Xatomi shu ishdi yozib bo'lish uni topilishi buni uning buzilyapti qoshiz uning xabarin ko'rinishi o'ylani ustini o'zinging ishida u `gcc -g corruption.c -o corruption` deb komplaylangan run ishini unga o'rin ko'rmagan buzilib tushganidan u faqat bir o'rgandi funksiya bu buzayotdi ishlar `student 0` ko'tarilganida bo'lgani u `rr record ./corruption` uzi qoldi uningda va `rr replay`. Keling aytchi funksiyasida yozilmadi xatocini qo'yamiz va tahrishi orqasi ish qilmoqchi o'shanda qayerda nmasidan o'zgardi ayb uning ekan kuzatish nuqtasi (`students[1].id` da) borisizlar. Yana `reverse-continue` qaytirab.

1. Olib sanitayzer bilan izlangan yozilishi xatoligini topadigan debag AddressSanitizer `uaf.c` deb olinga:

   ```c
   #include <stdlib.h>
   #include <string.h>
   #include <stdio.h>

   int main() {
       char *greeting = malloc(32);
       strcpy(greeting, "Hello, world!");
       printf("%s\n", greeting);

       free(greeting);

       greeting[0] = 'J';
       printf("%s\n", greeting);

       return 0;
   }
   ```

   Endini asosi nima ko'rinmadi undan bilishing o'zi ishin topildiku bu uaf dastangandan keyin u `gcc uaf.c -o uaf && ./uaf` uni ishlab. Ammo ko'rip AddressSanitizer orqasida: `gcc -fsanitize=address -g uaf.c -o uaf && ./uaf`. Ushbularda report topib shuni tahrirlashi asosi nega shundan unumdorini ayta?

1. MacOS dagi `dtruss` o'shalisi (Linuxda) `strace` tizim chaqiruvida yozgandi u qayerligida chaqiradi bir qarangchi u `ls -l` aytib. Kompleksroq yozilganni orqasida chiqadigan barchasiga yana chaqirga yoritasiz bo'lib chaqirsami ocholadiku faylarni farqin tahriri ko'rasa bilamiza.

1. Biron yashar tilida izlo'shiLLM aytilgan ishmas deb bu shuningda o'zi debig izla kompyuter u izlay qilib bozorga chiqazshi (bu u izlasaligiga o'rnidagi rust va c++ bu u dildagi xatosi so'rasi) va bu chiqar u sanitszer yo `strace` borishi bergangini tushuningini ishlatiz bilizsiz!

## Profillash (Profiling)

1. Performans hisobidan xulosa izlamoqliga bosh statistinkani topsa `perf stat` da o'zimiz asboki bilganga. Unumdor counter lari aytingiz nimagacha?

1. Profile ma'lumoti izlanga topila saqlandi bu yozgandi qilib oltingi orqadan `slow.c`:

   ```c
   #include <math.h>
   #include <stdio.h>

   double slow_computation(int n) {
       double result = 0;
       for (int i = 0; i < n; i++) {
           for (int j = 0; j < 1000; j++) {
               result += sin(i * j) * cos(i + j);
           }
       }
       return result;
   }

   int main() {
       double r = 0;
       for (int i = 0; i < 100; i++) {
           r += slow_computation(1000);
       }
       printf("Result: %f\n", r);
       return 0;
   }
   ```

   Debagin ma'lumotlar usti olib bilingish u kompliyalashi ishi: `gcc -g -O2 slow.c -o slow -lm`. Ularda vaqt uchi qayiga bilaman ishni qolinglar u `perf report` da u `perf record -g ./slow` bilan izida ko'rib o'qi yozilib o'sha! Skriptlarni yozish qilingiz orqadami flame graph da o'zingni chiqarma topsin!

1. Keling ikki xil qatlam u usullar ko'rsatkichi ikkinchisni farqhlarini `hyperfine` bilan u birinchi bir yechimi benchlarini chiqish (`find` yo `fd`, `grep` ga daxon bo'lgangan `ripgrep`, asbob qilib u yoq o'zingizki hisob bering!).

1. Uni boqish bor holiga ishladilar usular bilish CPU jarayoni izlanga aytadi topib ber uni u bo'lgan kommandali narsa `htop` og'irchi shunday. Unga uni yagona bir mallashti ko'ratar u jarayni holati limitlar bilan u `taskset` yordamishi yoziliga uni topilariz: `taskset --cpu-list 0,2 stress -c 3`. Xotira uzlasihida sababi shunchaki izla uni farqinchi nega 3 CPU ko'rinmangin stress dagi?

1. O'qingchi muommani izlangchi kimdir olib bo'layaptisida o'ylangi portnida kuzating shularnida eshitishdan o'rniga. Ehtimoli port u jarayon hisobida uni topsangz bilimi uni qo'yasi `python -m http.server 4444` bu 4444 portga. Ko'rishing yodiga terminal bering uning ishlatirsa u izidan qidirchi nimasi bo'lganda `ss -tlnp | grep 4444` qilsiz qarab u yopingshi uni ishladiga `kill <PID>`.