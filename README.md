# The Missing Semester of Your CS Education

[![Build Status](https://github.com/missing-semester-uz/missing-semester-uz.github.io/actions/workflows/build.yml/badge.svg)](https://github.com/missing-semester-uz/missing-semester-uz.github.io/actions/workflows/build.yml) [![Links Status](https://github.com/missing-semester-uz/missing-semester-uz.github.io/actions/workflows/links.yml/badge.svg)](https://github.com/missing-semester-uz/missing-semester-uz.github.io/actions/workflows/links.yml)

[Kompyuter fanlari ta’limingizning yetishmayotgan semestri](https://missing-semester-uz.github.io/) uchun vebsayt (*O'zbekcha tarjimasi*)

Inglizcha versiyasi (asl manba) [missing.csail.mit.edu](https://missing.csail.mit.edu/)

Bu kursni o‘zbek tiliga tarjima qilyapmiz. Agar tahrir yoki yangi bobni tarjima qilsangiz, shu repozitoriyada muammo oching yoki pull request yuboring.

## Loyiha holati

Tarjimaning joriy holatini [shu yerda](https://github.com/missing-semester-uz/missing-semester-uz.github.io?tab=readme-ov-file#loyiha-holati) kuzatib borishingiz mumkin.

Terminlarni tarjima qilishda qulaylik uchun hamma terminlarni [glossariyga](glossary.md) yig'dik. Agar biror termin tarjimasini to'g'irlasak yoki yaxshiroq variantini topsak, uni hamma fayllarda almashtira olamiz.

| # | bo‘limlar | tarjima holati |
| :-: |   --------   |  :----------:  |
| 1 | [course-shell.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/course-shell.md)  | `Tarjima jarayonida 🛠️` |
| 2 | [command-line-environment.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/command-line-environment.md)  | Tarjima boshlanmagan ⏳ |
| 3 | [development-environment.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/development-environment.md)  | Tarjima boshlanmagan ⏳ |
| 4 | [debugging-profiling.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/debugging-profiling.md)  | Tarjima boshlanmagan ⏳ |
| 5 | [version-control.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/version-control.md)  | Tarjima boshlanmagan ⏳ |
| 6 | [shipping-code.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/shipping-code.md)  | Tarjima boshlanmagan ⏳ |
| 7 | [agentic-coding.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/agentic-coding.md)  | Tarjima boshlanmagan ⏳ |
| 8 | [beyond-code.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/beyond-code.md)  | Tarjima boshlanmagan ⏳ |
| 9 | [code-quality.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2026/code-quality.md)  | Tarjima boshlanmagan ⏳ |
| 10 | [data-wrangling.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2020/data-wrangling.md)  | Tarjima boshlanmagan ⏳ |
| 11 | [security.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2020/security.md)  | Tarjima boshlanmagan ⏳ |
| 12 | [potpourri.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2020/potpourri.md) | Tarjima boshlanmagan ⏳ |
| 13 | [qa.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2020/qa.md) | Tarjima boshlanmagan ⏳ |
| 14 | [backups.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2019/backups.md) | Tarjima boshlanmagan ⏳ |
| 15 | [automation.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2019/automation.md) | Tarjima boshlanmagan ⏳ |
| 16 | [automation.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2019/automation.md) | Tarjima boshlanmagan ⏳ |
| 17 | [machine-introspection.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2019/machine-introspection.md) | Tarjima boshlanmagan ⏳ |
| 18 | [os-customization.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2019/os-customization.md) | Tarjima boshlanmagan ⏳ |
| 19 | [security.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/_2019/security.md) | Tarjima boshlanmagan ⏳ |
| * | [index.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/index.md) | `Tayyor ✔` |
| * | [about.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/about.md)  | `Tayyor ✔` |
| * | [license.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/license.md) | `Tayyor ✔` |
| * | [404.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/404.md) | `Tayyor ✔` |
| * | [past.md](https://github.com/missing-semester-uz/missing-semester-uz.github.io/blob/master/past.md) | `Tayyor ✔` |


## dev

Saytni local'da ko'tarish uchun quyidagi buyruqni ishga tushiring:

```bash
bundle exec jekyll serve -w
```

Agar saytni Docker konteynerida ko‘tarmoqchi bo‘lsangiz (masalan, o‘z kompyuteringizga Ruby va uning qo‘shimcha qismlarini o‘rnatishdan qochish uchun), quyidagi buyruqni ishga tushiring:

```bash
docker compose up --build
```

Sayt <http://localhost:4000>da ko‘rinishi kerak. Fayllarni o‘zgartirishingiz bilan Jekyll veb-saytni qaytadan quradi.

## Litsenziya

Ushbu kursdagi barcha materiallar, jumladan veb-sayt manba kodi, ma’ruza matnlari, mashqlar va ma’ruza videolari Attribution-NonCommercial-ShareAlike 4.0 International [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) litsenziyasi asosida tarqatiladi. Hissa qo‘shish yoki tarjima qilish bo‘yicha qo‘shimcha ma’lumot olish uchun [bu yerga](https://missing-semester-uz.github.io/license/) qarang.
