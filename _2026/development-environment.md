---
layout: lecture
title: "Dasturlash muhiti va vositalar"
description: >
  IDE'lar, Vim, til serverlari va SI yordamidagi dasturlash vositalari haqida bilib oling.
thumbnail: /static/assets/thumbnails/2026/lec3.png
date: 2026-01-14
ready: true
video:
  aspect: 56.25
  id: QnM1nVzrkx8
---

_Dasturlash muhiti_ (development environment) dasturiy ta'minot ishlab chiqish uchun yordamchi vositalar to'plamidir. Dasturlash muhitining markazida matn tahrirlash funksiyasi, shuningdek, sintaksisni yoritish (syntax highlighting), turni tekshirish (type checking), kodni formatlash va avtoto'ldirish kabi qo'shimcha imkoniyatlar yotadi. [VS Code][vs-code] kabi _integratsiyalashgan dasturlash muhitlari_ (IDE'lar) bu funksiyalarning barchasini bitta ilovada birlashtiradi. Terminalga asoslangan dasturlash jarayonlari tmux (terminal multipleksori), Vim (matn muharriri), Zsh (shell) kabi vositalarni hamda Ruff (Python linter'i va kodni formatlovchisi) va Mypy (Python uchun turni tekshiruvchi) kabi tilga xos buyruqlar satri vositalarini o'zida birlashtiradi.

IDE'lar va terminalga asoslangan jarayonlarning har biri o'zining kuchli va zaif tomonlariga ega. Masalan, grafik interfeysli IDE'larni o'rganish osonroq bo'lishi mumkin va bugungi IDE'lar odatda AI avtoto'ldirish kabi o'zida tayyor o'rnatilgan SI integratsiyalariga ega; boshqa tomondan, terminalga asoslangan jarayonlar yengil va grafik interfeysga ega bo'lmagan yoki dastur o'rnata olmaydigan muhitlarda yagona tanlovingiz bo'lishi mumkin. Biz har ikkalasi bilan ham har qalay tanishishni va kamida bittasini mukammal o'zlashtirishni tavsiya qilamiz. Agar sizda hali sevimli IDE bo'lmasa, [VS Code][vs-code] bilan boshlashni tavsiya qilamiz.

Ushbu ma'ruzada biz quyidagilarni ko'rib chiqamiz:

- [Matn tahrirlash va Vim](#matn-tahrirlash-va-vim)
- [Kod intellekti va til serverlari](#kod-intellekti-va-til-serverlari)
- [Sun'iy intellekt yordamida dasturlash](#si-yordamida-dasturlash)
- [Kengaytmalar va boshqa IDE funksiyalari](#kengaytmalar-va-boshqa-ide-funksiyalari)

[vs-code]: https://code.visualstudio.com/

# Matn tahrirlash va Vim

Dasturlash jarayonida vaqtingizning aksariyat qismi fayllarni yuqoridan pastgacha uzluksiz o'qish yoki uzun kodlar yozish bilan emas, balki kod bo'ylab harakatlanish, kodning ayrim qismlarini o'qish va kodga o'zgartirishlar kiritish bilan o'tadi. [Vim] aynan shu vazifalarni taqsimlash uchun optimallashtirilgan matn muharriridir.

**Vim falsafasi.** Vim o'zining negizida ajoyib bir g'oyaga ega: uning interfeysining o'zi matn bo'ylab harakatlanish va tahrirlash uchun mo'ljallangan dasturlash tilidir. Tugmalar (mnemonik nomlar bilan) buyruqlar bo'lib xizmat qiladi va bu buyruqlarni bir-biri bilan birlashtirish mumkin. Vim sichqonchadan foydalanishdan qochadi, chunki u juda sekin; Vim hatto yo'nalish tugmalaridan (arrow keys) foydalanishdan ham qochadi, chunki bu ortiqcha harakatni talab qiladi. Natija: miya-kompyuter interfeysiga o'xshaydigan va fikrlash tezligingizga mos keladigan muharrir.

**Boshqa dasturlarda Vim'ni qo'llab-quvvatlash.** Uning asosidagi g'oyalardan foydalanish uchun bevosita [Vim]'ning o'zidan foydalanishingiz shart emas. Har qanday matn tahrirlash bilan bog'liq bo'lgan ko'plab dasturlar "Vim rejimi" (Vim mode)ni ichki funksiya yoki plagin sifatida qo'llab-quvvatlaydi. Masalan, VS Code'da [VSCodeVim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) plagini, Zsh'da Vim emulyatsiyasi uchun [ichki yordam](https://zsh.sourceforge.io/Guide/zshguide04.html), va hatto Claude Code'da Vim muharriri rejimi uchun [ichki yordam](https://code.claude.com/docs/en/interactive-mode#vim-editor-mode) mavjud. Katta ehtimol bilan, matn tahrirlashni o'z ichiga olgan siz ishlatadigan har qanday vosita qaysidir ma'noda Vim rejimini qo'llab-quvvatlaydi.

## Rejimlardagi tahrirlash (Modal editing)

Vim _rejimli muharrir_ (modal editor) hisoblanadi: u turli toifadagi vazifalar uchun turli xil ishlash rejimlariga ega.

- **Normal**: fayl bo'ylab harakatlanish va o'zgartirishlar kiritish uchun
- **Insert** (Kiritish): matn kiritish uchun
- **Replace** (O'zgartirish): matnni o'zgartirish uchun
- **Visual** (oddiy, qator yoki blok): matn bloklarini tanlash uchun
- **Command-line** (Buyruqlar satri): buyruqni ishga tushirish uchun

Tugmalar turli ishlash rejimlarida farqli ma'nolarni anglatadi. Masalan, Insert rejimida `x` harfi shunchaki "x" belgisini kiritadi, lekin Normal rejimda u kursor tagidagi belgini o'chiradi, Visual rejimda esa tanlangan qismni o'chiradi.

Standart sozlamalarda, Vim joriy rejimni pastki chap burchakda ko'rsatadi. Dastlabki/standart rejim Normal rejimdir. Odatda siz vaqtingizning ko'p qismini Normal rejim va Insert rejimida o'tkazasiz.

`<ESC>` (escape tugmasi) bosish orqali ixtiyoriy rejimdan Normal rejimga qaytishingiz mumkin. Normal rejimdan turib Insert rejimiga `i`, Replace rejimiga `R`, Visual rejimiga `v`, Visual Line rejimiga `V`, Visual Block rejimiga `<C-v>` (Ctrl-V, ba'zan `^V` deb ham yoziladi) va Command-line rejimiga `:` orqali o'tasiz.

Vim'dan foydalanganda `<ESC>` tugmasidan juda ko'p foydalanasiz: Caps Lock tugmasini Escape'ga o'zgartirishni ko'rib chiqing ([macOS uchun qo'llanma](https://vim.fandom.com/wiki/Map_caps_lock_to_escape_in_macOS)) yoki oddiy tugmalar ketma-ketligi bilan `<ESC>` uchun [muqobil bog'lash](https://vim.fandom.com/wiki/Avoid_the_escape_key#Mappings) qilib oling.

## Asoslar: matn kiritish

Normal rejimdan Insert rejimiga o'tish uchun `i` tugmasini bosing. Endi Vim xuddi boshqa har qanday matn muharriri kabi ishlaydi, toki siz `<ESC>` ni bosib Normal rejimga qaytmaguningizgacha. Bularning barchasi va yuqoridagi asoslar Vim bilan fayllarni tahrirlashni boshlash uchun yetarli (agar butun vaqtingizni asosan Insert rejimida o'tkazmoqchi bo'lsangiz unchalik samarali emas, albatta).

## Vim interfeysi – bu dasturlash tili

Vim interfeysi xuddi dasturlash tili kabi ishlaydi. Tugmalar (mnemonik nomlar bilan) buyruqlar bo'lib, ular bir-birini _to'ldiradi_ (compose). Bu tezкор harakatlanish va o'zgartirishlar kiritish imkonini beradi, ayniqsa klaviaturingizni o'rgangandan so'ng xuddi mushaklar xotirasi kabi ishlaydi.

### Harakatlanish (Movement)

Siz vaqtingizning aksariyat qismini fayllar bo'ylab harakatlanish uchun Normal rejimda o'tkazishingiz kerak. Vim'dagi harakatlanishlar ham "otlar" (nouns) deyiladi, chunki ular matn bo'laklarini ifodalaydi.

- Asosiy harakatlar: `hjkl` (chap, past, tepa, o'ng)
- So'zlar: `w` (keyingi so'z), `b` (so'zning boshi), `e` (so'zning oxiri)
- Qatorlar: `0` (qator boshi), `^` (birinchi bo'sh bo'lmagan belgi), `$` (qatorning oxiri)
- Ekran: `H` (ekranning yuqori qismi), `M` (ekranning o'rta qismi), `L` (ekranning pastki qismi)
- Skrolling: `Ctrl-u` (tepaga), `Ctrl-d` (pastga)
- Fayl: `gg` (faylning boshi), `G` (faylning oxiri)
- Qator raqamlari: `:{number}<CR>` yoki `{number}G` ({number} qatoriga)
    - `<CR>` enter tugmasi vazifasini bildiradi
- Turli xil: `%` (mos keladigan juftlik, qavs va hakazo)
- Izlash (Find): `f{belgi}`, `t{belgi}`, `F{belgi}`, `T{belgi}`
    - joriy qatorda {belgini} oldinga/orqaga qarab qidirish
    - natijalar bo'ylab yurish uchun `,` / `;`
- Qidiruv (Search): `/{regulyar ifoda}`, mosliklar orasida yurish uchun `n` / `N`

### Tanlash (Selection)

Visual rejimlar:

- Visual: `v`
- Visual qator (Visual Line): `V`
- Visual blok (Visual Block): `Ctrl-v`

Tanlash uchun harakatlantirish tugmalaridan foydalansangiz bo'ladi.

### Tahrirlash (Edits)

Ilgari sichqoncha bilan qilingan barcha amallarni endi klaviatura orqali, harakatlantirish amallariga biriktirilgan holda amalga oshirasiz. Aynan shu yerda Vim interfeysi dasturlash tiliga o'xshashni boshlaydi. Vim'ning tahrir amallari "fe'llar" (verbs) deyiladi.

- `i` Insert rejimga o'tish
    - lekin matnni o'chirish/manipulyatsiya qilishda sizga backspace'dan ko'proq narsa kerak bo'ladi
- `o` / `O` pastda / yuqorida yangi qator ochish
- `d{motion}` {motion} bo'yicha o'chirish
    - masalan, `dw` - so'zni o'chirish, `d$` - qatorning oxirigacha, `d0` - boshigacha o'chirish
- `c{motion}` {motion} qismini o'zgartirish
    - masalan, `cw` so'zni o'zgartirish
    - `d{motion}` keyin esa `i` kabi ishlaydi
- `x` belgini o'chirish (bu `dl` ekvivalenti)
- `s` belgini almashtirish (bu `cl` ekvivalenti)
- Visual rejim + manipulyatsiya
    - matnni tanlang, o'chirish uchun `d` o'zgartirish uchun `c` bosing
- `u` ortga qaytarish, `<C-r>` qayta bajarish
- `y` nusxalash yoxud "yank" (`d` kabi boshqa amallar ham nusxa oladi)
- `p` joylashtirish (paste)
- Ko'proq o'rganish uchun: masalan, `~` harfning registrini o'zgartiradi, `J` qatorlarni birlashtiradi

### Sanoqlar (Counts)

Siz otlar va fe'llarni marta (count) soni yordamida birlashtirishingiz mumkin va bu harakat bir necha bor takrorlanadi.

- `3w` 3 ta so'z oldinga yurish
- `5j` 5 qator pastga siljish
- `7dw` 7 ta so'zni o'chirish

### Modifikatorlar (Modifiers)

Otlarning ma'nosini o'zgartirish uchun modifikatorlardan foydalanishingiz mumkin. Bular "ichki" degan ma'noni beruvchi `i` yoki "atrofi" degan ma'noni anglatuvchi `a` kabi belgilar.

- `ci(` qavslar ichidagi qatorni o'zgartiradi
- `ci[` to'rtburchak qavslar ichidagi qatorni o'zgartiradi
- `da'` bitta tirnoqli string'ni (qo'shtirnoqlarsiz stringni o'zi bilan) uzilish bilan o'chiradi

## Hammasini bir joyga jamlaganda

Bu yerda xato ishlangan [fizz buzz](https://en.wikipedia.org/wiki/Fizz_buzz) realizatsiyasi keltirilgan:

```python
def fizz_buzz(limit):
    for i in range(limit):
        if i % 3 == 0:
            print("fizz", end="")
        if i % 5 == 0:
            print("fizz", end="")
        if i % 3 and i % 5:
            print(i, end="")
        print()

def main():
    fizz_buzz(20)
```

Ishni Normal rejimdan boshlab muammolarni bartaraf qilish uchun biz quyidagi ketma-ketlikdan foydalanamiz:

- Main umuman chaqirilmagan
    - Fayl oxiriga sakrash uchun `G`
    - Pastroqda yangi qator ochish uchun **o** (open)
    - `if __name__ == "__main__": main()` deb yozing
        - Muharriringizda Python tiliga ega bo'lsangiz o'zi surib to'g'irlab qo'yishi mumkin
    - Normal rejimga qaytish uchun `<ESC>`
- U 1 o'rniga 0 dan boshlanyapti
    - `/` orqasidan `range` va qidirish uchun `<CR>`
    - Ikki **s**o'z oldiga yurish uchun `ww` (shuningdek, `2w` ishlatsa ham bo'ladi)
    - K**i**ritish (insert) rejimi uchun `i`, ustiga `1,` yozish
    - Normal rejimga qaytish uchun `<ESC>`
    - Keyingi so'zning o**x**iriga o'tish (end) uchun `e`
    - Qo'shish (**a**ppend) qilib boshlash uchun `a` bosiladi, qatorga `+ 1` qo'shiladi
    - Normal rejimga qaytish uchun `<ESC>`
    - 5 karrali qismi "fizz" qaytaryapti
    - 6-qatorga borish uchun `:6<CR>`
    - Qo'shtirnoqni i**ch**ini (**i**) almashtirish (change) `ci"`, uni `"buzz"` deb o'zgartiramiz
    - Normal rejimga qaytish uchun `<ESC>`

## Vim'ni o'rganish

Vim'ni o'rganishning eng yaxshi usuli uning asoslarini tushunib olish, keyin esa barcha dasturlaringizda Vim rejimini yoqish va uni amalda qo'llashdan iboratdir. Sichqoncha va ko'rsatkich tugmalaridan voz kechish juda foydali bo'ladi.

### Qo'shimcha manbalar

- Kursning o'tgan safargi nashridan [Vim ma'ruzasi](/2020/editors/) --- biz u yerda yana ham batafsil o'tdik
- `vimtutor` bu Vim o'rnatilganda keladigan darslik --- agar Vim o'rnatilgan bo'lsa uni terminalda ishga tushirish mumkin
- [Vim Adventures](https://vim-adventures.com/) bu Vim'ni o'yinda o'rganish
- [Vim Tips Wiki](https://vim.fandom.com/wiki/Vim_Tips_Wiki)
- [Vim Advent Calendar](https://vimways.org/2019/) Vim hiylalari asosan
- [VimGolf](https://www.vimgolf.com/) Vim UI orqali qilingan [kod golfi](https://en.wikipedia.org/wiki/Code_golf).
- [Vi/Vim Stack Exchange](https://vi.stackexchange.com/)
- [Vim Screencasts](http://vimcasts.org/)
- [Practical Vim](https://pragprog.com/titles/dnvim2/) (kitob)

[Vim]: https://www.vim.org/

# Kod intellekti va til serverlari

IDE'lar odatda [Language Server Protocol](https://microsoft.github.io/language-server-protocol/) protokoliga asoslangan interfeyslarni, ya'ni IDE kengaytmalari va til serverlarini amalga oshirishni taklif etadi. Ularda kodning semantik anglashini talab qiluvchi tilga xos amallar amalga oshiriladi. Masalan, VS Codedagi [Python kengaytmasi](https://marketplace.visualstudio.com/items?itemName=ms-python.python) bevosita [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) yordamida yoki [Go](https://marketplace.visualstudio.com/items?itemName=golang.go) kengaytmalari orqali ishlatilishi mumkin va h.k. O'zingiz ishlaydigan tillar bo'yicha serverlarni ulab kod yozganingizda, avtoto'ldirish singari ba'zi afzalliklarga erishasiz:

- **Kodni to'ldirish.** Avtoto'ldiruvchi kabi (object. yozilgan zahoti siz uni davomini ko'rasiz).
- **Inline documentation (Ichki hujjatlar).** Sichqonchangizni u yerga olib borsangiz ko'rish mumkin.
- **Jump-to-definition (O'rnatish nuqtasiga o'tish).** Qaysidir field qa'yerdan olingan bo'lsa huddi shu yerga sakrab o'tish imkonini beradi.
- **Find references (Bog'liqliklar qidiruvi).** Eng osoni kod qayerlarda ishlatilayapti ko'rsatib turadi.
- **Import bilan yordam.** O'chirib yuborilgan yoki yangi importlarni aytadi.
- **Kod sifati.** Avtomatik formatlashni berishi kabi afzalliklari bor. Bularning barchasini [Kod sifati](/2026/code-quality/) bo'limida yana ko'rib o'tamiz.

## Til serverlarini sozlash

Ba'zi tillar o'z-o'zidan sozlanadi. Boshqalari esa ishqalanish yuzaga keltirishi mumkin. Masalan, Python qanday ishlatilishini va qay manzilda paketlar bog'langanligini ko'rsatish darkor. Bu kabi mavzuni o'zimizning [dastur yetkazib berish](/2026/shipping-code/) darslarida ko'proq yoritamiz.

Tilga qarab turli xil sozlamalar mavjud bo'lishi mumkin. Masalan, Python type-checking shart bo'lmasa uni o'chirib qo'yishni taklif etadi.

# SI yordamida dasturlash

[GitHub Copilot][github-copilot] e'lon qilingandan buyon dasturlashda [LLM'lar](https://en.wikipedia.org/wiki/Large_language_model) ishlatilishni boshladi. Ayni paytda uchta eng ommabop variant bor: avtoto'ldirish, ichki suhbat (inline chat) va dasturlash agentlari (coding agents).

[github-copilot]: https://github.com/features/copilot/ai-code-editor

## Avtoto'ldirish (Autocomplete)

SI orqali to'ldirish an'anaviy kod muharrirlarida xuddi shunday formatga egaki, kursorni oldinga olgan sari u variantlarni chiaraveradi. Masalan, buni izohlardan orqali [prompt'lar bilan boshqarsh](https://en.wikipedia.org/wiki/Prompt_engineering) oson.

Masalan skript yozmoqchimiz deylik:

```python
import requests

def download_contents(url: str) -> str:
```

Model qolganini davom ettiradi:

```python
    response = requests.get(url)
    return response.text
```

Siz izohlarda tushuntirish kiritishingiz mumkin:

```python
def extract(contents: str) -> list[str]:
```

Bunga bunday natija beradi:

```python
    lines = contents.splitlines()
    return [line for line in lines if line.strip()]
```

Biz o'z sharhlarimizni izohlarga kiritsak bo'ladi:

```python
def extract(content: str) -> list[str]:
    # contentdan barcha Markdown havolalarinig oling
```

Bu marta esa yaxshiroq natija olamiz:

```python
    import re
    pattern = r'\[.*?\]\((.*?)\)'
    return re.findall(pattern, content)
```

Bu modelni aybi shu yerda ko'rinadi - faqatgina kursorda aytilgan joyini u ko'rishi mumkin xolos va module bo'limlariga ta'sir qilmaydi. Shuning uchun ham amaliyotda odatda eng asosiysi funksiyalarni aniq ravan nomlar, misol uchun `extract_links` qilib nomlash va docstrings qo'shish kabi qulay bo'ladigan tarzda xizmat ko'rsatishlari shart bo'ladi.

Va bu hamma narsani tugatadi:

```python
print(extract(download_contents("https://raw.githubusercontent.com/missing-semester/missing-semester/refs/heads/master/_2026/development-environment.md")))
```

## Ichki chat (Inline chat)

Block ko'rnishini ajratib olib sun'iy intellekt modelidan ushbu joriy amallarni tahrir qilishni taklif qilish mumkin. Masalan aytaylik siz har qanday qatorlarni to'g'rilamoqchisiz.

Davomi sifatida ko'rsak biz `requests` o'rniga boshqasini ishlata qolamiz. Satrni tanlaymiz va shunday yozamiz:

```
use built-in libraries instead
```

Model shuni beradi:

```python
from urllib.request import urlopen

def download_contents(url: str) -> str:
    with urlopen(url) as response:
        return response.read().decode('utf-8')
```

## Dasturlash agentlari (Coding agents)

Bular chuqur ravishda [Agentic dasturlash](/2026/agentic-coding/) bo'limida o'rnatilgan.

## Tavsiya qilinadigan dastur

Mashxur IDE kengaytmalar orasidan [GitHub Copilot][github-copilot] yoki [Cursor](https://cursor.com/) ni ayta olamiz. Talabalar uchun GitHub platformasida talabalik va ustozlar uchun [bepul beriladi](https://github.com/education/students). Bu joy uzluksiz integratsiya qilinadigan muhit.

# Kengaytmalar va boshqa IDE funksiyalari

Bu muhitlarni aynan _kengaytmalar_ (extensions) kuchaytirib beradi. Va ularga o'xshagan asosan qilinadigan ishlarga oqimning [VS Code kengaytmalar ommabopligi](https://marketplace.visualstudio.com/search?target=VSCode&category=All%20categories&sortBy=Installs) kiritilgan.

- [Dasturlash muhiti konteynerlari](https://containers.dev/): [VS Code da saqlanadigan o'rnatma](https://code.visualstudio.com/docs/devcontainers/containers) va boshqalr hammasi bor. Shuningdek konteynerlarda [kodni yetkazish](/2026/shipping-code/) yaxshi yorilib berilgan.
- Masofaviy dasturlash (remote serverlar) asosida ishlar ([Masofaviy SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)) ko'rsatib o'tilgan va bu orqali masalan GPU li katta qutulishli qismlarga kirishishingiz mumkin.
- Hamkorlikda (collaborative) tahrirlash kabi bir vaqtni ichida ishlash qismi ham plagin tufayli ta'minlanadi, masalan [Live Share](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare).

# Mashqlar

1. Barcha dasturlaringiz va ish instrumentlaringiz, masalan, fayllaringiz yoki shell, ichida Vim rejimini o'rnating, hamma tahrirlashda shundan bir oy davomida foydalaning. Agar nimadir sizni ko'proq qiynamoqchi bo'lsa, uni internet orqali tekshirish mumkin.
1. [VimGolf](https://www.vimgolf.com/) platformasidagi challlenjelarni bajaring.
1. O'zingiz kiritadigan loyihalar uchun qandaydir kengaytma o'rnating. U ishlashini bilish misol masalan bog'lanishlarga (dependencies) o'tib ko'ring. Agar kodingiz yo'q bo'lsa buni mana bunday ochiq kodli base ustida qilshingiz mumkin (misol, [mana buni](https://github.com/spf13/cobra)).
1. O'zingiz qiziqqan biror IDE kengaytmasi orqali shug'ullaning o'rganing.