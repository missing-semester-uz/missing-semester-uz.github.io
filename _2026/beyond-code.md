---
layout: lecture
title: "Koddan tashqari"
description: >
  Muhim yumshoq ko'nikmalar, jumladan hujjatlashtirish, ochiq kodli hamjamiyat me'yorlari va AI odoblari haqida bilib oling.
thumbnail: /static/assets/thumbnails/2026/lec8.png
date: 2026-01-22
ready: true
video:
  aspect: 56.25
  id: 2DOEATfXT8k
---

Yaxshi dasturiy ta'minot muhandisi bo'lish faqatgina ishlaydigan kod yozishdangina iborat emas. Bu boshqalar (jumladan, kelajakdagi o'zingiz) tushuna oladigan, qo'llab-quvvatlaydigan va ustiga qura oladigan kod yozishdir. Bu ochiq tushuntirish, o'ylab hissa qo'shish va siz ishtirok etayotgan ekotizimlarda — xoh u ochiq kodli, xoh yopiq loyihalarda bo'lsin — yaxshi fuqaro bo'lish demakdir.

# Bir tomonlama aloqa

Dasturiy ta'minot muhandisligining katta qismi joriy kontekstingizni bilmaydigan odamlar uchun yozishdan iborat: jamoaga keyinroq qo'shiladigan jamoadoshlar, kodingizni qabul qilib oladigan maintainer'lar yoki nega aynan shunday qarorga kelganingizni unutib qo'yadigan olti oydan keyingi o'zingiz. Barcha turdagi bunday yozishmalar uchun eng muhim maslahat shuki, sizning maqsadingiz faqatgina *nima* qilinganini emas, balki uning *sababini* (why) ham ko'rsatish va yetkazishdir. *Nima* qilingani odatda tushunarli bo'ladi, lekin uning *sababi* mashaqqat bilan topilgan va vaqt o'tishi bilan oson unutiladigan bilimdir.

Balki muhandislar o'rtasidagi eng keng tarqalgan muloqot shakli (kodning o'zidan tashqari) kod izohlaridir. Men shaxsan ko'pgina kod izohlarining foydasiz ekanini ko'rganman. Lekin ular bunday bo'lishi shart emas! Yaxshi izohlar kodning o'zi tushuntirib berolmaydigan narsalarni tushuntiradi: biror narsa *qanday* ishlashini emas (bu kodning o'zida ko'rinadi), nima uchun aynan shunday qilinganini tushuntiradi. Ular soatlab vaqtni tejab, chalkashliklarning oldini oladi, yomon izohlar esa shovqin qo'shadi yoki undan ham yomoni, adashtiradi.

Deyarli har doim yozishga arziydigan izoh turlari:

- **TODO'lar**: Chala yoki silliqlanmagan kodni belgilang, lekin boshqa birov nima qilinmaganligini va nima uchun qoldirilganligini tushunishi uchun yetarli kontekst qoldiring. "TODO: optimallashtirish" foydasiz; "TODO: ushbu O(n²) sikl `n<100` uchun yetarli, ammo biz masshtablashsak, indekslash kerak bo'ladi" degani aniq ko'rsatmadir.
- **Havolalar**: Agar kod maqoladagi algoritmni amalga oshirsa, boshqa joydan kodni moslashtirsa yoki hujjatlarda ko'rsatilgan xatti-harakatni ifodalasa, tashqi manbalarga havola bering. Doimiy havolalardan (permalink) foydalaning. Asl manbadan har qanday farqlarni yozib qo'ying.
- **To'g'rilik dalillari**: Nega oddiy bo'lmagan kod to'g'ri natijalarni berishini tushuntiring. Kod qadamlarni ko'rsatadi; izoh esa nima uchun bu qadamlar ishlashini tushuntiradi.
- **Mashaqqat bilan o'rganilgan darslar**: Agar siz biror narsani debag qilish uchun 30+ daqiqa sarflagan bo'lsangiz va yechim kutilmagan ibora bo'lsa, uni hujjatlashtiring. Sizning o'tmishdagi holatingiz buning kerakligini bilmagan edi; kelajakdagi o'quvchilar ham bilishmaydi.
- **O'zgarmaslar (konstantalar) mantiqiy asoslari**: Sehrli raqamlar tushuntirishga loyiq. Nega 1492? Nega 16 bit? Bu tasodifan tanlandimi, test natijasida kelib chiqdimi yoki to'g'ri ishlashi uchun kerakmi? Hatoki "ixtiyoriy ravishda tanlangan" degani ham foydali ma'lumotdir.
- **Muhim tanlovlar**: Agar to'g'rilik qandaydir zararsiz ko'ringan amalga oshirish detaliga bog'liq bo'lsa (masalan, "iteratsiya tartibi quyida muhim bo'lgani uchun BTreeSet bo'lishi kerak"), buni ochiq ayting.
- **"Nima uchun bunday qilinmadi" (Why not)**: Siz ataylab oson yoki odatiy usuldan qochganingizda, nega shunday qilganingizni tushuntiring. Aks holda kimdir keyinroq uni "tuzatib", narsalarni buzib qo'yadi.

