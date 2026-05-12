---
layout: lecture
title: "Agentic Coding"
description: >
  Dasturlash vazifalari uchun AI dasturlash agentlaridan samarali foydalanishni o'rganing.
thumbnail: /static/assets/thumbnails/2026/lec7.png
date: 2026-01-21
ready: true
video:
  aspect: 56.25
  id: sTdz6PZoAnw
---

Dasturlash agentlari bu fayllarni o'qish/yozish, veb qidiruv va shell buyruqlarini ishga tushirish kabi vositalarga kirish imkoniga ega bo'lgan suhbatdosh AI modellaridir. Ular IDE'da yoki alohida buyruqlar satri yoxud grafik interfeysli vositalarda yashaydi. Dasturlash agentlari yuqori darajada avtonom va kuchli vositalar bo'lib, turli xil foydalanish holatlarini imkonini beradi.

Bu ma'ruza [Dasturlash muhiti va vositalar](/2026/development-environment/) ma'ruzasidagi AI yordamidagi dasturlash materialiga asoslanadi. Qisqacha misol sifatida [AI yordamidagi dasturlash](/2026/development-environment/#si-yordamida-dasturlash) bo'limidan namuna bilan davom etamiz:

```python
from urllib.request import urlopen

def download_contents(url: str) -> str:
    with urlopen(url) as response:
        return response.read().decode('utf-8')

def extract(content: str) -> list[str]:
    import re
    pattern = r'\[.*?\]\((.*?)\)'
    return re.findall(pattern, content)

print(extract(download_contents("https://raw.githubusercontent.com/missing-semester/missing-semester/refs/heads/master/_2026/development-environment.md")))
```

Biz dasturlash agentiga quyidagi vazifa bilan so'rov yuborib ko'rishimiz mumkin:

```
Turn this into a proper command-line program, with argparse for argument parsing. Add type annotations, and make sure the program passes type checking.
```

Agent faylni tushunish uchun uni o'qiydi, so'ngra ba'zi tahrirlarni amalga oshiradi va oxir-oqibat tur annotatsiyalari to'g'riligiga ishonch hosil qilish uchun turni tekshiruvchi dasturni ishga tushiradi. Agar u xato qilsa va turni tekshirishdan o'ta olmasa, u ehtimol buni takrorlaydi (iterate), garchi bu oddiy vazifa bo'lgani uchun bunday bo'lishi dargumon. Dasturlash agentlari zararli bo'lishi mumkin bo'lgan vositalarga kirish imkoniga ega bo'lganligi sababli, sukut bo'yicha agent harness'lari foydalanuvchidan vosita chaqiruvlarini tasdiqlashni so'raydi.

> Agar dasturlash agenti xato qilsa --- masalan, agar sizda `$PATH` orqali to'g'ridan-to'g'ri `mypy` binari mavjud bo'lsa, lekin agent `python -m mypy` ni chaqirishga harakat qilsa --- unga yo'nalishni to'g'rilashga yordam berish uchun matnli fikr-mulohaza berishingiz mumkin.

Dasturlash agentlari ko'p bosqichli o'zaro aloqani qo'llab-quvvatlaydi, shuning uchun agent bilan ikki tomonlama suhbat orqali ishni takrorlashingiz mumkin. Agar agent noto'g'ri yo'ldan ketayotgan bo'lsa, uni hatto to'xtatishingiz ham mumkin. Bunga yordam beradigan aqliy model amaliyotchining (intern) menejeri modeli bo'lishi mumkin: amaliyotchi mayda-chuyda ishlarni bajaradi, lekin yo'l-yo'riq talab qiladi va vaqti-vaqti bilan noto'g'ri ish qilib, to'g'rilanishi kerak bo'ladi.

> Yanada ko'rgazmaliroq misol uchun, agentdan davomi sifatida natijaviy skriptni ishga tushirishni so'rab ko'ring. Chiqishlarni kuzating va o'zgarish qilishni so'rang (masalan, faqat mutlaq yo'llarni kiritishni so'rang).

# AI modellari va agentlari qanday ishlaydi

Zamonaviy yirik til modellarining (LLM) va agent harness'lari kabi infratuzilmaning ichki ishlashini to'liq tushuntirish ushbu kurs doirasidan tashqarida. Biroq, ba'zi asosiy g'oyalarni yuqori darajada tushunish ushbu eng zamonaviy texnologiyadan samarali _foydalanish_ va uning cheklovlarini tushunish uchun foydalidir.

LLM'larga so'rov satrlari (kiritishlar) berilganda to'ldirish satrlari (chiqishlar) ehtimollik taqsimotini modellashtirish sifatida qarash mumkin. LLM inferensiyasi (masalan, suhbatdosh chat ilovasiga so'rov yuborganingizda nima sodir bo'ladi) ushbu ehtimollik taqsimotidan _namuna oladi_. LLM'lar qat'iy _kontekst oynasiga_ ega, bu kiritish va chiqish satrlarining maksimal uzunligidir.

{% comment %}
> In mathematical notation, the LLM models the probability distribution $\pi_\theta$ of completions $y$ conditioned on prompts $x$, and we sample from this distribution: $\hat{y} \sim \pi_\theta(\cdot \mid x)$.
{% endcomment %}

Suhbatdosh chat va dasturlash agentlari kabi AI vositalari ushbu primitiv ustida quriladi. Ko'p bosqichli o'zaro aloqalar uchun chat ilovalari va agentlar burilish belgilaridan foydalanadi va har safar yangi foydalanuvchi so'rovi bo'lganda butun suhbat tarixini so'rov satri sifatida taqdim etib, foydalanuvchi so'rovi uchun LLM inferensiyasini bir marta ishga tushiradi. Vosita chaqiradigan agentlar uchun harness ma'lum LLM chiqishlarini vositani chaqirish talablari sifatida talqin qiladi va harness vosita chaqiruvi natijalarini modelga so'rov satrining bir qismi sifatida qaytaradi (shuning uchun har safar vosita chaqiruvi/javobi bo'lganda LLM inferensiyasi qayta ishga tushadi). Vosita chaqiradigan agentlardagi asosiy tushunchalar [200 qator kodda amalga oshirilishi mumkin](https://www.mihaileric.com/The-Emperor-Has-No-Clothes/).

## Maxfiylik

Aksariyat AI dasturlash vositalari o'zlarining standart konfiguratsiyalarida ma'lumotlaringizning ko'p qismini bulutga yuboradi. Ba'zida harness mahalliy joyda ishlaydi, LLM inferensiyasi esa bulutda ishlaydi, boshqa paytlarda dasturiy ta'minotning yana ham ko'proq qismi bulutda ishlaydi (va masalan, xizmat ko'rsatuvchi provayder butun repozitoriyni, shuningdek sizning AI vositasi bilan qilgan barcha harakatlaringiz nusxasini samarali ravishda olishi mumkin).

Juda yaxshi bo'lgan ochiq kodli AI dasturlash vositalari va ochiq kodli LLM'lar mavjud (garchi xususiy modellar kabi yaxshi bo'lmasa ham), lekin hozirgi vaqtda ko'pchilik foydalanuvchilar uchun eng yangi ochiq LLM'larni mahalliy joyda ishga tushirish apparat cheklovlari tufayli imkonsiz bo'ladi.

# Foydalanish holatlari

Dasturlash agentlari turli xil vazifalar uchun foydali bo'lishi mumkin. Ba'zi misollar:

- **Yangi xususiyatlarni tatbiq etish.** Yuqoridagi misoldagi kabi, siz dasturlash agentidan biror xususiyatni tatbiq etishni so'rashingiz mumkin. Yaxshi spetsifikatsiya berish hozirgi bosqichda fandan ko'ra ko'proq san'atdir; siz agentga beriladigan kiritish yetarli darajada tavsiflovchi bo'lishini xohlaysiz, shunda agent nima qilishingizni xohlasa shuni bajaradi (hech bo'lmaganda siz takrorlashingiz uchun to'g'ri yo'nalishga qarab), lekin o'zingiz juda ko'p ish qiladigan darajada o'ta tavsiflovchi bo'lmasligi kerak. Testlarga asoslangan dasturlash ayniqsa samarali bo'lishi mumkin: testlar yozing (yoki testlar yozishda sizga yordam berishi uchun dasturlash agentidan foydalaning), ular siz xohlagan narsani qamrab olishini tekshirib chiqing va keyin dasturlash agentidan xususiyatni tatbiq etishni so'rang. Modellar doimiy ravishda takomillashib bormoqda, shuning uchun modellar nima qila olishi haqidagi sezgingizni doimo yangilab turishingiz kerak bo'ladi.
    > Biz ushbu Tufte uslubidagi yon eslatmalarni [tatbiq etish](https://github.com/missing-semester/missing-semester/pull/345) uchun Claude Code'dan foydalandik.
{%- comment %}
No need to demo this, since the intro of a lecture was a small demo of adding a new feature.
{% endcomment %}
- **Xatolarni tuzatish.** Agar sizda kompilyator, linter, turni tekshiruvchi dastur yoki testlardan xatolar bo'lsa, agentingizdan ularni to'g'rilashni so'rashingiz mumkin, masalan, "mypy bilan bog'liq muammolarni tuzat" degan so'rov bilan. Dasturlash modellari ayniqsa ularni fikr-mulohaza sikliga kiritganingizda samarali bo'ladi, shuning uchun model muvaffaqiyatsiz tekshiruvni to'g'ridan-to'g'ri ishga tushira oladigan qilib o'rnatishga harakat qiling, bu unga avtonom ravishda takrorlash imkonini beradi. Agar bu amaliy bo'lmasa, siz modelga o'zingiz qo'lda fikr-mulohaza berishingiz mumkin.
    > missing-semester repozitoriysining [f552b55](https://github.com/missing-semester/missing-semester/commit/f552b5523462b22b8893a8404d2110c4e59613dd) commitida, biz Claude Code'ga "Agentic coding ma'ruzasini imlo va grammatik xatolar uchun tekshirib chiqing" degan so'rov yubordik va keyinchalik topilgan muammolarni tuzatishni so'radik, bu [f1e1c41](https://github.com/missing-semester/missing-semester/commit/f1e1c417adba6b4149f7eef91ff5624de40dc637) da commit qilindi.
{%- comment %}
Demo a coding agent fixing the bug in https://github.com/anishathalye/dotbot/commit/cef40c902ef0f52f484153413142b5154bbc5e99.

Write the failing tests to demo the bug, and then ask the agent to fix. Prepped in branch demo-bugfix.

Can run the failing test with:

    hatch test tests/test_cli.py::test_issue_357

Can prompt coding agent with:

    There is a bug I wrote a failing test for, you can repro it with `hatch test tests/test_cli.py::test_issue_357`. Fix the bug.

Get it to commit the changes.
{% endcomment %}
- **Refaktor qilish.** Dasturlash agentlaridan kodni turli xil usullarda refaktor qilish uchun foydalanishingiz mumkin, metodni nomini o'zgartirish kabi oddiy vazifalardan tortib (bu turdagi refaktor qilish [kod intellekti](/2026/development-environment/#kod-intellekti-va-til-serverlari) orqali ham qo'llab-quvvatlanadi), funksionallikni alohida modulga ajratish kabi murakkabroq vazifalargacha.
    > Biz agentic coding'ni o'zining alohida ma'ruzasiga [ajratish](https://github.com/missing-semester/missing-semester/pull/344) uchun Claude Code'dan foydalandik.
{%- comment %}
Show usage in Missing Semester, point out that the agent did make some mistakes.
{% endcomment %}
- **Kodni ko'rib chiqish.** Dasturlash agentlaridan kodni ko'rib chiqishni so'rashingiz mumkin. Siz ularga "hali commit qilinmagan oxirgi o'zgarishlarimni ko'rib chiqing" kabi asosiy ko'rsatmalar berishingiz mumkin. Agar siz pull request'ni ko'rib chiqmoqchi bo'lsangiz va dasturlash agentingiz veb fetch'ni qo'llab-quvvatlasa, yoki [GitHub CLI](https://cli.github.com/) kabi buyruqlar satri vositalari o'rnatilgan bo'lsa, siz hatto dasturlash agentidan "{link} dagi pull request'ni ko'rib chiqing" deb so'rashingiz ham mumkin va u u yerdan o'zi davom ettiradi.
{%- comment %}
In Porcupine repo, prompt agent with:

    Review this PR: https://github.com/anishathalye/porcupine/pull/39
{% endcomment %}
- **Kodni tushunish.** Dasturlash agentidan kod bazasi haqida savollar so'rashingiz mumkin, bu ayniqsa loyihaga yangi qo'shilayotganda foydali bo'lishi mumkin.
{%- comment %}
Some prompts to try in the missing-semester repo:

    How do I run this site locally?

    How are the social preview cards implemented?
{% endcomment %}
- **Shell sifatida.** Siz dasturlash agentidan vazifani hal qilish uchun muayyan vositadan foydalanishni so'rashingiz mumkin, shuning uchun tabiiy tildan foydalanib shell buyrug'ini ishga tushirishingiz mumkin, masalan, "find buyrug'idan foydalanib, 30 kundan eski barcha fayllarni top" yoki "barcha jpg fayllarini o'zining asl o'lchamining 50% gacha kichraytirish uchun mogrify'dan foydalan".
{%- comment %}
In Dotbot repo, prompt agent with:

    Use the ag command to find all Python renaming imports
{% endcomment %}
- **Vibe coding.** Agentlar shunchalik kuchliki, siz ba'zi ilovalarni o'zingiz bir qator ham kod yozmasdan turib tatbiq etishingiz mumkin.
    > [Bu yerda](https://github.com/cleanlab/office-presence-dashboard) instruktorlardan biri vibe coding qilgan real loyihaga misol keltirilgan.
{%- comment %}
In missing-semester repo, prompt agent with:

    Make this site look retro.
{% endcomment %}

# Ilg'or agentlar

Bu yerda biz dasturlash agentlarining yanada ilg'orroq foydalanish usullari va imkoniyatlari haqida qisqacha ma'lumot beramiz.

- **Qayta ishlatiladigan so'rovlar (Reusable prompts).** Qayta ishlatiladigan so'rovlar yoki shablonlar yarating. Masalan, kodni ko'rib chiqishni ma'lum bir usulda bajarish uchun batafsil so'rov yozishingiz va uni qayta ishlatiladigan so'rov sifatida saqlashingiz mumkin.
    > Agent vositalari tez rivojlanadi. Ba'zi vositalarda alohida xususiyat sifatidagi qayta ishlatiladigan so'rovlar eskirgan hisoblanadi. Masalan, Codex va Claude Code'da ular [ko'nikmalar (skills)](https://code.claude.com/docs/en/skills) bilan [almashtirilgan](https://developers.openai.com/codex/custom-prompts).
- **Parallel agentlar.** Dasturlash agentlari sekin bo'lishi mumkin: siz agentga so'rov yuborishingiz mumkin va u o'nlab daqiqalar davomida muammo ustida ishlashi mumkin. Bir vaqtning o'zida agentlarning bir nechta nusxalarini ishga tushirishingiz mumkin, ular yo bir xil vazifa ustida ishlashi (LLM'lar stoxastikdir, shuning uchun bir xil narsani bir necha marta ishga tushirish va eng yaxshi yechimni olish foydali bo'lishi mumkin) yoki turli xil vazifalar ustida ishlashi (masalan, bir vaqtning o'zida bir-biriga mos kelmaydigan ikkita xususiyatni tatbiq etish) mumkin. Turli agentlarning o'zgarishlari bir-biriga xalaqit bermasligi uchun siz [versiyalarni boshqarish](/2026/version-control/) ma'ruzasida ko'rib chiqilgan [git worktree'laridan](https://git-scm.com/docs/git-worktree) foydalanishingiz mumkin.
- **MCP'lar.** _Model Context Protocol_ ma'nosini anglatuvchi MCP ochiq protokol bo'lib, undan dasturlash agentlaringizni vositalar bilan bog'lash uchun foydalanishingiz mumkin. Masalan, ushbu [Notion MCP serveri](https://github.com/makenotion/notion-mcp-server) agentingizga Notion hujjatlarini o'qish/yozish imkonini beradi, bu "{Notion hujjatida} havola qilingan spetsifikatsiyani o'qing, Notion'da yangi sahifa sifatida tatbiq etish rejasini tuzing va keyin prototipni tatbiq eting" kabi foydalanish holatlarini imkonini beradi. MCP'larni topish uchun siz [Pulse](https://www.pulsemcp.com/servers) va [Glama](https://glama.ai/mcp/servers) kabi kataloglardan foydalanishingiz mumkin.
- **Kontekstni boshqarish.** [Yuqorida](#ai-modellari-va-agentlari-qanday-ishlaydi) ta'kidlaganimizdek, dasturlash agentlarining asosini tashkil etuvchi LLM'lar cheklangan _kontekst oynasiga_ ega. Dasturlash agentlaridan samarali foydalanish kontekstdan to'g'ri foydalanishni talab qiladi. Agent kerakli ma'lumotlarga kirish imkoniga ega ekanligiga ishonch hosil qilishni xohlaysiz, lekin kontekst oynasi to'lib ketishining yoki model unumdorligi pasayishining (bu odatda kontekst oynasi to'lib ketmasa ham kontekst hajmi oshgani sari sodir bo'ladi) oldini olish uchun keraksiz kontekstdan qochishingiz kerak. Agent harness'lari kontekstni avtomatik ravishda taqdim etadi va ma'lum darajada boshqaradi, lekin ko'p nazorat foydalanuvchida qoldiriladi.
    - **Kontekst oynasini tozalash.** Eng asosiy boshqaruv, dasturlash agentlari kontekst oynasini tozalashni (yangi suhbatni boshlash) qo'llab-quvvatlaydi, siz bir-biriga bog'liq bo'lmagan so'rovlar uchun shunday qilishingiz kerak.
    - **Suhbatni orqaga qaytarish.** Ba'zi dasturlash agentlari suhbat tarixidagi qadamlarni bekor qilishni qo'llab-quvvatlaydi. Agentni boshqa yo'nalishga buruvchi keyingi xabarni berish o'rniga, "orqaga qaytarish" ko'proq mos keladigan holatlarda, bu kontekstni samaraliroq boshqaradi.
{%- comment %}
Make up a quick demo.
{% endcomment %}
    - **Siqish.** Chegaralanmagan uzunlikdagi suhbatlarni qo'llab-quvvatlash uchun dasturlash agentlari kontekstni _siqishni_ qo'llab-quvvatlaydi: agar suhbat tarixi juda uzun bo'lib ketsa, ular suhbat boshini xulosalash uchun avtomatik ravishda LLM'ni chaqiradilar va suhbat tarixini xulosa bilan almashtiradilar. Ba'zi agentlar foydalanuvchiga kerak bo'lganda siqishni chaqirish huquqini beradi.
{%- comment %}
Show `/compact` in Claude Code, show full summary.
{% endcomment %}
    - **llms.txt.** `/llms.txt` fayli inferensiya vaqtida LLM'lar foydalanishi uchun mo'ljallangan hujjatning taklif qilingan [standart](https://llmstxt.org/) joylashuvidir. Mahsulotlar (masalan, [cursor.com/llms.txt](https://cursor.com/llms.txt)), dasturiy kutubxonalar (masalan, [ai.pydantic.dev/llms.txt](https://ai.pydantic.dev/llms.txt)) va API'lar (masalan, [apify.com/llms.txt](https://apify.com/llms.txt)) dasturlash uchun qulay bo'lgan `llms.txt` fayllariga ega bo'lishi mumkin. Bunday hujjatlar har bir token uchun ko'proq ma'lumotga boy va shuning uchun ular dasturlash agentingizdan HTML sahifasini fetch qilish va o'qishni so'rashdan ko'ra kontekst jihatidan samaraliroqdir. Dasturlash agenti siz ishlatmoqchi bo'lgan qaramlik haqida o'rnatilgan bilimga ega bo'lmaganda tashqi hujjatlar juda qo'l keladi (masalan, u LLM bilimlarining cheklash sanasidan keyin nashr etilgan bo'lsa).
{%- comment %}
Side-by-side comparison in an empty repo (on Desktop or some other self-contained place, with `git init` run in it):

    Write a single-file Python program example in demo.py using semlib to sort "Ilya Sutskever", "Soumith Chintala", and "Donald Knuth" in terms of their fame as AI researchers.

    Write a single-file Python program example in demo.py using semlib to sort "Ilya Sutskever", "Soumith Chintala", and "Donald Knuth" in terms of their fame as AI researchers. See https://semlib.anish.io/llms.txt. Follow links to Markdown versions of any pages linked in llms.txt files.

Not sure why the agent doesn't do this by default. You'd probably put that last sentence in a CLAUDE.md file.
{% endcomment %}
    - **AGENTS.md.** Aksariyat dasturlash agentlari dasturlash agentlari uchun README sifatida [AGENTS.md](https://agents.md/) yoki shunga o'xshashni (masalan, Claude Code `CLAUDE.md` ni qidiradi) qo'llab-quvvatlaydi. Agent ishga tushganda, u kontekstni `AGENTS.md` ning to'liq mazmuni bilan oldindan to'ldiradi. Bundan agentga seanslar davomida umumiy bo'lgan maslahatlarni berish uchun foydalanishingiz mumkin (masalan, kodni o'zgartirgandan keyin har doim turni tekshiruvchi dasturni ishga tushirishni buyurish, birlik testlarini qanday ishga tushirishni tushuntirish yoki agent ko'rib chiqishi mumkin bo'lgan uchinchi tomon hujjatlariga havolalar berish). Ba'zi dasturlash agentlari ushbu faylni avtomatik yarata oladi (masalan, Claude Code'dagi `/init` buyrug'i). `AGENTS.md` ning haqiqiy misolini ko'rish uchun [bu yerga](https://github.com/pydantic/pydantic-ai/blob/main/CLAUDE.md) qarang.
{%- comment %}
Dotbot example, CLAUDE.md that includes @DEVELOPMENT.md and says to always run the type checker and code formatter after making any changes to Python code.

Example prompt, off of master:

    Remove the "--version" command-line flag.

This is something that'll be fast, for demonstration purposes.
{% endcomment %}
    - **Ko'nikmalar (Skills).** `AGENTS.md` dagi mazmun har doim agentning kontekst oynasiga to'liq yuklanadi. _Ko'nikmalar_ kontekst to'lib ketishining oldini olish uchun yana bir bilvosita darajani qo'shadi: siz agentga tavsiflari bilan birga ko'nikmalar ro'yxatini taqdim etishingiz mumkin va agent kerak bo'lganda ko'nikmani "ochishi" (uni o'zining kontekst oynasiga yuklashi) mumkin.
    - **Subagentlar.** Ba'zi dasturlash agentlari sizga aniq bir vazifaga yo'naltirilgan jarayonlar uchun subagentlarni yaratishga imkon beradi. Yuqori darajadagi dasturlash agenti muayyan vazifani bajarish uchun subagentni chaqirishi mumkin, bu ham yuqori darajadagi agentga, ham subagentga kontekstni samaraliroq boshqarishga yordam beradi. Yuqori darajadagi agentning konteksti subagent ko'rgan hamma narsa bilan to'lib ketmaydi va subagent o'z vazifasi uchun faqat o'ziga kerak bo'lgan kontekstni olishi mumkin. Bunga bir misol sifatida, ba'zi dasturlash agentlari veb qidiruvni subagent sifatida amalga oshiradi: yuqori darajadagi agent subagentga so'rov yuboradi, u veb qidiruvni amalga oshiradi, alohida veb sahifalarni yuklab oladi, ularni tahlil qiladi va yuqori darajadagi agentga so'rov uchun javobni taqdim etadi. Shu tariqa, yuqori darajadagi agentning konteksti barcha yuklab olingan veb sahifalarning to'liq mazmuni bilan to'lib ketmaydi va subagent ham o'z kontekstida yuqori darajadagi agentning qolgan suhbat tarixiga ega bo'lmaydi.

So'rovlarni yozishni talab qiladigan ko'plab ilg'or xususiyatlar (masalan, ko'nikmalar yoki subagentlar) uchun boshlashingizda LLM'lardan foydalanishingiz mumkin. Ba'zi dasturlash agentlarida buni qilish uchun o'rnatilgan qo'llab-quvvatlash ham mavjud. Masalan, Claude Code qisqa so'rovdan subagent yaratishi mumkin (`/agents` ni chaqiring va yangi agent yarating). Ushbu so'rov bilan subagent yaratib ko'ring:

```
A Python code checking agent that uses `mypy` and `ruff` to type-check, lint, and format *check* any files that have been modified from the last git commit.
```

Shundan so'ng, yuqori darajadagi agentdan "kodni tekshiruvchi subagentdan foydalan" kabi xabar bilan subagentni ochiq-oydin chaqirish uchun foydalanishingiz mumkin. Shuningdek, kerak bo'lganda, masalan, istalgan Python faylini o'zgartirgandan so'ng yuqori darajadagi agentga subagentni avtomatik ravishda chaqirishga ko'ndira olishingiz ham mumkin.

# Nimalarga e'tibor berish kerak

AI vositalari xato qilishi mumkin. Ular ehtimollik asosida keyingi tokenni bashorat qiluvchi modellar bo'lgan LLM'larga asoslangan. Ular odamlar kabi "aqlli" emas. AI chiqishini to'g'rilik va xavfsizlik xatolariga tekshirib chiqing. Ba'zan kodni tekshirish uni o'zingiz yozishingizdan ko'ra qiyinroq bo'lishi mumkin; o'ta muhim kodlar uchun uni qo'lda yozishni o'ylab ko'ring. AI quyon teshigiga kirib ketishi va sizni chalg'itishi mumkin; debag qilish spirallaridan xabardor bo'ling. AI'dan tayanch sifatida foydalanmang va unga o'ta bog'lanib qolishdan yoki yuzaki tushunishdan ehtiyot bo'ling. AI hozir ham bajarishga qodir bo'lmagan juda katta dasturlash vazifalari mavjud. Hisoblashli fikrlash hanuzgacha qadrlidir.

# Tavsiya etilgan dasturiy ta'minot

Ko'pgina IDE'lar / AI dasturlash kengaytmalari dasturlash agentlarini o'z ichiga oladi ([dasturlash muhiti ma'ruzasidagi](/2026/development-environment/) tavsiyalarni ko'ring). Boshqa mashhur dasturlash agentlariga Anthropic'ning [Claude Code](https://www.claude.com/product/claude-code), OpenAI'ning [Codex](https://openai.com/codex/) va [opencode](https://github.com/anomalyco/opencode) kabi ochiq kodli agentlari kiradi.

# Mashqlar

1. Bir xil dasturlash vazifasini to'rt marta bajarish orqali qo'lda yozish, AI avtoto'ldirish, inline chat va agentlardan foydalangan holda kod yozish tajribasini taqqoslang. Eng yaxshi nomzod - bu siz allaqachon ishlayotgan loyihadagi kichik hajmdagi xususiyat. Agar boshqa g'oyalarni qidirayotgan bo'lsangiz, GitHub'dagi ochiq kodli loyihalardagi "good first issue" uslubidagi vazifalarni yoki [Advent of Code](https://adventofcode.com/) yoxud [LeetCode](https://leetcode.com/) muammolarini hal qilishni ko'rib chiqishingiz mumkin.
1. Notanish kod bazasida harakatlanish uchun AI dasturlash agentidan foydalaning. Buni o'zingiz chindan ham g'amxo'rlik qiladigan loyihadagi nosozliklarni tuzatish yoki yangi xususiyat qo'shish istagi doirasida bajarish eng yaxshisidir. Agar xayolingizga hech narsa kelmasa, [opencode](https://github.com/anomalyco/opencode) agentida xavfsizlik bilan bog'liq xususiyatlar qanday ishlashini tushunish uchun AI agentidan foydalanib ko'ring.
1. Kichik dasturni noldan vibe coding qiling. O'zingiz bir qator ham kod yozmang.
1. O'zingiz tanlagan dasturlash agenti uchun `AGENTS.md` (yoki tanlagan agentingiz uchun unga o'xshash narsa, masalan, `CLAUDE.md`), ko'nikma (masalan, [Claude Code'dagi ko'nikma](https://code.claude.com/docs/en/skills) yoki [Codex'dagi ko'nikma](https://developers.openai.com/codex/skills/)) va subagentni (masalan, [Claude Code'dagi subagent](https://code.claude.com/docs/en/sub-agents)) yarating va test qilib ko'ring. Bularning birini boshqasidan ko'ra qachon ishlatishni xohlashingiz haqida o'ylab ko'ring. E'tibor bering, siz tanlagan dasturlash agenti ushbu funksionalliklarning ba'zilarini qo'llab-quvvatlamasligi mumkin; siz ularni o'tkazib yuborishingiz yoki ularni qo'llab-quvvatlaydigan boshqa dasturlash agentini sinab ko'rishingiz mumkin.
1. [Kod sifati ma'ruzasidagi](/2026/code-quality/) Markdown dagi ro'yxat nuqtalari uchun regulyar ifoda mashqidagi bir xil maqsadga erishish uchun dasturlash agentidan foydalaning. U vazifalarni bevosita fayllarni tahrirlash orqali yakunlaydimi? Bunday vazifani bajarish uchun agentning bevosita faylni tahrirlashining qanday kamchiliklari va cheklovlari bor? Agentni vazifani bevosita fayllarni tahrirlash orqali bajarmasligi uchun unga qanday so'rov yuborishni o'ylab toping. Maslahat: agentdan [birinchi ma'ruzada](/2026/course-shell/) aytib o'tilgan buyruqlar satri vositalaridan birini ishlatishni so'rang.
1. Aksariyat dasturlash agentlari qandaydir "yolo rejimini" qo'llab-quvvatlaydi (masalan, Claude Code'da, `--dangerously-skip-permissions`). Ushbu rejimdan bevosita foydalanish xavfsiz emas, biroq dasturlash agentini virtual mashina yoki konteyner kabi yakkalangan muhitda ishga tushirish va avtonom ishlashiga ruxsat berish maqbul bo'lishi mumkin. Ushbu sozlamani o'z mashinangizda ishga tushiring. [Claude Code devkonteynerlari](https://code.claude.com/docs/en/devcontainer) yoki [Docker Sandboxes / Claude Code](https://docs.docker.com/ai/sandboxes/agents/claude-code/) kabi hujjatlar qo'l kelishi mumkin. Buni o'rnatishning bir necha xil yo'li bor.