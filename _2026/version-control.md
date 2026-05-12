---
layout: lecture
title: "Versiyalarni boshqarish va Git"
description: >
  Git'ning ma'lumotlar modelini va versiyalarni boshqarish hamda hamkorlikda ishlash uchun Git'dan qanday foydalanishni o'rganing.
thumbnail: /static/assets/thumbnails/2026/lec5.png
date: 2026-01-16
ready: true
video:
  aspect: 56.25
  id: 9K8lB61dl3Y
---

Versiyalarni boshqarish tizimlari (VCS) manba kodidagi (yoki boshqa fayl va kataloglar to'plamidagi) o'zgarishlarni kuzatish uchun ishlatiladigan vositalardir. Nomi aytib turganidek, bu vositalar o'zgarishlar tarixini saqlashga yordam beradi; bundan tashqari, ular hamkorlikda ishlashni osonlashtiradi. Mantiqiy jihatdan, VCS'lar katalog va uning tarkibidagi o'zgarishlarni bir qator _snapshot_'lar sifatida kuzatib boradi, bunda har bir snapshot eng yuqori darajadagi katalog ichidagi fayllar/kataloglarning butun holatini qamrab oladi. VCS'lar shuningdek, har bir snapshot'ni kim yaratganligi, har bir snapshot bilan bog'liq xabarlar va shunga o'xshash metama'lumotlarni saqlaydi.

Nima uchun versiyalarni boshqarish foydali? Hattoki o'zingiz yolg'iz ishlayotganingizda ham, u sizga loyihaning eski snapshot'larini ko'rishga, nima uchun ma'lum o'zgarishlar qilinganligi jurnalini yuritishga, parallel tarmoqlarda ishlashga va boshqa ko'plab ishlarni bajarishga imkon beradi. Boshqalar bilan ishlaganda esa, u boshqa odamlar nimalarni o'zgartirganini ko'rish, shuningdek, parallel dasturlashdagi ziddiyatlarni hal qilish uchun bebaho vositadir.

Zamonaviy VCS'lar quyidagi kabi savollarga osonlik bilan (va ko'pincha avtomatik ravishda) javob topishga imkon beradi:

- Bu modulni kim yozgan?
- Ushbu ma'lum faylning ushbu ma'lum qatori qachon tahrirlangan? Kim tomonidan? Nima uchun tahrirlangan?
- So'nggi 1000 ta reviziya davomida qachon/nima uchun ma'lum bir unit test ishlashdan to'xtadi?