README'lar (sizda bor, shundaymi?) ham boshqa dasturchilar bilan eng keng tarqalgan birinchi aloqa nuqtasi hisoblanadi. Yaxshi README to'rtta savolga darhol javob beradi: Bu nima qiladi? Bu menga nega kerak? Undan qanday foydalanaman? Uni qanday o'rnataman? Aynan shu tartibda. Uni voronka (funnel) kabi tuzing: eng tepada bir qatorli ta'rif va ehtimol vizual demo, shunda kimdir soniyalar ichida bu ularning muammosini hal qiladimi yoki yo'qligini hal qilishi mumkin, so'ngra asta-sekin chuqurlashib boring. O'rnatishdan oldin undan qanday foydalanishni ko'rsating — odamlar sozlash bosqichlariga kirishishdan oldin nima olishlarini ko'rishni xohlashadi.

Commit xabarlari ham ko'pincha e'tibordan chetda qoladigan "boshqalar uchun yozish" ning yana bir turidir. Ular ko'pincha "blah tuzatildi" yoki "foo qo'shildi" deb yoziladi va bu ba'zi hollarda yetarli bo'lishi mumkin bo'lsa-da, ular kod bazasi nima uchun aynan shunday rivojlanganligi haqidagi tarixiy yozuvlarni shakllantirishini unutish oson. Kimdir (shu jumladan siz!) tushunarsiz o'zgarishni tushunishga harakat qilib `git blame`'ni ishga tushirganda, yaxshi commit xabarlari unga javob berishi kerak.

Umuman olganda, xabarning asosiy qismi quyidagilarga javob berishi kerak:
- Ushbu o'zgarishni amalga oshirishga qanday muammo majbur qildi?
- Qanday alternativalarni ko'rib chiqdingiz?
- Buning foyda va zararlari (trade-offs) yoki oqibatlari qanday?
- Ushbu yondashuvda nima hayratlanarli bo'lishi mumkin?

> Albatta, siz batafsillikni qiyinlik darajasiga qarab o'lchashingiz kerak. Bir qatorli xatoni tuzatish faqat mavzuni talab qiladi. Debag qilish uchun soatlab vaqt ketgan nozik poyga holatini tuzatish muammo va echimni tushuntiruvchi xatboshilarga loyiqdir.

Murakkab o'zgarishlar uchun Muammo → Yechim → Oqibatlar tuzilmasiga amal qilish foydali bo'lishi mumkin: Cheklov yoki muammolardan boshlang, nima o'zgargani va asosiy dizayn qarorlarini tushuntiring va keyin muhim oqibatlarni (ijobiy va salbiy) sanab o'ting. Oxirgi qism ayniqsa muhim; haqiqiy muhandislik turli jihatlarni muvozanatlashni o'z ichiga oladi va biror kelishuvning ataylab qilinganligini hujjatlashtirish kelajakdagi dasturchilarga sizning muammoni o'tkazib yuborganingizni o'ylashlariga to'sqinlik qiladi.

