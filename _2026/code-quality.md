---
layout: lecture
title: "Kod Sifati"
description: >
  Formatlash, linter'lar, testlash, uzluksiz integratsiya va boshqalar haqida bilib oling.
thumbnail: /static/assets/thumbnails/2026/lec9.png
date: 2026-01-23
ready: true
video:
  aspect: 56.25
  id: XBiLUNx84CQ
---

Dasturchilarga yuqori sifatli kod yozishda yordam beradigan turli xil vosita va usullar mavjud. Ushbu ma'ruzada biz quyidagilarni ko'rib chiqamiz:

- [Formatlash](#formatlash)
- [Lintlash](#lintlash)
- [Testlash](#testlash)
- [Pre-commit hook'lar](#pre-commit-hook)
- [Uzluksiz integratsiya](#uzluksiz-integratsiya)
- [Buyruq ishga tushiruvchilar (Command runners)](#buyruq-ishga-tushiruvchilar)

Qo'shimcha mavzu sifatida biz [regulyar ifodalarni](#regulyar-ifodalar) (regular expressions) ham ko'rib chiqamiz. Bu turli sohalarni qamrab oluvchi mavzu bo'lib, u kod sifati sohasida (masalan, shablonga mos keladigan testlarning ma'lum qismini ishga tushirish uchun) hamda IDE'lar kabi boshqa joylarda (masalan, qidirish va o'rniga qo'yish uchun) qo'llaniladi.

Ushbu vositalarning ko'pchiligi aniq bir dasturlash tiliga bog'liq bo'ladi (masalan, Python uchun [Ruff](https://docs.astral.sh/ruff/) linter'i/formater'i). Ba'zi hollarda vositalar bir nechta tillarni qo'llab-quvvatlashi mumkin (masalan, [Prettier](https://prettier.io/) kod formater'i). Biroq, g'oyalar deyarli universaldir --- siz har qanday dasturlash tili uchun kod formater'lari, linter'lar, testlash kutubxonalari va hokazolarni topishingiz mumkin.

# Formatlash

Kodni avtomatik formatlovchi vositalar dasturning tashqi sintaksisini avtomatik ravishda tartibga soladi. Shu tariqa, siz e'tiboringizni chuqurroq va murakkabroq muammolarga qaratishingiz mumkin. Avto-formatlash vositasi esa qatorlar uchun `'` o'rniga `"` ishlatilishi, binar operatorlar atrofida bo'sh joy qoldirilishi (`x+y` o'rniga `x + y`), `import` ko'rsatmalarining alifbo tartibida bo'lishi va qatorlarning juda uzun bo'lib ketishini oldini olish kabi oddiy ishlarni o'z zimmasiga oladi. Kod formater'larining asosiy afzalliklaridan biri shundaki, ular bitta repozitoriy ustida ishlayotgan barcha dasturchilar uchun kod yozish uslubini birxillashtiradi.

Prettier kabi ba'zi vositalar [juda moslashuvchan va ko'p sozlanishi mumkin](https://prettier.io/docs/configuration); siz o'z loyihangiz uchun sozlamalar faylini [versiyalarni boshqarish](/2026/version-control/) tizimiga kiritishingiz kerak. [Black](https://github.com/psf/black) va [gofmt](https://pkg.go.dev/cmd/gofmt) kabi boshqa vositalarda esa sozlash imkoniyati cheklangan yoki umuman yo'q, bu [bikeshedding'ni](https://en.wikipedia.org/wiki/Law_of_triviality) (kichik detallar ustida ortiqcha tortishishni) kamaytirish uchun qilingan.

Siz kod formater'i bilan [IDE integratsiyasini](/2026/development-environment/#kod-intellekti-va-til-serverlari) sozlashingiz mumkin, shunda kodingizni yozayotganingizda yoki faylni saqlaganingizda u avtomatik ravishda formatlanadi. Shuningdek, loyihangizga [EditorConfig](https://editorconfig.org/) faylini qo'shishingiz ham mumkin, bu IDE'ga har bir fayl turi uchun xatboshi (indent) o'lchami kabi umumiy loyiha darajasidagi sozlamalarni yetkazadi.

# Lintlash

Linter'lar kodingizdagi anti-pattern'larni va yuzaga kelishi mumkin bo'lgan muammolarni topish uchun statik tahlil (kodingizni ishga tushirmasdan tahlil qilish) o'tkazadi. Ushbu vositalar avtoformater'larga qaraganda chuqurroq ishlaydi va shunchaki tashqi sintaksisdan nariga o'tadi. Tahlilning chuqurlik darajasi asbobga qarab farq qiladi.

Linter'lar qoidalar ro'yxati bilan birga keladi va bu qoidalar to'plamlari har bir loyiha uchun alohida sozlanishi mumkin. Ba'zi linter qoidalari noto'g'ri xatoliklarni (false positives) keltirib chiqarishi mumkin, shuning uchun ularni fayl yoki qator darajasida o'chirib qo'yishingiz mumkin.

Yaxshi linter'lar o'rnatilgan yordam yoki har bir linter qoidasini tushuntirib beruvchi hujjatlarga ega bo'ladi --- qoida nimani qidirayotgani, nima uchun u yomon ekanligi va shu kod shabloni uchun yaxshiroq muqobil nima bo'lishi haqida ma'lumot beradi. Masalan, Python kodida keraksiz ketma-ket (nested) `if` operatorlarini aniqlaydigan [Ruff'dagi](https://docs.astral.sh/ruff/) [SIM102](https://docs.astral.sh/ruff/rules/collapsible-if/) qoidasi hujjatini ko'ring.

Ba'zi linter'lar nafaqat muammolarni aniqlay oladi, balki ba'zi xatolarni avtomatik ravishda tuzatishi ham mumkin.

Tilga xos linter'lardan tashqari yana bir qo'l kelishi mumkin bo'lgan vosita bu [semgrep](https://github.com/semgrep/semgrep) deb nomlanuvchi "semantik grep" vositasidir. U (grep kabi belgilar darajasida emas) AST darajasida ishlaydi va ko'plab tillarni qo'llab-quvvatlaydi. Semgrep yordamida loyihalaringiz uchun shaxsiy linter qoidalarini osongina yozishingiz mumkin. Masalan, Python'da xavfli bo'lgan `subprocess.Popen(..., shell=True)` funksiyasining oldini olishni istasangiz, u holda quyidagi buyruq bilan shu kod shablonini topishingiz mumkin:

```bash
semgrep -l python -e "subprocess.Popen(..., shell=True, ...)"
```

# Testlash

Dasturiy ta'minotni testlash – bu kodingizning to'g'riligiga ishonchingizni oshirish uchun ishlatiladigan standart texnikadir. Siz kod yozasiz, so'ngra siz yozgan kodni tekshiradigan va kod kutilgandek ishlamasa, xato chiqaradigan kod (test) yozasiz.

Kodni turli darajalardagi bo'laklarga ajratib testlashingiz mumkin: alohida funksiyalar uchun _unit testlar_, modullar yoki xizmatlar o'rtasidagi o'zaro ta'sirni tekshirish uchun _integratsion testlar_, va jarayonni to'liq tekshirish uchun _funksional testlar_. Shuningdek, _test-driven development_ (testga asoslangan dasturlash) usulidan foydalanishingiz mumkin, ya'ni haqiqiy kodni yozishdan oldin uning testini yozasiz. Kodingizda xatolik topsangiz, kelajakda shu funksiya yana buzilsa ushlab qolishi uchun _regression testlar_ yozishingiz mumkin. Xaskell'dagi [QuickCheck'da](https://hackage.haskell.org/package/QuickCheck) o'ylab topilgan va Python uchun [Hypothesis](https://hypothesis.readthedocs.io/) kabi ko'plab kutubxonalarda qo'llaniladigan xususiyatlarga asoslangan testlarni (_property-based tests_) yozishingiz mumkin. Qaysi testlash yondashuvi to'g'ri ekanligi loyihangizga bog'liq; ehtimol, siz ularning kombinatsiyasidan foydalanasiz.

Agar dasturingiz ma'lumotlar bazasi yoki veb API kabi tashqi tizimlarga qaram (dependency) bo'lsa, testlash paytida uchinchi tomon vositalari bilan o'zaro aloqa qilish o'rniga testlarda bu qaramliklarni _mock_ qilish (ya'ni taqlidini yaratish) foydali bo'lishi mumkin.

## Kod qamrovi

Kod qamrovi (code coverage) – bu kodingizdagi testlarning qanchalik yaxshi yozilganligini o'lchaydigan metrika. Kod qamrovi testlar ishlaganda kodingizning qaysi qatorlari bajarilganligini ko'rsatib beradi, shunda siz barcha ehtimoliy holatlarni hisobga olganingizga ishonch hosil qilishingiz mumkin. Kod qamrovi vositalari sizga test yozishda yordam berish uchun qatorma-qator qamrovni ko'rsata oladi. [Codecov](https://app.codecov.io) kabi xizmatlar loyiha tarixi davomida kod qamrovini kuzatish va ko'rish uchun veb-interfeyslarni taqdim etadi.

Boshqa har qanday metrika singari, kod qamrovi ham mukammal emas; unga ortiqcha bog'lanib qolmang, ko'proq yuqori sifatli testlar yozishga e'tibor qarating.

# Pre-commit hook

[pre-commit](https://pre-commit.com/) freymvorki orqali osonlashtirilgan Git pre-commit [hook'lar](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) foydalanuvchi tomonidan belgilangan kodni har bir Git commit'dan oldin avtomatik ravishda ishga tushiradi. Loyihalar ko'pincha pre-commit hook'lardan formater'lar va linter'larni, ba'zan esa testlarni har bir commit'dan oldin avtomatik ishga tushirish uchun foydalanadi. Bu commit qilinayotgan kodning loyiha uslubiga mos kelishini va ayrim xatolardan xoli bo'lishini ta'minlaydi.

# Uzluksiz integratsiya

[GitHub Actions](https://github.com/features/actions) kabi uzluksiz integratsiya (continuous integration - CI) xizmatlari siz kodni push qilganingizda (yoki har bir pull request'da, yoki jadval bo'yicha) siz uchun skriptlarni avtomatik ravishda ishga tushirishi mumkin. Dasturchilar odatda formater, linter va testlarni o'z ichiga olgan kod sifatini tekshiruvchi vositalarni yurgizish uchun CI xizmatlaridan foydalanadilar. Kompilyatsiya qilinadigan tillar uchun siz kodning to'g'ri kompilyatsiya bo'lishini ta'minlashingiz mumkin; statik turlarga ega tillar uchun uni tur (type checking) bo'yicha tekshirishingiz mumkin. Har safar yangi commit push qilinganda CI'ni ishga tushirish kodning asosiy versiyasiga tushgan xatolarni tezda aniqlashi mumkin; pull request'lar ustida ishlaganda kontribyutorlarning yuborgan o'zgarishlaridagi muammolarni topish; ularni ma'lum jadval asosida yurgizish tashqi qaramliklar bilan bog'liq muammolarni aniqlashi mumkin (masalan, kutilmaganda buzuvchi o'zgarish (breaking change) e'lon qilinib ketilganda).

CI skriptlari dasturchilarning shaxsiy kompyuterlaridan alohida ishlagani uchun, siz bemalol u yerda uzoq davom etadigan vazifalarni bajarishingiz mumkin. Bundan turli xil operatsion tizimlar va dasturlash tilining turli versiyalarida testlar _matritsasini_ (matrix) yurgizib, dastur ularning barchasida to'g'ri ishlashini tekshirish uchun foydalanish mumkin.

Odatda, CI'da ishlaydigan skript kodingizga bevosita o'zgartirish kiritmaydi: u vositalarni "tuzatish" o'rniga faqat "tekshirish" rejimida ishga tushiradi. Shuning uchun, masalan, agar kod formati talabga mos kelmasa, avto-formater xatolik haqida signal beradi.

Repozitoriylarning README qismida ko'pincha CI holatini va kod qamrovi kabi boshqa ma'lumotlarni ko'rsatadigan [holat nishonlari (status badges)](https://docs.github.com/en/actions/how-tos/monitor-workflows/add-a-status-badge) bo'ladi. Masalan, quyida "Missing Semester" ning joriy build holati ko'rsatilgan.

[![Build Status](https://github.com/missing-semester/missing-semester/actions/workflows/build.yml/badge.svg)](https://github.com/missing-semester/missing-semester/actions/workflows/build.yml) [![Links Status](https://github.com/missing-semester/missing-semester/actions/workflows/links.yml/badge.svg)](https://github.com/missing-semester/missing-semester/actions/workflows/links.yml)

> Bizning [havolalar tekshirgichimiz](https://github.com/missing-semester/missing-semester/blob/master/.github/workflows/links.yml) (u [proof-html](https://github.com/anishathalye/proof-html) GitHub Action'dan foydalanadi) ko'pincha tashqi veb-saytlardagi muammolar sababli xato beradi. Shunga qaramay, u bizga ko'plab buzilgan havolalarni (ba'zan imlo xatolar tufayli, lekin asosan veb-saytlar tarkibni ko'chirganda redirect qo'shmagani yoxud butunlay g'oyib bo'lgani sababli) ushlash va tuzatishda yordam berdi.

CI xizmatlari, formater'lar, linter'lar va testlash kutubxonalarini o'rganishning eng yaxshi usuli misollarni ko'rishdir. GitHub'dan yuqori sifatli ochiq manbali loyihalarni (open-source) toping — u dasturlash tili, sohasi, o'lchami va ko'lami bo'yicha siznikiga qanchalik yaqin bo'lsa shuncha yaxshi — va ularning `pyproject.toml`, `.github/workflows/`, `DEVELOPMENT.md` kabi muhim fayllarini o'rganib chiqing.

## Uzluksiz yoyish (Continuous deployment)

Uzluksiz yoyish (Continuous deployment) tizimi kod o'zgarishlarini asboblar yordamida haqiqatda _joylashtirish_ (deploy) uchun CI infratuzilmasidan foydalanadi. Masalan, Missing Semester repozitoriyasi GitHub pages'ga uzluksiz yoyishdan foydalanadi, natijada qachonki yangilangan ma'ruzalar matnini `git push` qilsak, sayt avtomatik ravishda qurilib foydalanuvchilar ommasiga joylashtiriladi. Siz CI'da ilovalar uchun binar fayllar yoki xizmatlar uchun Docker tasvirlari kabi boshqa turdagi [artefaktlarni](/2026/shipping-code/) ham yig'ishingiz (build qilishingiz) mumkin.

# Buyruq ishga tushiruvchilar

[just](https://github.com/casey/just) kabi buyruq ishga tushiruvchilar loyiha doirasida buyruqlarni chaqirish vazifasini soddalashtiradi. Loyihangiz uchun kod sifati bo'yicha turli instrumentlarni sozlaganingizdan so'ng, ehtimol, siz jamoangiz dasturchilaridan `uv run ruff check --fix` kabi uzun buyruqlarni yodlab olishini xohlamassiz. Buyruq yurgizuvchilar yordamida bu oddiygina `just lint` ko'rinishiga keladi va dasturchi o'z ishi uchun ehtiyoj sezishi mumkin bo'lgan barcha vositalar uchun `just format`, `just typecheck` kabi soddalashtirilgan komandalarni shakllantirishingiz mumkin.

Ba'zi tillar uchun maxsus loyiha yoki paket menejerlari bunday funksiyalarni avvaldan o'zida mujassam qilgan bo'lishi mumkin, bu esa tilga bog'lanmagan `just` kabi universal asboblarga muhtojlik yo'qligini bildiradi. Masalan, [npm](https://nodejs.org/en/learn/getting-started/an-introduction-to-the-npm-package-manager) (Node.js) uchun `package.json` faylining `scripts` qismi va [Hatch](https://hatch.pypa.io/) (Python) uchun `pyproject.toml` faylining `tool.hatch.envs.*.scripts` qismlari ushbu funksiyani bajaradi.

# Regulyar ifodalar

_Regulyar ifodalar_, qisqacha "regex" yoki "regexp", qatorlarning ma'lum to'plamlarini ifodalash uchun ishlatiladigan til hisoblanadi. Regex shablonlari turli xil asboblar, masalan, buyruqlar qatori dasturlari va IDE'larda qidiruv va moslikni (pattern matching) tekshirish uchun ko'p qo'llaniladi. Masalan, [ag](https://github.com/ggreer/the_silver_searcher) butun kodlar bazasi bo'ylab qidirish uchun regex shablonlarini qo'llab-quvvatlaydi (masalan, `ag "import .* as .*"` Pythondagi barcha qayta nomlangan import'larni topadi), [go test](https://pkg.go.dev/cmd/go#hdr-Test_packages) asbobi esa testlarning ma'lum bir guruhini tanlab yurgizish uchun `-run [regexp]` parametrini taqdim etadi. Shu bilan birga dasturlash tillari regex uchun o'rnatilgan yoki qo'shimcha kutubxonalarga ega, shuning uchun regex'dan pattern matching, tekshirish (validation) va parsing ishlarida qulayroq foydalanishingiz mumkin.

Bu narsa bo'yicha tushunchalarni rivojlantirish uchun quyida ba'zi regex misollari berilgan. Ushbu ma'ruzada biz [Python regex sintaksisidan](https://docs.python.org/3/library/re.html) foydalanamiz. Regex'ning ko'plab turlari bo'lib, ular orasida, ayniqsa chuqurlashtirilgan funksionalligida biroz farqlar mavjud. O'zingizni regulyar ifodalaringizni yozish va tekshirish uchun [regex101](https://regex101.com/) kabi onlayn regex tekshiruvchilaridan foydalanishingiz mumkin.

- `abc` --- "abc" so'zining aynan o'ziga mos keladi.
- `missing|semester` --- "missing" yoki "semester" so'zlariga mos keladi.
- `\d{4}-\d{2}-\d{2}` --- "2026-01-14" kabi YYYY-MM-DD formatidagi sanalarga mos keladi. Matnda to'rtta raqam, chiziqcha, ikkita raqam, chiziqcha va yana ikkita raqamdan iborat ekanligini ta'minlashdan tashqari, u sananing haqiqiyligini tekshirmaydi, shuning uchun "2026-01-99" ham shu regex naqshiga to'g'ri tushaveradi.
- `.+@.+` --- elektron pochta manzillariga (qandaydir matn, keyin "@", va yana qandaydir matn bo'lgan qatorlar) mos keladi. Bu juda oddiy darajada bo'lib, "bema'nilik@@@email" kabi bo'lmag'ur manzillarni ham o'tkazib yuboradi. Har qanday xatolik (false positive yoki negative)siz email'ni ushlaydigan regex [mavjud](https://pdw.ex-parrot.com/Mail-RFC822-Address.html), lekin u amaliyotda noqulay.

## Regex sintaksisi

Regex sintaksisi bo'yicha keng qamrovli qo'llanmani [ushbu hujjatdan](https://docs.python.org/3/library/re.html#regular-expression-syntax) (yoki internetdagi boshqa ochiq manbalardan) topishingiz mumkin. Mana ba'zi asosiy elementlar:

- `abc` agar belgilarga maxsus ma'no yuklatilmagan bo'lsa ularga aynan mos tushadi (bu holda "abc")
- `.` har qanday yagona belgiga mos keladi
- `[abc]` qavs ichida keltirilgan belgilardan istalgan birortasiga mos keladi (masalan, "a", "b", yoki "c")
- `[^abc]` qavs ichidagi belgilardan TASHQARI istalgan bitta belgiga mos keladi (masalan, "d")
- `[a-f]` qavs ichida belgilangan oraliqdagi bitta belgiga mos keladi (masalan, "c", lekin "q" emas)
- `a|b` qaysi birining mavjudligidan qat'iy nazar ("a" yoki "b") shablonga mos tushadi
- `\d` har qanday raqam (masalan, "3") bilan mos tushadi
- `\w` har qanday so'z harfi (masalan, "x") ga mos keladi
- `\b` so'z _chegaralariga_ mos tushadi (masalan, "missing semester" so'zida, aynan "m" dan oldin, "g" dan keyin, "s" dan oldin va "r" dan keyin turgan o'rin)
- `(...)` shablonning guruh qilingan ko'rinishi
- `...?` guruhning yoki shablonning nol yoki bitta variantiga mos keladi. Misol uchun `words?` shabloni "word" yoxud "words" qatorlaridan istalganini anglatishi mumkin.
- `...*` nol yoxud ixtiyoriy miqdordagi narsalarni anglatadi, masalan `.*` deb yozsangiz istalgan belgi ixtiyoriy miqdorda kelaveradi
- `...+` shablondan bir yoki undan ko'proq takrorlanishiga mos keladi, masalan `\d+` deb yozsangiz ixtiyoriy noldan farqli uzunlikdagi son ushlab olinadi
- `...{N}` shablonda aynan N marotaba takrorlanish. Misol uchun `\d{4}` bu - to'rt xonali son degani
- `\.` haqiqiy nuqta belgisiga (".") mos keladi
- `\\` haqiqiy backslash belgisiga ("\\") mos keladi
- `^` qatorning boshlanishiga ishora qiladi
- `$` qatorning oxiriga ishora qiladi

## Capture group'lar va referenslar

Agar siz regex group'lardan `(...)` foydalansangiz, ushlab olingan moslikning sub-qismlariga qayta referensiya berib malumot ajratib olishingiz yoki o'rniga boshqa narsa almashtirishingiz (search-and-replace) mumkin. Masalan, YYYY-MM-DD ko'rinishidagi sanadan faqat oy raqamini sug'urib olish uchun siz Python'da shunday yoza olasiz:

```python
>>> import re
>>> re.match(r"\d{4}-(\d{2})-\d{2}", "2026-01-14").group(1)
'01'
```

Kodni tahrirlovchi IDE larda moslikka almashtirayotganingizda capture group'larni raqam orqali belgilashingiz mumkin. Sintaksis IDE larda turlicha bo'ladi. Masalan, VS Code'da guruhlarga ishora qilish uchun `$1`, `$2` va hokazo, Vim'da esa `\1`, `\2` lardan foydalanishingiz mumkin.

## Cheklovlar

[Regulyar tillar](https://en.wikipedia.org/wiki/Regular_language) qudratli bo'lsa-da cheklangandir; ularni standart regex ko'rinishida yozib bo'lmaydigan simvollar ketma-ketliklari bor. (Masalan, siz hech qachon shunday regulyar ifoda yoza olmaysizki [u qatorga](https://en.wikipedia.org/wiki/Pumping_lemma_for_regular_languages) {a^n b^n \| n &ge; 0} mos kelsin — ya'ni, nechta "a" bo'lsa keyin shuncha "b" simvoli keladigan hollar guruhini; shuningdek HTML kabi tillar regex guruhiga ham kirmaydi). Amalda zamonaviy regex dvigatellari (engines) cheklovlardan chetga chiqqan qarab olish (lookahead) yoki backreference kabi kuchli qo'shimcha mexanizmlarni o'z ichiga olgan. Bular ishlatishga nihoyatda samarali hamdir. Biroq chuqurroq tizimli yozishmalar uchun ba'zida rivojlangan parser xizmatlariga murojaat qilish o'rinli (ularga misol uchun qarang: [pyparsing](https://github.com/pyparsing/pyparsing) yoki [PEG](https://en.wikipedia.org/wiki/Parsing_expression_grammar) parser).

## Regex o'rganish

Boshida butun texnologiyani to'liq yodlab olish o'rniga, asoslarini (ma'ruzada aytib o'tilgan qismlarni) chuqur tushunishni va qachon zarurat tug'ilsa spravkalardan shablonlarga qarab olib ishlashni tavsiya qilamiz.

Sun'iy intellekt xizmatlari regex shablonlari yaratishda qo'l kelishi ham mumkin. O'zingiz qiziqqan LLM ni quyidagicha so'rov (prompt) yordamida tekshirib ko'rishingiz mumkin:

```
Nginx jurnallaridan (log) yo'l(path) manzilini ushlab olish uchun Python stilida regex pattern yozib ber. Masalan quyidagicha log qatori bor deb tasavvur qil:

169.254.1.1 - - [09/Jan/2026:21:28:51 +0000] "GET /feed.xml HTTP/2.0" 200 2995 "-" "python-requests/2.32.3"
```

# Mashqlar

1. O'zingiz hozirda ustida ishlayotgan loyiha uchun formater, linter va pre-commit hook'larni sozlang. Agar kodingizda yuzlab xatoliklar qolib ketgan bo'lsa: formater ko'rinish xatolarini avtomatik to'g'rilab berishi mumkin. Linter xatolari uchun esa hammasini bittada tuzatish niyatida sun'iy idrokka asoslangan [dasturlash agentidan](/2026/agentic-coding/) foydalanib ko'ring. Agenti linter'ni ishlab tekshira oladigan, takrorlanuvchi jarayonlar xatolarni tuzatgunga qadar ishlashi kerak. Keyin ehtiyotkorlik bilan sun'iy idrok sizga yangi muammolar tug'dirmaganiga ishonch hosil qiling!
1. O'zingiz bilgan biror til uchun testlash kutubxonasini o'rganing va loyihangizdagi ba'zi kodlarni tekshirish uchun unit test yozing. Kod qamrovini ishga tushiring, qamrov haqida HTML ko'rinishidagi hisobot yarating va natijani baholang. Kodingizning qaysi joylari qamrab olinganini seza oldingizmi? Katta ehtimol bilan hozir hisobot past bo'ladi. Unit testlarni o'zingiz yozish orqali hisobot qamrovini yaxshilab borishga urining. Ushbu muammoni yaxshilash maqsadida testning kod qamrovi haqida line-by-line hisobot bera oluvchi va kodlarni testlay oluvchi sun'iy intellekt (dasturlash agenti) [bilan ishlab ko'ring](/2026/agentic-coding/). Sizningcha AI yozgan kod tekshiruv testlari chin dildan ishlanganiga amin bo'lasizmi?
1. O'zingiz ishlayotgan loyiha uchun uzluksiz integratsiyani sozlang, shunda har safar push qilinganda ishlashini ta'minlaysiz. CI uchun avtoformat, linter hamda test skriptlarini ishga soling. Ataylab xato ishlangan kod o'zgarishini yarating (masalan linter talablariga zid o'zgarish kiriting) hamda haqiqatdan ham uning buzilishini CI bilib qolishini ko'ring.
1. Shaxsiy loyihangizni birortasida "dangerous pattern" hisoblangan `subprocess.Popen(..., shell=True)` qatorlari ishtirok etishini tekshirish uchun `grep` [buyrug'i](/2026/course-shell/) orqali ishlashi mumkin bo'lgan regex [shablon qidiruvchisini](#regulyar-ifodalar) o'ylab toping. Keyin bu qidiruv tizimini biror xato yo'li bilan chetlab o'tish (buzish) mumkinligini aniqlang. Endi xuddi o'sha tekshiruvdagi buzilgan joylarni baribir o'tkazib yubormasdan ishlashda [semgrep](#lintlash) vositasidan umid qilib ko'rasizmi?
1. O'z IDE yoki matn muharriringiz yordamida quyidagi [ushbu darslik matnidagi](https://raw.githubusercontent.com/missing-semester/missing-semester/refs/heads/master/_2026/code-quality.md) har bir [Markdown qator yozuvi (bullet marker)](https://spec.commonmark.org/0.31.2/#bullet-list-marker) bo'lgan `-` chiziqchalarini `*` yulduzchalari bilan regex yordamida qidirib o'rniga qo'yishni mashq qiling. Ammo eslab qoling: hujjat ichidagi barcha "-" larni ham to'laligicha yulduzga aylantirmoqchi bo'lsangiz xatoga yo'l qilasiz. Chunki faqatgina bullet formatdagi emas balki ko'plab boshqa hollarda qator oxirlari chiziqcha bo'ladi.
1. Shunday maxsus regex yozingki unda `{"name": "Alyssa P. Hacker", "college": "MIT"}` shablonidagi kabi JSON yozuvlari kelsa ulardan malumot egalarini, ya'ni bizning misol uchun `Alyssa P. Hacker` isimlarini tushunib tortib ola bilsin. Qo'shimcha e'tibor: bu kabi testning dastlabkisida `Alyssa P. Hacker", "college": "MIT` degan kabi keraksiz simvollarni ham o'zlashtirgan variant qilib qo'yishingiz mumkin bo'lib; shunga shunday bo'lmasligi uchun greedy quantifiers (ko'p miqdor egallaydigan vositalar) to'g'risida [Python regex doclaridan](https://docs.python.org/3/library/re.html) o'qishingiz tavsiya qilinadi.
    1. Hatto ba'zi ismlarda (`\"`) formatida keladigan simvollar aralashgan hollarda ham ishlaydigan mukammal moslik topuvchisini ishlab chiqing.
    1. Ammo murakkab parsing kabi mashaqqat talab qilinuvchi o'rinlarda biz aslo regex instrumentlaridan doimiy ishlab borishni tavsiya qilmaymiz. O'z programmingiz yordamida qanday qilib JSON vositalarini tahlillash orqali buni oddiy muammo xususiga o'tkazishni aniqlab chiqing. Yuqoridagiga asoslanib faqat bir ikkita qator kod yozish yordamida buyruqlar satri(stdin) orqali yozilgan har qanday JSON faylni o'qish (stdout) natijasiga faqat kerak ismni olib chiqaruvchi funksiya loyihalashtiring. Pythonda ushbu masala shunchaki `import json` qilingandan keyin asosan bir satrlik funksiya bo'la oladi xolos.