Boshqa VCS'lar ham mavjud bo'lsa-da, **Git** versiyalarni boshqarish uchun amaldagi standart (de facto) hisoblanadi. Ushbu [XKCD komiksi](https://xkcd.com/1597/) Git'ning obro'sini aks ettiradi:

![xkcd 1597](https://imgs.xkcd.com/comics/git.png)

Git'ning interfeysi yetarlicha mukammal bo'lmagan abstraksiya (leaky abstraction) bo'lgani uchun, Git'ni yuqoridan pastga (interfeysidan / buyruqlar satridan boshlab) o'rganish ko'p chalkashliklarga olib kelishi mumkin. Bir nechta buyruqlarni yodlab olish va ularni sehrli afsun sifatida tasavvur qilish, hamda biror narsa noto'g'ri ketganda yuqoridagi komiksdagi yondashuvga amal qilish oson.

Garchi Git interfeysi xunuk bo'lsa-da, uning asosiy dizayni va g'oyalari chiroyli. Xunuk interfeysni _yodlash_ kerak bo'lsa, chiroyli dizaynni _tushunish_ mumkin. Shu sababli, biz Git'ni uning ma'lumotlar modelidan boshlab va keyinroq buyruqlar satri interfeysini qamrab olgan holda pastdan yuqoriga qarab tushuntiramiz. Ma'lumotlar modeli tushunilgandan so'ng, buyruqlarni ular asosiy ma'lumotlar modelini qanday o'zgartirishi orqali yaxshiroq tushunish mumkin.

# Git'ning ma'lumotlar modeli

Git'ning ixtirosi uning yaxshi o'ylangan ma'lumotlar modelida bo'lib, u versiyalarni boshqarishning tarixni saqlash, tarmoqlarni qo'llab-quvvatlash va hamkorlik qilishga imkon berish kabi barcha ajoyib xususiyatlarini ta'minlaydi.

## Snapshot'lar

Git biror yuqori darajadagi katalog ichidagi fayllar va kataloglar to'plami tarixini snapshot'lar seriyasi sifatida modellashtiradi. Git terminologiyasida fayl "blob" deb ataladi va u shunchaki baytlar to'plamidir. Katalog esa "daraxt" deb ataladi va u nomlarni blob'lar yoki daraxtlarga xaritalaydi (shuning uchun kataloglar ichida boshqa kataloglar bo'lishi mumkin). Snapshot esa kuzatilayotgan eng yuqori darajadagi daraxtdir. Masalan, bizda quyidagicha daraxt bo'lishi mumkin:

```text
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```

Eng yuqori darajadagi daraxt ikkita elementni o'z ichiga oladi: "foo" daraxti (u o'z ichida bitta "bar.txt" blob'ini saqlaydi) va "baz.txt" blob'i.

## Tarixni modellashtirish: snapshot'larni bog'lash

Versiyalarni boshqarish tizimi snapshot'larni qanday bog'lashi kerak? Bitta oddiy model tarixni chiziqli qilib yaratish bo'lishi mumkin. Tarix vaqt tartibida joylashgan snapshot'lar ro'yxati bo'ladi. Ko'pgina sabablarga ko'ra, Git bunday oddiy modeldan foydalanmaydi.

Git'da tarix snapshot'larning yo'naltirilgan asiklik grafidir (DAG). Bu qandaydir murakkab matematik atama bo'lib tuyulishi mumkin, ammo qo'rqmang. Bu shunchaki Git'dagi har bir snapshot o'zidan oldingi snapshot'lar to'plamiga, ya'ni "ota-ona" (parents) larga ishora qilishini anglatadi. Bu yagona ota-ona emas (chiziqli tarixda bo'lgani kabi), balki ota-onalar to'plamidir, chunki bitta snapshot bir nechta ota-onadan kelib chiqqan bo'lishi mumkin, masalan, ikkita parallel dasturlash tarmog'ini birlashtirish (merge) natijasida.

Git bu snapshot'larni "commit" deb ataydi. Commit'lar tarixini vizualizatsiya qilish taxminan shunday ko'rinishi mumkin:

```text
o <-- o <-- o <-- o
            ^
             \
              --- o <-- o
```

Yuqoridagi ASCII tasvirlarda `o` lar alohida commit'larga (snapshot'larga) mos keladi. O'qlar har bir commit'ning ota-onasiga ishora qiladi (bu "keyin keladi" emas, balki "oldin keladi" munosabatidir). Uchinchi commit'dan so'ng tarix ikkita alohida tarmoqqa bo'linadi. Bu, masalan, bir-biridan mustaqil ravishda parallel ishlab chiqilayotgan ikkita alohida xususiyatga mos kelishi mumkin. Kelajakda bu tarmoqlar har ikkala xususiyatni o'z ichiga olgan yangi snapshot yaratish uchun birlashtirilishi mumkin, va buning natijasida quyidagicha yangi tarix hosil bo'ladi, bu yerda yangi yaratilgan birlashtirilgan commit qalin qilib ko'rsatilgan:

<pre class="highlight">
<code>
o <-- o <-- o <-- o <---- <strong>o</strong>
            ^            /
             \          v
              --- o <-- o
</code>
</pre>

Git'da commit'lar o'zgarmasdir (immutable). Biroq, bu xatolarni to'g'irlab bo'lmaydi degani emas; shunchaki commit tarixidagi "tahrirlar" aslida mutlaqo yangi commit'larni yaratadi va havolalar (quyiga qarang) yangilariga ishora qilish uchun yangilanadi.

## Ma'lumotlar modeli, psevdokod sifatida

Git'ning ma'lumotlar modelini psevdokodda yozib ko'rish foydali bo'lishi mumkin:

```text
// a file is a bunch of bytes
type blob = array<byte>

// a directory contains named files and directories
type tree = map<string, tree | blob>

// a commit has parents, metadata, and the top-level tree
type commit = struct {
    parents: array<commit>
    author: string
    message: string
    snapshot: tree
}
```

Bu tarixning toza va oddiy modelidir.

## Obyektlar va kontentni manzillash

"Obyekt" bu blob, daraxt yoki commit'dir:

```text
type object = blob | tree | commit
```

Git'ning ma'lumotlar omborida barcha obyektlar [SHA-1 xeshi](https://en.wikipedia.org/wiki/SHA-1) orqali kontent bilan manzillanadi.

```text
objects = map<string, object>

def store(object):
    id = sha1(object)
    objects[id] = object

def load(id):
    return objects[id]
```

Blob'lar, daraxtlar va commit'lar shu tarzda birlashtiriladi: ularning barchasi obyektlardir. Ular boshqa obyektlarga havola berganda, ularni haqiqatda diskdagi ko'rinishida _o'z ichiga olmaydi_, balki ularga xeshlari orqali havola qiladi.

Masalan, [yuqoridagi](#snapshotlar) misol katalog tuzilmasi uchun daraxt (`git cat-file -p 698281bc680d1995c5f4caaf3359721a5a58d48d` yordamida vizualizatsiya qilingan) quyidagicha ko'rinadi:

```text
100644 blob 4448adbf7ecd394f42ae135bbeed9676e894af85    baz.txt
040000 tree c68d233a33c5c06e0340e4c224f0afca87c8ce87    foo
```

Daraxt o'z ichiga olgan elementlariga, `baz.txt` (blob) va `foo` (daraxt) ga ko'rsatkichlarni saqlaydi. Agar biz `baz.txt` ga mos keluvchi xesh bilan manzillangan kontentga `git cat-file -p 4448adbf7ecd394f42ae135bbeed9676e894af85` bilan qarasak, quyidagini ko'ramiz:

```text
git is wonderful
```

## Havolalar

Endi, barcha snapshot'lar o'zlarining SHA-1 xeshlari orqali aniqlanishi mumkin. Ammo bu noqulay, chunki odamlar 40 ta o'n oltilik tizimdagi belgilardan iborat qatorlarni eslab qolishga usta emas.

Git'ning bu muammoga yechimi SHA-1 xeshlari uchun "havolalar" deb ataluvchi inson o'qiy oladigan nomlarni taqdim etishdir. Havolalar commit'larga ko'rsatkichlardir. O'zgarmas (immutable) bo'lgan obyektlardan farqli o'laroq, havolalar o'zgaruvchandir (yangi commit'ga ishora qilish uchun yangilanishi mumkin). Masalan, `master` havolasi odatda asosiy ish tarmog'idagi oxirgi commit'ga ishora qiladi.

```text
references = map<string, string>

def update_reference(name, id):
    references[name] = id

def read_reference(name):
    return references[name]

def load_reference(name_or_id):
    if name_or_id in references:
        return load(references[name_or_id])
    else:
        return load(name_or_id)
```

Buning yordamida, Git tarixning qaysidir bir qismiga uzundan-uzoq o'n oltilik satr bilan emas, balki "master" kabi odam o'qiy oladigan nomlardan foydalanib havola qilishi mumkin.

Yana bir detal shundaki, biz yangi snapshot yaratganimizda, u nimaga nisbatan olinganligini (commit'ning `parents` maydonini qanday o'rnatishimizni) bilishimiz uchun ko'pincha tarixda "hozir qayerdaligimiz" haqida tushunchaga ega bo'lishni xohlaymiz. Git'da o'sha "hozir qayerdamiz" degan tushuncha "HEAD" deb ataluvchi maxsus havoladir.

## Repozitoriylar

Nihoyat, biz Git _repozitoriysi_ (taxminan) nima ekanligini ta'riflashimiz mumkin: bu `objects` (obyektlar) va `references` (havolalar) ma'lumotlaridir.

Diskda Git butunlay faqat obyektlar va havolalarni saqlaydi: Git'ning ma'lumotlar modeli faqat shulardan iborat. Barcha `git` buyruqlari obyektlar qo'shish va havolalarni qo'shish/yangilash orqali commit DAG'ini (yo'naltirilgan asiklik grafini) qandaydir tahrirlashga mos keladi.

Har qanday buyruqni kiritganingizda, o'ylab ko'ringki bu buyruq ostidagi graf ma'lumotlar tuzilmasida qanday o'zgartirishlar qilmoqda. Va aksincha, agar siz commit DAG'ida biror o'zgartirish kiritmoqchi bo'lsangiz, masalan, "commit qilinmagan o'zgarishlarni bekor qilish va 'master' havolasini `5d83f9e` commit'ga o'tkazish", bu uchun ham, ehtimol, buyruq mavjud (masalan, ushbu holatda, `git checkout master; git reset --hard 5d83f9e`).

# Staging area

Bu ma'lumotlar modeliga bevosita bog'liq bo'lmagan yana bir tushuncha, lekin u commit'larni yaratish uchun mo'ljallangan interfeysning bir qismidir.

Siz yuqorida ta'riflanganidek, snapshot yaratishni ishchi katalogning _joriy holatiga_ asoslangan yangi snapshot yaratadigan "snapshot yaratish" buyrug'i bo'lishi deb o'ylashingiz mumkin. Ba'zi versiyalarni boshqarish vositalari shunday ishlaydi, ammo Git emas. Bizga toza snapshot'lar kerak, va doim ham mavjud holatdan snapshot yaratish eng to'g'ri tanlov bo'lmasligi mumkin. Masalan, ikkita turli xususiyat qo'shganingizni va bittasi faqat birinchi xususiyatni, ikkinchisi esa faqat ikkinchisini qo'shadigan ikkita alohida commit yaratmoqchi ekanligingizni tasavvur qiling. Yoki butun kodingiz bo'ylab xatoliklarni topish (debugging) uchun print ko'rsatmalari va qo'shimcha ravishda xatolik tuzatmasi (bugfix) bor deylik; barcha print'larni chetga surib faqat bugfix'ni commit qilmoqchi bo'lishingiz mumkin.

Git bunday ssenariylarga "staging area" deb ataladigan mexanizm orqali navbatdagi snapshot'ga aynan qaysi o'zgarishlar kiritilishini aniq ko'rsatishga ruxsat berish orqali moslashadi.

# Git buyruqlar satri interfeysi

Ma'lumotlarni takrorlamaslik uchun, quyidagi buyruqlarni ushbu ma'ruza matnlarida batafsil tushuntirmaymiz. Ko'proq ma'lumot olish uchun tavsiya etilgan [Pro Git](https://git-scm.com/book/en/v2) kitobini o'qing yoki ma'ruza videosini ko'ring.

## Asoslar

- `git help <command>`: git buyrug'i uchun yordam oling
- `git init`: `.git` katalogida saqlanadigan ma'lumotlarga ega yangi git repozitoriysini yaratadi
- `git status`: nima bo'layotganini ko'rsatadi
- `git add <filename>`: fayllarni staging area'ga qo'shadi
- `git commit`: yangi commit yaratadi
    - [Yaxshi commit xabarlarini](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) yozing!
    - [Yaxshi commit xabarlarini](https://chris.beams.io/posts/git-commit/) yozish uchun yana ko'proq sabablar!
- `git log`: tarixning tekislangan jurnalini ko'rsatadi
- `git log --all --graph --decorate`: tarixni DAG sifatida tasvirlaydi
- `git diff <filename>`: siz kiritgan o'zgarishlarni staging area'ga nisbatan ko'rsatadi
- `git diff <revision> <filename>`: snapshot'lar orasidagi fayldagi farqlarni ko'rsatadi
- `git checkout <revision>`: HEAD'ni (va agar tarmoqqa o'tilayotgan bo'lsa, joriy tarmoqni) yangilaydi

## Tarmoqlash va birlashtirish

- `git branch`: tarmoqlarni ko'rsatadi
- `git branch <name>`: tarmoq yaratadi
- `git switch <name>`: tarmoqqa o'tadi
- `git checkout -b <name>`: tarmoq yaratadi va unga o'tadi
    - `git branch <name>; git switch <name>` bilan bir xil
- `git merge <revision>`: joriy tarmoqqa birlashtiradi
- `git mergetool`: ziddiyatlarni hal qilishga yordam beruvchi maxsus vositadan foydalaning
- `git rebase`: yamoqlar (patches) to'plamini yangi bazaga o'tkazish (rebase)

## Masofaviy repozitoriylar

- `git remote`: masofaviy repozitoriylarni ro'yxatlaydi
- `git remote add <name> <url>`: masofaviy repozitoriy qo'shish
- `git push <remote> <local branch>:<remote branch>`: obyektlarni masofaviy repozitoriyga yuborish va masofaviy havolani yangilash
- `git branch --set-upstream-to=<remote>/<remote branch>`: mahalliy va masofaviy tarmoqlar o'rtasidagi bog'liqlikni o'rnatish
- `git fetch`: masofaviy repozitoriydan obyektlar/havolalarni yuklab olish
- `git pull`: `git fetch; git merge` bilan bir xil
- `git clone`: masofaviy repozitoriyni yuklab olish

## Bekor qilish (Undo)

- `git commit --amend`: commit'ning tarkibini/xabarini tahrirlash
- `git reset <file>`: faylni staging area'dan chiqarib tashlash
- `git restore`: o'zgarishlarni bekor qilish

# Murakkab Git

- `git config`: Git [yuqori darajada moslashtiriladigan (customizable)](https://git-scm.com/docs/git-config) tizimdir
- `git clone --depth=1`: barcha versiyalar tarixini olmaydigan yuzaki (shallow) klonlash
- `git add -p`: interaktiv tarzda staging area'ga qo'shish
- `git rebase -i`: interaktiv rebase
- `git blame`: qaysi qatorni oxirgi marta kim tahrirlaganini ko'rsatadi
- `git stash`: ishchi katalogga kiritilgan o'zgartirishlarni vaqtinchalik olib tashlash
- `git bisect`: tarix bo'yicha binar qidiruv (masalan, regressiyalarni topish uchun)
- `git revert`: avvalgi commit ta'sirini teskarisiga o'zgartiradigan yangi commit yaratish
- `git worktree`: bir vaqtning o'zida bir nechta tarmoqlarni ochib ishlash
- `.gitignore`: ataylab kuzatilmaydigan fayllarni e'tiborsiz qoldirishni [belgilash](https://git-scm.com/docs/gitignore)

# Turli xil narsalar

- **GUI'lar**: Git uchun juda ko'p [GUI (grafik interfeys) mijozlari](https://git-scm.com/downloads/guis) mavjud. Shaxsan biz ulardan foydalanmaymiz va uning o'rniga buyruqlar satri interfeysidan foydalanamiz.
- **Shell integratsiyasi**: shell so'rovida Git holati ko'rinib turishi juda qulay ([zsh](https://github.com/olivierverdier/zsh-git-prompt), [bash](https://github.com/magicmonty/bash-git-prompt)). Ko'pincha [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh) kabi freymvorklar tarkibiga kiradi.
- **Tahrirlovchi (Editor) integratsiyasi**: xuddi yuqoridagiga o'xshab, juda ko'p imkoniyatlarga ega qulay integratsiyalar mavjud. [fugitive.vim](https://github.com/tpope/vim-fugitive) Vim uchun standart hisoblanadi.
- **Ish oqimlari (Workflows)**: biz sizga ma'lumotlar modelini, hamda ba'zi asosiy buyruqlarni o'rgatdik; yirik loyihalarda ishlaganda qanday amaliyotlarga rioya qilish kerakligini aytmadik (buning [ko'plab](https://nvie.com/posts/a-successful-git-branching-model/) [turli xil](https://www.endoflineblog.com/gitflow-considered-harmful) [yondashuvlari mavjud](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)).
- **GitHub**: Git bu GitHub emas. GitHub boshqa loyihalarga kod qo'shishning o'ziga xos usuliga ega bo'lib, u [pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests) deb ataladi.
- **Boshqa Git provayderlari**: GitHub yagona emas: [GitLab](https://about.gitlab.com/) va [BitBucket](https://bitbucket.org/) kabi ko'plab boshqa Git repozitoriylari xostlari ham mavjud.

# Manbalar

- [Pro Git](https://git-scm.com/book/en/v2) ni o'qish **jiddiy tavsiya etiladi**. Ma'lumotlar modelini tushunib olganingizdan so'ng, 1-5 boblarni o'qib chiqish sizga Git'dan qanday professional tarzda foydalanish kerakligi bo'yicha ko'p narsani o'rgatadi. Keyingi boblar qiziqarli, murakkab materiallarni o'z ichiga oladi.
- [Oh Shit, Git!?!](https://ohshitgit.com/) bu ba'zi umumiy Git xatolarini qanday to'g'irlash bo'yicha qisqacha qo'llanma.
- [Kompyuter mutaxassislari uchun Git](https://eagain.net/articles/git-for-computer-scientists/) Git'ning ma'lumotlar modeli bo'yicha qisqacha tushuntirish bo'lib, ushbu ma'ruza matnlaridan ko'ra kamroq psevdokod va chiroyliroq diagrammalardan iborat.
- [Pastdan yuqoriga Git](https://jwiegley.github.io/git-from-the-bottom-up/) (Git from the Bottom Up) Git qanday amalga oshirilgani haqida qiziquvchilar uchun faqat ma'lumotlar modelidan tashqari batafsil tushuntirishdir.
- [Git'ni oddiy so'zlar bilan qanday tushuntirish mumkin](https://smusamashah.github.io/blog/2017/10/14/explain-git-in-simple-words)
- [Learn Git Branching](https://learngitbranching.js.org/) brauzer orqali Git'ni o'rgatuvchi o'yin.

# Mashqlar

1. Agar siz Git bilan oldin ishlash tajribasiga ega bo'lmasangiz, [Pro Git](https://git-scm.com/book/en/v2) ning dastlabki bir necha bobini o'qishga yoki [Learn Git Branching](https://learngitbranching.js.org/) kabi qo'llanmadan o'tishga harakat qiling. O'qish jarayonida Git buyruqlarini ma'lumotlar modeliga bog'lab o'rganing.
1. [Kurs veb-sayti repozitoriysini](https://github.com/missing-semester/missing-semester) klonlang.
    1. Versiyalar tarixini graf sifatida tasvirlab o'rganing.
    1. `README.md` faylini oxirgi marta kim o'zgartirgan? (Maslahat: argument bilan `git log` dan foydalaning).
    1. `_config.yml` faylidagi `collections:` qatoriga kiritilgan oxirgi o'zgartirish bilan bog'liq commit xabari nima edi? (Maslahat: `git blame` va `git show` dan foydalaning).
1. Git'ni o'rganayotgandagi eng keng tarqalgan xatolardan biri bu Git tomonidan boshqarilmasligi kerak bo'lgan katta hajmdagi fayllarni yoki maxfiy ma'lumotlarni commit qilishdir. Repozitoriyga bitta fayl qo'shishga urinib ko'ring, bir nechta commit'lar yarating va so'ngra ushbu faylni _tarixdan_ butunlay o'chirib tashlang (nafaqat so'nggi commit'dan). Buni qanday qilishni bilish uchun [buni](https://help.github.com/articles/removing-sensitive-data-from-a-repository/) o'qib chiqsangiz bo'ladi.
1. GitHub'dan biron bir repozitoriyni klonlang va undagi mavjud fayllardan birini o'zgartiring. Agar siz `git stash` ni bajarsangiz nima sodir bo'ladi? `git log --all --oneline` ni bajarganda nimani ko'ryapsiz? `git stash` orqali qilgan amalingizni orqaga qaytarish uchun `git stash pop` buyrug'ini bajaring. Bu qaysi ssenariyda foydali bo'lishi mumkin?
1. Ko'pgina buyruqlar satri vositalari singari, Git ham `~/.gitconfig` nomli konfiguratsiya fayli (yoki nuqtali fayl) ni taqdim etadi. `~/.gitconfig` ichida qisqartma (alias) yarating, shu tufayli qachonki `git graph` buyrug'ini bersangiz, sizga `git log --all --graph --decorate --oneline` natijasi chiqadigan bo'lsin. Buni `~/.gitconfig` faylini bevosita [tahrirlash](https://git-scm.com/docs/git-config#Documentation/git-config.txt-alias) orqali yoki alias qo'shish uchun `git config` buyrug'ini ishlatish bilan amalga oshirishingiz mumkin. Git alias'lari bo'yicha ma'lumotlarni [bu yerdan](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases) topsangiz bo'ladi.
1. `git config --global core.excludesfile ~/.gitignore_global` buyrug'ini berib, `~/.gitignore_global` faylida global e'tiborsiz qoldirish naqshlari (ignore patterns) ni aniqlashingiz mumkin. Bu Git foydalanadigan global ignore fayli manzilini o'rnatadi, biroq siz baribir o'sha manzilda faylni qo'lda yaratishingiz kerak. O'zingizning operatsion tizimingiz yoki tahrirlovchingizga xos vaqtinchalik fayllarni, masalan, `.DS_Store` ni e'tiborsiz qoldirish (ignore qilish) uchun global gitignore faylingizni sozlang.
1. [Kurs veb-sayti repozitoriysini](https://github.com/missing-semester/missing-semester) fork qiling, imlo xatosini (typo) yoki o'zingiz kiritishingiz mumkin bo'lgan boshqa yaxshilanishni topib, GitHub'da pull request jo'nating (Buning uchun [buni](https://github.com/firstcontributions/first-contributions) ko'rishingiz mumkin). Iltimos, faqatgina foydali bo'lgan PR'larni yuboring (iltimos, bizga spam yubormang!). Agar hech qanday yaxshilanish topa olmasangiz, bu mashqni o'tkazib yuborishingiz mumkin.
1. Hamkorlikdagi stsenariyni simulyatsiya qilib, ziddiyatlarni (merge conflicts) hal qilishni amalda sinab ko'ring:
    1. `git init` orqali yangi repozitoriy yarating va ichida bir nechta qator (masalan, retsept) dan iborat `recipe.txt` degan fayl yarating.
    1. Uni commit qiling, so'ngra ikkita tarmoq (branch) yarating: `git branch salty` va `git branch sweet`.
    1. `salty` tarmog'ida bir qatorni o'zgartiring (masalan, "1 piyola shakar" ni "1 piyola tuz" ga o'zgartiring) va commit qiling.
    1. `sweet` tarmog'ida ham xuddi shu qatorni boshqacha qilib o'zgartiring (masalan, "1 piyola shakar" ni "2 piyola shakar" ga o'zgartiring) va commit qiling.
    1. Endi `master` tarmog'iga qayting va avval `git merge salty`, so'ng `git merge sweet` ni bajarib ko'ring. Nima sodir bo'ladi? `recipe.txt` faylining tarkibini ko'ring - undagi `<<<<<<<`, `=======` va `>>>>>>>` belgilari nimani anglatadi?
    1. O'zingiz qoldirmoqchi bo'lgan tarkibni saqlab faylni tahrirlash orqali ziddiyatni hal qiling, ziddiyat belgilarini olib tashlang va birlashtirishni `git add` va `git commit` (yoki `git merge --continue`) orqali yakunlang. Muqobil variant sifatida, grafik yoki terminal asosidagi merge vositalaridan foydalanib ziddiyatni hal qilish uchun `git mergetool` ni ishlating.
    1. O'zingiz hozirgina yaratgan birlashish tarixini (merge history) ko'rish uchun `git log --graph --oneline` dan foydalaning.