LLM'lar commit xabarlarini yozishda foydali bo'lishi _mumkin_. Ammo, agar siz shunchaki unga o'zgartirishingizni ko'rsatsangiz va undan o'zgarish uchun commit xabari yozishni so'rasangiz, LLM faqat *nima* bo'lganini biladi, uning *sababini* bilmaydi. Natijada yozilgan commit xabari asosan ta'riflovchi bo'ladi (biz xohlagan narsaning teskarisi!). Agar siz o'zgarish qilish uchun birinchi navbatda LLM'dan foydalansangiz, LLM'dan aynan shu sessiyada commit xabarini yozishni so'rash ancha yaxshiroq variant bo'lishi mumkin, chunki sizning LLM bilan suhbatingiz aslida o'zgarish haqidagi boy kontekst manbaidir! Aks holda yoki bunga qo'shimcha ravishda, LLM'ga *nima uchun* (va yuqoridagi eslatmalardan boshqa jihatlar) asoslangan commit xabarini xohlayotganingizni aniq aytish va so'ngra _yetishmayotgan kontekst uchun sizdan so'rashini buyurish_ foydali hiyladir. Aslida, siz dasturlash agenti uchun u kontekstni "o'qish" uchun ishlata oladigan MCP "vositasi" kabi harakat qilyapsiz.

O'zgarishlaringiz qiyinlashgani sari, commit'larni mantiqan qismlarga ajratishga harakat qiling (`git add -p` sizning do'stingizdir). Har bir commit mustaqil ravishda tushunilishi va ko'rib chiqilishi mumkin bo'lgan bitta yaxlit o'zgarishni ifodalashi kerak. Yangi xususiyatlar bilan refaktor qilishni aralashtirmang yoki bir-biriga aloqasi bo'lmagan xatolarni tuzatishni birlashtirmang, chunki bu qaysi o'zgarish qaysi muammoni hal qilganligi haqidagi hikoyani chalkashtiradi va keyinchalik o'zgarishlaringizni ko'rib chiqishni sekinlashtiradi. Bu shuningdek, `git bisect` orqali sizga katta imkoniyatlar beradi, lekin bu boshqa mavzu.

> Yana bir eslatma: texnik matnlarni yozishda tirishqoqroq bo'la boshlaganingizda va undan kengroq foydalanishni boshlaganingizda, o'quvchini hurmat qilishni unutmang. Boshlaganingizdan so'ng ortiqcha tushuntirish berib yuborish oson, lekin o'quvchi siz yozgan narsalarning _hech birini_ o'qimasligiga yo'l qo'ymaslik uchun bu istakka qarshi turishingiz kerak. Uning *sababini* tushuntiring va ularning holati uchun *qanday* ekanligini o'zlari tushunib olishlariga ishoning.

# Hamkorlik

Muhandis sifatida biz ko'p vaqtimizni klaviatura oldida kod yozish bilan o'tkazishimiz mumkin, biroq vaqtimizning katta qismi boshqalar bilan muloqot qilishga ham ketadi. Bu vaqt odatda hamkorlik va ta'lim o'rtasida bo'linadi va har ikkalasida ham malakani oshirishga sarflangan investitsiyaning foydasi katta bo'ladi.

## Hissa qo'shish

Siz xato haqida hisobot yuboryapsizmi, oddiy xatoni tuzatishga hissa qo'shyapsizmi yoki ulkan xususiyatni yaratyapsizmi, yodda tutingki, odatda hissa qo'shuvchilardan ko'ra foydalanuvchilar soni minglab barobar ko'proq va maintainer'lardan ko'ra hissa qo'shuvchilar o'nlab barobar ko'proq. Natijada, maintainer'larning vaqti juda tig'iz bo'ladi. Agar siz qo'shgan hissangiz qandaydir ijobiy natija berish imkoniyatini oshirmoqchi bo'lsangiz, hissanggizda shovqinga nisbatan foydali ma'lumot yuqori bo'lishini va maintainer'larning vaqtiga arziydigan darajada bo'lishini ta'minlashingiz kerak.

Masalan, yaxshi xato haqida hisobot maintainer'ning vaqtini hurmat qiladi va muammoni tushunish va qayta takrorlash uchun kerakli bo'lgan barcha narsani taqdim etadi:

- **Muhit**: OT, versiya raqamlari, tegishli konfiguratsiya
- **Nima kutgan edingiz** va **aslida nima bo'ldi**
- **Qayta takrorlash qadamlari**: Aniq bo'ling. "Tugmani bosing" deganidan ko'ra, "Admin sifatida tizimga kirganda /settings sahifasidagi Submit tugmasini bosing" deb yozish ancha foydaliroq.
- **Siz allaqachon nima qilib ko'rdingiz**: Bu takroriy takliflarning oldini oladi va siz qandaydir izlanish olib borganingizni ko'rsatadi

> Agar xavfsizlik zaifligini topsangiz, uni hammaga ochiq e'lon qilmang. Dastlab maintainer'lar bilan shaxsan bog'laning va muammoni e'lon qilishdan oldin uni tuzatishlari uchun ularga yetarli vaqt bering. Ko'pgina loyihalarda shu maqsadda SECURITY.md yoki unga o'xshash fayl mavjud.

**Mavjud muammolarni qidirib ko'rganingizga ishonch hosil qiling.** Sizning xato yoki xususiyat so'rovingiz avvalroq e'lon qilingan bo'lishi mumkin va yangi nusxasini yaratish o'rniga mavjud muhokamalarga qo'shimcha ma'lumot kiritish ancha afzalroq. Bundan tashqari, bu maintainer'lar uchun ortiqcha shovqinni kamaytiradi.

Agar qila olsangiz, minimal qayta takrorlanadigan misollar (minimal reproducible examples) juda qadrlidir. Ular maintainer'ning juda ko'p vaqt va kuchini tejaydi va ko'pincha muammoni ishonchli takrorlash uni tuzatishning eng qiyin qismi bo'ladi. Qolaversa, muammoni ajratib ko'rsatish uchun sarflagan kuchingiz ko'pincha muammoni yaxshiroq tushunishingizga yordam beradi va ba'zan uni o'zingiz tuzatishga ham yordam beradi.

Agar darhol javob olmasangiz, maintainer'lar ko'pincha vaqti cheklangan ko'ngillilar ekanligini yodda tuting. Agar siz ulardan javob kutayotgan bo'lsangiz, bir necha haftadan so'ng xushmuomalalik bilan eslatib o'tish normal holat; har kuni bezovta qilish esa noto'g'ri. Shunga o'xshab, "men ham shunday" qabilidagi izohlar yoki shunchaki terminaldagi xatolarni nusxalab joylashtirilgan xato haqida hisobotlar muammongizga e'tibor jalb etish borasida foydadan xoli bo'ladi.

Agar kod qismini hissa sifatida qo'shmoqchi bo'lsangiz, hissa qo'shish qoidalari bilan tanishib chiqing. Ko'pgina loyihalarda `CONTRIBUTING.md` fayli bor — unga amal qiling. Odatda kichik narsadan boshlashni xohlaysiz; xatolarni tuzatish yoki hujjatlarni yaxshilash loyiha jarayonlarini o'rganishda ajoyib dastlabki hissa hisoblanadi.

> Loyiha qanday litsenziyadan foydalanishini tekshiring, chunki siz qo'shgan har qanday kod aynan shu litsenziya ostida bo'ladi. Xususan, nusxalash huquqi bo'lgan litsenziyalarga (GPL kabi) e'tibor bering, bu keyingi o'zgarishlarni ochiq kodli bo'lishini talab qiladi va agar unga qo'l ursangiz, bu sizning ish beruvchingizga ta'sir qilishi mumkin!
> [choosealicense.com](https://choosealicense.com/) saytida ko'proq foydali ma'lumotlar bor.

Siz pull request ("PR") ochishga qaror qilganingizda, avvalo, o'zingiz qabul qilinishini xohlagan o'zgarishni alohida ekanligiga ishonch hosil qiling. Agar sizning PR'ingiz bir vaqtning o'zida boshqa aloqasiz ko'p narsalarni ham o'zgartirsa, katta ehtimol bilan uni tekshiruvchi sizga uni tozalashni so'rab qaytaradi. Bu sizning git commit'larni semantik jihatdan bog'liq qismlarga qanday bo'lishingiz kerakligiga o'xshaydi.

Ba'zi hollarda, agar sizda bir-biriga bog'liq bo'lmagan turli xil o'zgarishlar mavjud bo'lsa, lekin ular bitta xususiyatni ishga tushirish uchun barchasi kerak bo'lsa, barcha o'zgarishlarni o'z ichiga olgan kattaroq PR ni ochish mumkin bo'lishi mumkin. Lekin, bunday holatda, maintainer'lar o'zgarishni "commit'ma commit" ko'rib chiqish imkoniyatiga ega bo'lishlari uchun commit gigiyenasiga ahamiyat berish ayniqsa muhimdir.

Keyin, o'zgarishning "nima uchun" ligini yaxshi tushuntirganingizga ishonch hosil qiling. Faqat *nima* o'zgarganligini aytib qolmang — *nima uchun* o'zgartirish kerakligini va nima uchun bu muammoni hal qilishning yaxshi usuli ekanligini tushuntiring. Shuningdek, siz ko'rib chiqishda alohida e'tibor berilishi kerak bo'lgan o'zgarish qismlarini (agar mavjud bo'lsa) ochiqchasiga ajratib ko'rsatishingiz kerak. O'zgarishingiz mohiyatiga va `CONTRIBUTING.md` fayliga qarab, tekshiruvchilar o'zgarishni qanday tekshirish kabi qo'shimcha ma'lumotlarni ham ko'rishni xohlashlari mumkin.

> Dastlabki yondashuv sifatida biz loyihani "fork" qilish o'rniga to'g'ridan-to'g'ri upstream loyihalarga hissa qo'shishni tavsiya qilamiz. Fork qilish (litsenziya ruxsat bergan hollarda) siz qo'shmoqchi bo'lgan o'zgarishlar asl loyihaning qamrovidan tashqarida bo'lgan hollar uchungina saqlanishi kerak. Agar siz rostdan ham fork qilsangiz, albatta, asl loyihaga havola bering!

AI tezkor ravishda ishonarli ko'rinadigan kodlar va PR'larni yaratishni osonlashtiradi, lekin bu sizni o'z hissangizni tushunish mas'uliyatidan ozod qilmaydi. O'zingiz tushuntirib berolmaydigan AI tomonidan yozilgan kodni yuborish, maintainer'larga hatto uning muallifi tushunmaydigan kodni ko'rib chiqish va qo'llab-quvvatlashdek ortiqcha yuk tushiradi. Muammolarni aniqlash va xatolarni tuzatish yoxud yangi xususiyatlarni yaratish uchun AI'dan foydalanish normal holat, **faqat agar siz hali ham uni maintainer'larga yuk qilmay, mazmunli hissa bo'lishi darajasiga keltira olsangizgina**.

Yodda tutingki, maintainer'lar uchun PR ni qabul qilish uzoq muddatli javobgarlikni anglatadi. Ular ushbu kodni hissa qo'shuvchi ketganidan so'ng ham uzoq vaqt qo'llab-quvvatlashadi, shuning uchun ular niyati yaxshi bo'lgan, ammo loyiha yo'nalishiga to'g'ri kelmaydigan, ortiqcha murakkablik qo'shadigan yoki umuman yetarlicha asoslanmagan o'zgarishlarni rad etishlari mumkin. Hissangiz nega arzigulik ekanligini isbotlash sizning, ya'ni hissa qo'shuvchining zimmangizda.

> PR bo'yicha fikr-mulohazalar (feedback) olganingizda, yodda tutingki, yozgan kodingiz bu shaxsan siz emassiz! Ko'rib chiquvchilar kodni yaxshilashga harakat qilishmoqda, sizni shaxsan tanqid qilishmayapti. Agar rozi bo'lmasangiz, savollar bering — ehtimol siz nimanidir o'rganarsiz yoki ular tushunib olishar.

## Ko'rib chiqish

Siz kodni ko'rib chiqishni asosan tajribali dasturchilar (senior developers) qilinadigan ish deb o'ylashingiz mumkin, lekin ehtimol sizdan kodni ko'rib chiqish ancha ertaroq so'ralishi mumkin va sizning nuqtai nazaringiz qadrlidir. Yangicha nigohlar tajribali dasturchilar e'tiborsiz qoldirgan narsalarni ko'radi va kod bilan unchalik tanish bo'lmagan kishining savollari ko'pincha hujjatlashtirilishi yoki soddalashtirilishi kerak bo'lgan jihatlarni fosh etadi.

Shuningdek, kodni ko'rib chiqish eng tez o'rganish usullaridan biridir. Siz boshqalarning muammolarga qanday yondashishini ko'rasiz, turli naqshlar va idiomalarni tushunasiz, hamda kodni o'qish qulayligi qanday bo'lishi kerakligi haqidagi bilimlaringizni oshirasiz. Shaxsiy o'sishdan tashqari, tekshirishlar mahsulot ishlab chiqarishga chiqqunga qadar muammolarni aniqlash, bilimlarni butun jamoa bo'ylab tarqatish va hamkorlik orqali kod sifatini oshirish imkonini beradi. Ular shunchaki byurokratik jarayon emas.

Kodni to'g'ri ko'rib chiqish vaqt o'tishi bilan o'stirilishi kerak bo'lgan ko'nikmadir, biroq ularni tezroq yaxshilashga yordam beruvchi ba'zi maslahatlar mavjud:

- **Shaxsni emas, kodni tekshiring**:
  "Sen chalkash kod yozibsan" o'rniga "Bu funksiya tushunarsiz" deng.
- **Amalga oshirsa bo'ladigan izohlar bering**:
  "Bu yerda globallardan foydalanmang" deyishdan ko'ra "Ushbu globallarni config ma'lumotlar sinfiga (dataclass) almashtira olasizmi?" deyish ancha qulay.
- **Talab qo'yishdan ko'ra savol so'rang**:
  "Null holatni tuzating" o'rniga "Bu yerda X null bo'lsa nima bo'ladi?" deyish muhokamani osonlashtiradi.
- **"Sababini" tushuntiring**:
  "Muhitga qarab kutish vaqtini oson sozlashimiz uchun, bu yerda o'zgarmasdan (constant) foydalanishni o'ylab ko'ring" deyish, "Bu yerda o'zgarmasdan foydalanishni o'ylab ko'ring" deyishdan ancha aniqroq.
- **To'sqinlik qiluvchi muammolarni takliflardan ajrating**:
  O'zgarishi shart bo'lgan narsalar va shunchaki tanlov masalasi bo'lgan narsalar o'rtasidagi farqni ochiq ko'rsating.
- **Yaxshi joylarini e'tirof eting**:
  Aqlli yechimlar yoki toza amalga oshirilgan joylarni e'tirof etish rag'batlantiradi va muallifga nimani davom ettirishi kerakligini tushunishga yordam beradi.
- **Qachon to'xtatishni biling**:
  Hissa qo'shuvchilarning vaqti va sabr-toqati cheklangan va barcha kichik tafsilotlarni (nits) to'g'rilash har doim ham o'zini oqlamaydi. Muhimroq narsalarga e'tibor qarating va o'sha kichik detallarni keyinchalik o'zingiz to'g'irlab qo'yishni o'ylab ko'ring.

> AI vositalari muayyan muammolarni ushlab olishi mumkin, ammo ular inson nazoratining o'rnini bosa olmaydi. Ular kontekstni o'tkazib yuborishadi, mahsulot talablarini tushunishmaydi va noto'g'ri narsalarni ishonch bilan taklif qilishlari mumkin. Ularni birinchi yondashuv sifatida ishlatish foydali, lekin o'ylab qilingan insoniy tekshirishning o'rniga emas.

# Ta'lim

Muhandis sifatida bizning kodlashdan tashqari vaqtimizning katta qismi yo savol berishga, yoki savolga javob berishga, yoxud ikkalasini aralashtirgan holda: hamkorlik, hamkasblar bilan muloqot va o'rganish davomida sarflanadi. Yaxshi savol berish ko'nikmasi har kimdan — faqat eng zo'r tushuntiruvchilardan emas — o'rganishingizga yordam beradi. Julia Evansning "[How to ask good questions](https://jvns.ca/blog/good-questions/)" (Qanday qilib yaxshi savollar berish kerak) va "[How to get useful answers to your questions](https://jvns.ca/blog/2021/10/21/how-to-get-useful-answers-to-your-questions/)" (Savollaringizga qanday qilib foydali javoblar olish mumkin) deb nomlangan juda ajoyib blog postlari bor, ularni o'qib chiqishni tavsiya qilamiz.

Bunga oid ba'zi xususiy va foydali maslahatlar:

- **Dastlab qanday tushunganingizni ayting**: Sizningcha nima bilishingizni ayting va "shu to'g'rimi?" deb so'rang. Bu javob beruvchiga bilimingizdagi bo'shliqlarni ko'rishga yordam beradi.
- **Ha/Yo'q deb javob beriladigan savollar so'rang**: "X to'g'rimi?" deyish ortiqcha tushuntirishlarning oldini oladi va ko'pincha baribir kerakli tafsilotlarga undaydi.
- **Aniq bo'ling**: "SQL joins qanday ishlaydi?" juda noaniq savol. "LEFT JOIN o'ng jadvalda hech qanday moslik bo'lmagan qatorlarni ham o'z ichiga oladimi?" javob berishga osonroq.
- **Tushunmaganingizni tan oling**: Notanish so'zlarni so'rash uchun gapni bo'ling. Bu ishonchni aks ettiradi, zaiflikni emas. Xuddi shunday, agar sizdan javobini bilmagan savollar so'rashsa, eng yaxshisi "Men bilmayman", keyin "lekin menimcha ..." yoki hatto "lekin men topishim mumkin" deyishingiz mumkin.
- **To'liqsiz javoblarni qabul qilmang**: Haqiqatan ham tushunmaguningizcha, qo'shimcha savollar beravering.
- **Avval ozroq izlanish qiling**: Asosiy tadqiqot qilsangiz, maqsadliroq savol berishingizga yordam beradi (garchi hamkasblar orasidagi oddiy savollar uchun buning hojati yo'q).

Yodda tuting: yaxshi tuzilgan savollar butun kompyuter ixlosmandlariga foyda keltiradi. Ular boshqalar ham tushunishi kerak bo'lgan yashirin jihatlarni ochib beradi.

> Esda tutingki, ushbu maslahatlar LLM'lar bilan muloqot qilishda ham to'laqonli tegishli!

# AI odoblari

Dasturiy ta'minot yaratishda LLM va AIdan foydalanish tobora o'sib borayotgani sababli, bu boradagi ijtimoiy va professional me'yorlar hali ham o'zgarish holatida. [Dasturlash agenti ma'ruzamizda](/2026/agentic-coding/) ko'plab taktik masalalarni qamrab olgan bo'lsak-da, ularni qo'llashdagi "yumshoqroq" jihatlarni ham muhokama qilish foydalidir.

Ulardan birinchisi - agar AI ishingizda muhim rol o'ynagan bo'lsa, **buni oshkora ayting**. Bu uyat emas — bu halollik, to'g'ri taxminlarni o'rnatish va natijaviy ishlarning tegishli darajada tekshirilishini ta'minlash bilan bog'liq. Shuningdek, qaysi _qismlari_ uchun AI'dan foydalanganingizni oshkor qilish ham muhim — "bu butun narsa vibe coding yordamida qilingan" va "men bu zahira(backup) vositasini yozdim, ammo veb-interfeysni uslublashda LLM'dan foydalandim" degan javoblar o'rtasida farq bor. Masalan, biz ba'zi ushbu ma'ruza yozuvlarini tayyorlashda, jumladan, matnni tahrir qilish, miyaga hujum qilish, shuningdek, kod parchalari va mashqlarni tayyorlashda LLM'lardan yordam sifatida foydalandik.

Bunda hamkorlik qilayotgan jamoalaringiz va loyihalaringiz tartib-qoidalariga rioya qilishni unutmang. Ba'zi jamoalar AI'dan foydalanish borasida boshqalarga qaraganda qattiqroq qoidalarga ega bo'lishadi (masalan, muvofiqlik yoki ma'lumotlar saqlanishi kabi sabablar bilan) va siz xatolik tufayli ularga qarshi chiqishni xohlamaysiz. AI'dan qanday foydalanayotganingiz borasida ochiqroq bo'lish, kelajakda yuzaga kelishi mumkin bo'lgan qimmatga tushuvchi xatoliklarning oldini oladi.

> Agar siz ish faoliyatingiz orqali o'rganishni maqsad qilsangiz, AI barcha yoki aksariyat ishlarni siz uchun qilib bersa, bu sizga aks ta'sir qilishi mumkinligini unutmang; ehtimol siz vazifaning o'zidan ko'ra prompting (yoki sun'iy intellekt xulosalarini ko'rib chiqish) haqida ko'proq o'rganarsiz. Ayniqsa siz o'rganayotgan bo'lsangiz, asosiy maqsad natija emas, balki izlanishdir. Shuning uchun "yechimni tezroq topish" uchun AI'dan foydalanish maqsadlaringizga mos kelmaydi.

Shunga o'xshash masalalar, ish intervyularida va boshqa baholash vaziyatlarida ham paydo bo'ladi. Ular odatda LLM'ni emas, faqat _sizning_ qobiliyatingizni va ko'nikmangizni baholash maqsadida qo'llaniladi. Aksar kompaniyalar hozirda suhbat paytida LLM va boshqa sun'iy intellektga asoslangan vositalardan foydalanishga ijozat bermoqda, agar ular intervyuning bir qismi bo'lsa (ya'ni, ular sizning asboblardan unumli foydalana olish ko'nikmangizni ham baholashadi!). Biroq bu hali ham kamchilikni tashkil etadi. Agar sun'iy intellekt vositalari qaysidir muayyan vazifa doirasiga kiritilishi haqida shubhalansangiz, avvalo, uni so'rab aniqlashtirib oling!

> Agar baholash holatida har qanday tashqi vositalar yoxud LLM'lardan foydalanmaslik ta'kidlab o'tilgan bo'lsa, bilish kerakki, siz ulardan foydalanmasligingiz kerak. Qandaydir ko'rinmas tarzda ularni ishlatishga urinish **oxiri** borib sizga teskari zarba berishi tayin.

# Mashqlar

1. Taniqli loyihaning kodini ko'rib chiqing (masalan, [Redis](https://github.com/redis/redis) yoki [curl](https://github.com/curl/curl)). Ma'ruzada eslatib o'tilgan izoh turlaridan ba'zilariga misollar toping: foydali TODO, tashqi hujjatlarga havola, nega bunday yondashuv ishlatilmagani tushuntirilgan "Why not" izohi yoki qiyinlik bilan o'rganilgan dars. Agar o'sha izoh bo'lmaganida, loyiha nimani yo'qotardi?

1. O'zingiz qiziqqan ochiq kodli loyihani tanlang va uning so'nggi commit tarixini (`git log`) ko'rib chiqing. O'zgarish *nima uchun* amalga oshirilganligini yaxshi tushuntirib bera oladigan bir yaxshi commit xabarini va faqat *nima* o'zgarganligini ko'rsatadigan bir kuchsiz commit xabarini toping. Kuchsiz xabar uchun farqni ko'ring (`git show <hash>`) va Muammo → Yechim → Oqibatlar tuzilmasidan foydalangan holda yaxshiroq commit xabarini yozishga harakat qiling. Keyinchalik kerakli kontekstni tiklash uchun qancha harakat kerakligiga e'tibor bering!

1. 1000+ yulduzi bor uchta GitHub loyihasining README'larini taqqoslang. Ular barchasi birdek foydalimi? Kelajakda o'zingiz yozadigan README'lar uchun shovqin deb biladigan, sizni chalg'itadigan xatoliklarni topishga harakat qiling.

1. O'zingiz foydalanayotgan loyihada ochiq muammoni (issue) toping (agar mavjud bo'lsa, "good first issue" yoki "help wanted" belgilarini tekshiring). Muammoni ma'ruzadagi mezonlarga asoslanib baholang: muammo hisobotiga qarab u maintainer'ning vaqtini qadrlagandek, muammoni hal qilish uchun hamma ma'lumotni yetarlicha yozgandekmi yoki maintainer muammo nimaligini tushunish uchun yuboruvchi bilan bir qancha marta yozishishga majburmi?

1. Foydalanadigan dasturiy ta'minotingizda duch kelgan xatoingizni eslang (yoki issue tracker'dan birortasini toping). Minimal qayta takrorlanadigan misol yaratishni mashq qiling: xato bilan bog'liq bo'lmagan hamma narsani olib tashlab, muammoni namoyish etishda davom etuvchi eng kichik kodni qoldiring. Nima uchun va qaysi narsalarni olib tashlaganingizni yozing.

1. O'zingiz yaxshi biladigan loyihada mazmunli tahlil qilingan ("LGTM" bo'lmagan) merge qilingan pull request toping. Sharhni oxirigacha o'qing. Barcha izohlar bir xil darajada unumli bo'lganmi? Agar siz PR muallifi bo'lganingizda, barcha sharhlarni qabul qilish tajribasi haqida qanday fikrda bo'lar edingiz?

1. Stack Overflow saytiga kiring va o'zingiz bilgan texnologiya haqida ko'p ijobiy ovoz olgan savolni toping. Shuningdek, yopib qo'yilgan yoki juda ko'p salbiy ovoz to'plagan bitta savolni qidiring. Ularni ma'ruzadagi maslahatlarga solishtiring; qaysi savol yaxshiroq javob olganini oldindan bilsa bo'larmidi?