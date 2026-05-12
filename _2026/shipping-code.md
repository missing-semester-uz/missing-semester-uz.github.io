---
layout: lecture
title: "Kodni paketlash va yetkazib berish"
description: >
  Loyiha paketlari, muhitlar, versiyalash va kutubxonalar, dasturlar hamda xizmatlarni yoyishni o'rganing.
thumbnail: /static/assets/thumbnails/2026/lec6.png
date: 2026-01-20
ready: true
video:
  aspect: 56.25
  id: KBMiB-8P4Ns
---

Kodni kutilganidek ishlashini ta'minlash qiyin; aynan o'sha kodni o'zingiznikidan farq qiluvchi mashinada ishga tushirish odatda undan ham qiyinroq.

Kodni yetkazib berish (shipping) degani, siz yozgan kodni olib, uni kompyuteringizdagi aniq sozlamalarsiz boshqa kishi ishga tushira oladigan yaroqli shaklga keltirishni anglatadi.
Kodni yetkazib berish ko'plab shakllarda bo'ladi va boshqa ko'plab omillar qatorida dasturlash tili, tizim kutubxonalari va operatsion tizim tanloviga bog'liq.
Bu shuningdek, nima qurayotganingizga ham bog'liq: dasturiy kutubxona, buyruqlar satri vositasi va veb-xizmat turli xil talablar va yoyish (deployment) bosqichlariga ega.
Shunga qaramay, bu ssenariylarning barchasida umumiy qonuniyat bor: biz yetkazib beriladigan mahsulot, ya'ni _artefakt_ nima ekanligini va uning atrofidagi muhit haqida qanday farazlar borligini aniqlashimiz kerak.

Ushbu ma'ruzada biz quyidagilarni ko'rib chiqamiz:

- [Qaramliklar va muhitlar](#qaramliklar-va-muhitlar)
- [Artefaktlar va paketlash](#artefaktlar-va-paketlash)
- [Relizlar va versiyalash](#relizlar-va-versiyalash)
- [Qayta tiklanuvchanlik](#qayta-tiklanuvchanlik)
- [Virtual mashinalar va konteynerlar](#virtual-mashinalar-va-konteynerlar)
- [Konfiguratsiya](#konfiguratsiya)
- [Xizmatlar va orkestratsiya](#xizmatlar-va-orkestratsiya)
- [Nashr qilish](#nashr-qilish)

Biz bu tushunchalarni Python ekotizimidagi misollar orqali tushuntiramiz, chunki aniq misollar tushunishga yordam beradi. Garchi boshqa dasturlash tillari ekotizimlari uchun vositalar farq qilsa ham, tushunchalar asosan bir xil bo'ladi.

# Qaramliklar va muhitlar

Zamonaviy dasturiy ta'minotni dasturlashda abstraksiya qatlamlari hamma joyda uchraydi.
Dasturlar tabiiy ravishda mantiqni boshqa kutubxonalar yoki xizmatlarga yuklaydi.
Biroq, bu sizning dasturingiz va uning ishlashi uchun kerak bo'lgan kutubxonalar o'rtasida _qaramlik_ munosabatini kiritadi.
Masalan, Python'da veb-sayt tarkibini olish uchun ko'pincha quyidagilarni bajaramiz:

```python
import requests

response = requests.get("https://missing.csail.mit.edu")
```

Biroq, `requests` kutubxonasi Python runtime bilan birga kelmaydi, shuning uchun agar biz bu kodni `requests`ni o'rnatmasdan ishga tushirishga harakat qilsak, Python xato beradi:

```console
$ python fetch.py
Traceback (most recent call last):
  File "fetch.py", line 1, in <module>
    import requests
ModuleNotFoundError: No module named 'requests'
```

Ushbu kutubxonadan foydalanish uchun avval uni o'rnatish maqsadida `pip install requests` buyrug'ini ishga tushirishimiz kerak.
`pip` - bu Python dasturlash tili paketlarni o'rnatish uchun taqdim etadigan buyruqlar satri vositasi.
`pip install requests` buyrug'ini bajarish quyidagi harakatlar ketma-ketligini hosil qiladi:

1. Python Package Index ([PyPI](https://pypi.org/)) da requests ni qidirish
1. Biz ishlayotgan platformaga mos artefaktni qidirish
1. Qaramliklarni hal qilish --- `requests` kutubxonasining o'zi boshqa paketlarga bog'liq bo'ladi, shuning uchun o'rnatuvchi barcha tranzitiv qaramliklarning mos versiyalarini topishi va ularni oldindan o'rnatishi kerak
1. Artefaktlarni yuklab olish, keyin o'ramdan chiqarish va fayllarni fayl tizimimizdagi to'g'ri joylarga ko'chirish

```console
$ pip install requests
Collecting requests
  Downloading requests-2.32.3-py3-none-any.whl (64 kB)
Collecting charset-normalizer<4,>=2
  Downloading charset_normalizer-3.4.0-cp311-cp311-manylinux_x86_64.whl (142 kB)
Collecting idna<4,>=2.5
  Downloading idna-3.10-py3-none-any.whl (70 kB)
Collecting urllib3<3,>=1.21.1
  Downloading urllib3-2.2.3-py3-none-any.whl (126 kB)
Collecting certifi>=2017.4.17
  Downloading certifi-2024.8.30-py3-none-any.whl (167 kB)
Installing collected packages: urllib3, idna, charset-normalizer, certifi, requests
Successfully installed certifi-2024.8.30 charset-normalizer-3.4.0 idna-3.10 requests-2.32.3 urllib3-2.2.3
```

Bu yerda ko'rishimiz mumkinki, `requests` o'zining `certifi` yoki `charset-normalizer` kabi qaramliklariga ega va ular `requests` o'rnatilishidan oldin o'rnatilishi kerak.
O'rnatilgandan so'ng, Python runtime uni import qilganda ushbu kutubxonani topa oladi.

```console
$ python -c 'import requests; print(requests.__path__)'
['/usr/local/lib/python3.11/dist-packages/requests']

$ pip list | grep requests
requests        2.32.3
```

Dasturlash tillarida kutubxonalarni o'rnatish va nashr qilish uchun turli xil vositalar, konvensiyalar va amaliyotlar mavjud.
Rust kabi ba'zi tillarda vositalar to'plami (toolchain) birlashtirilgan --- `cargo` qurish, testlash, qaramlikni boshqarish va nashr qilish bilan shug'ullanadi.
Python kabi boshqa tillarda birlashish spetsifikatsiya darajasida sodir bo'ladi --- bitta vosita o'rniga paketlash qanday ishlashini belgilaydigan standartlashtirilgan spetsifikatsiyalar mavjud bo'lib, bu har bir vazifa uchun bir nechta raqobatlashuvchi vositalarga imkon beradi (`pip` va [`uv`](https://docs.astral.sh/uv/) , `setuptools` va [`hatch`](https://hatch.pypa.io/) va [`poetry`](https://python-poetry.org/) ).
LaTeX kabi ba'zi ekotizimlarda esa TeX Live yoki MacTeX kabi tarqatmalar minglab oldindan o'rnatilgan paketlar bilan birga keladi.

Qaramliklarni joriy etish qaramlik ziddiyatlarini ham keltirib chiqaradi.
Ziddiyatlar (conflict) dasturlar bir xil qaramlikning mos kelmaydigan versiyalarini talab qilganda yuzaga keladi.
Masalan, agar `tensorflow==2.3.0` `numpy>=1.16.0,<1.19.0` ni talab qilsa va `pandas==1.2.0` `numpy>=1.16.5` ni talab qilsa, `numpy>=1.16.5,<1.19.0` ni qanoatlantiradigan har qanday versiya yaroqli bo'ladi.
Ammo agar loyihangizdagi boshqa paket `numpy>=1.19` ni talab qilsa, sizda barcha cheklovlarni qanoatlantiradigan yaroqli versiyasi bo'lmagan ziddiyat paydo bo'ladi.

Bu holat --- ya'ni bir nechta paketlar umumiy qaramliklarning o'zaro mos kelmaydigan versiyalarini talab qilishi --- ko'pincha _dependency hell_ (qaramlik do'zaxi) deb ataladi.
Ziddiyatlarni hal qilishning bir usuli har bir dasturning qaramliklarini o'zlarining shaxsiy _muhit_iga ajratishdir.
Python'da biz quyidagini ishga tushirish orqali virtual muhit yaratamiz:

```console
$ which python
/usr/bin/python
$ pwd
/home/missingsemester
$ python -m venv venv
$ source venv/bin/activate
$ which python
/home/missingsemester/venv/bin/python
$ which pip
/home/missingsemester/venv/bin/pip
$ python -c 'import requests; print(requests.__path__)'
['/home/missingsemester/venv/lib/python3.11/site-packages/requests']

$ pip list
Package Version
------- -------
pip     24.0
```

Muhitni tili runtimening o'rnatilgan paketlar to'plamiga ega bo'lgan to'liq mustaqil versiyasi deb o'ylashingiz mumkin.
Ushbu virtual muhit yoki venv o'rnatilgan qaramliklarni global Python o'rnatilishidan ajratib turadi.
Har bir loyiha uchun o'zi talab qiladigan qaramliklarni o'z ichiga olgan virtual muhitga ega bo'lish yaxshi amaliyotdir.

> Garchi ko'pgina zamonaviy operatsion tizimlar Python kabi dasturlash tillari runtimelarining o'rnatilmalari bilan birga kelsa ham, bu o'rnatilmalarni o'zgartirish oqilona emas, chunki operatsion tizim o'z funksionalligi uchun ularga tayanishi mumkin. O'rniga alohida muhitlardan foydalanishni afzal ko'ring.

Ba'zi tillarda o'rnatish protokoli vosita bilan emas, balki spetsifikatsiya sifatida aniqlanadi.
Python'da [PEP 517](https://peps.python.org/pep-0517/) qurish tizimi interfeysini belgilaydi va [PEP 621](https://peps.python.org/pep-0621/) loyiha metama'lumoti `pyproject.toml`da qanday saqlanishini ko'rsatadi.
Bu dasturchilarga `pip`ni yaxshilash va `uv` kabi optimallashtirilgan vositalarni ishlab chiqish imkonini berdi. `uv`ni o'rnatish uchun `pip install uv` qilish kifoya.

`pip` o'rniga `uv`dan foydalanish xuddi shu interfeysga amal qiladi, lekin ancha tezroq:

```console
$ uv pip install requests
Resolved 5 packages in 12ms
Prepared 5 packages in 0.45ms
Installed 5 packages in 8ms
 + certifi==2024.8.30
 + charset-normalizer==3.4.0
 + idna==3.10
 + requests==2.32.3
 + urllib3==2.2.3
```

> Iloji boricha `pip` o'rniga `uv pip`dan foydalanishni qat'iy tavsiya qilamiz, chunki u o'rnatish vaqtini keskin qisqartiradi.

Qaramliklarni izolyatsiya qilishdan tashqari, muhitlar sizning dasturlash tili runtimeingizning turli xil versiyalariga ega bo'lish imkonini ham beradi.

```console
$ uv venv --python 3.12 venv312
Using CPython 3.12.7
Creating virtual environment at: venv312

$ source venv312/bin/activate && python --version
Python 3.12.7

$ uv venv --python 3.11 venv311
Using CPython 3.11.10
Creating virtual environment at: venv311

$ source venv311/bin/activate && python --version
Python 3.11.10
```

Bu kodingizni bir nechta Python versiyalarida sinab ko'rishingiz kerak bo'lganda yoki loyiha ma'lum bir versiyani talab qilganda yordam beradi.

> Ba'zi dasturlash tillarida, har bir loyiha uning qaramliklari uchun o'zingiz qo'lda yaratishingiz o'rniga avtomatik ravishda o'z muhitini oladi, lekin tamoyil bir xil. Hozirgi kunda aksariyat tillarda bitta tizimda tilning bir nechta versiyalarini boshqarish mexanizmi mavjud va u yerdan alohida loyihalar uchun qaysi versiyadan foydalanishni ko'rsatib beradi.

# Artefaktlar va paketlash

Dasturiy ta'minotni ishlab chiqishda biz manba kodi (source code) va artefaktlar o'rtasida farq qilamiz. Dasturchilar manba kodini yozadilar va o'qiydilar, artefaktlar esa shu manba kodidan olingan o'rnatishga yoki yoyishga tayyor bo'lgan paketlangan, tarqatiladigan natijalardir.
Artefakt biz ishga tushiradigan kod fayli kabi oddiy bo'lishi ham, ilovaning barcha zaruriy qismlari va elementlarini o'z ichiga olgan butun Virtual mashina kabi murakkab bo'lishi ham mumkin.
Joriy katalogimizda Python fayli `greet.py` bo'lgan ushbu misolni ko'rib chiqing:

```console
$ cat greet.py
def greet(name):
    return f"Hello, {name}!"

$ python -c "from greet import greet; print(greet('World'))"
Hello, World!

$ cd /tmp
$ python -c "from greet import greet; print(greet('World'))"
ModuleNotFoundError: No module named 'greet'
```

Biz boshqa katalogga o'tganimizdan so'ng import qilish muvaffaqiyatsizlikka uchraydi, chunki Python faqat ma'lum joylardagi (joriy katalog, o'rnatilgan paketlar va `PYTHONPATH` dagi yo'llar) modullarni qidiradi. Paketlash bu muammoni kodni ma'lum qilingan joyga o'rnatish orqali hal qiladi.

Python'da kutubxonani paketlash `pip` yoki `uv` kabi paket o'rnatuvchilar tegishli fayllarni o'rnatish uchun foydalanishi mumkin bo'lgan artefakt ishlab chiqarishni o'z ichiga oladi.
Python artefaktlari _wheel_lar deb ataladi va paketni o'rnatish uchun zarur bo'lgan barcha ma'lumotlarni o'z ichiga oladi: kod fayllari, paket haqida metama'lumotlar (nomi, versiyasi, qaramliklari) va fayllarni muhitda qayerga joylashtirish bo'yicha ko'rsatmalar.
Artefaktni qurish loyihaning o'ziga xos xususiyatlari, kerakli qaramliklar, paket versiyasi va boshqa ma'lumotlarni batafsil bayon qiluvchi loyiha faylini (ko'pincha manifest sifatida tanilgan) yozishimizni talab qiladi. Python'da biz bu maqsad uchun `pyproject.toml`dan foydalanamiz.

> `pyproject.toml` zamonaviy va tavsiya etilgan usuldir. `requirements.txt` yoki `setup.py` kabi oldingi paketlash usullari hali ham qo'llab-quvvatlansa-da, iloji bo'lsa, `pyproject.toml` ni afzal ko'rishingiz kerak.

Bu yerda buyruqlar satri vositasini ham taqdim etuvchi kutubxona uchun minimal `pyproject.toml` keltirilgan:

```toml
[project]
name = "greeting"
version = "0.1.0"
description = "A simple greeting library"
dependencies = ["typer>=0.9"]

[project.scripts]
greet = "greeting:cli"

[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"
```

`typer` kutubxonasi minimal boilerplate bilan buyruqlar satri interfeyslarini yaratish uchun mashhur Python paketidir.

Va mos keladigan `greeting.py`:

```python
import typer


def greet(name: str) -> str:
    return f"Hello, {name}!"


def cli():
    typer.run(greet)


if __name__ == "__main__":
    cli()
```

Ushbu fayl bilan biz endi wheel'ni qura olamiz:

```console
$ uv build
Building source distribution...
Building wheel from source distribution...
Successfully built dist/greeting-0.1.0.tar.gz
Successfully built dist/greeting-0.1.0-py3-none-any.whl

$ ls dist/
greeting-0.1.0-py3-none-any.whl
greeting-0.1.0.tar.gz
```

`.whl` fayli bu wheel (ma'lum bir tuzilishga ega zip arxivi) va `.tar.gz` manbadan qurilishi kerak bo'lgan tizimlar uchun manba taqsimotidir.

Nima paketlanishini ko'rish uchun wheel tarkibini tekshirishingiz mumkin:

```console
$ unzip -l dist/greeting-0.1.0-py3-none-any.whl
Archive:  dist/greeting-0.1.0-py3-none-any.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
      150  2024-01-15 10:30   greeting.py
      312  2024-01-15 10:30   greeting-0.1.0.dist-info/METADATA
       92  2024-01-15 10:30   greeting-0.1.0.dist-info/WHEEL
        9  2024-01-15 10:30   greeting-0.1.0.dist-info/top_level.txt
      435  2024-01-15 10:30   greeting-0.1.0.dist-info/RECORD
---------                     -------
      998                     5 files
```

Endi agar biz ushbu wheel'ni boshqa birovga bersak, ular uni quyidagini ishga tushirish orqali o'rnatishlari mumkin edi:

```console
$ uv pip install ./greeting-0.1.0-py3-none-any.whl
$ greet Alice
Hello, Alice!
```

Bu biz avval qurgan kutubxonani o'z muhitiga, jumladan `greet` cli vositasini ham o'rnatadi.

Bu yondashuvda cheklovlar mavjud. Xususan, agar kutubxonamiz platformaga xos kutubxonalarga bog'liq bo'lsa, masalan GPU tezlashtirish uchun CUDA, unda bizning artefaktimiz faqat o'sha maxsus kutubxonalar o'rnatilgan tizimlarda ishlaydi va turli platformalar (Linux, macOS, Windows) hamda arxitekturalar (x86, ARM) uchun alohida wheellarni qurishimiz kerak bo'lishi mumkin.


Dasturiy ta'minotni o'rnatishda manbadan o'rnatish (installing from source) va oldindan qurilgan binarni (prebuilt binary) o'rnatish o'rtasida muhim farq bor. Manbadan o'rnatish degani original kodni yuklab olish va uni kompyuteringizda kompilyatsiya qilish demakdir --- bu kompilyator va o'rnatilgan qurish vositalarini talab qiladi va katta loyihalar uchun ancha vaqt talab qilishi mumkin.

Oldindan qurilgan binarni o'rnatish boshqa kishi tomonidan kompilyatsiya qilingan artefaktni yuklab olishni anglatadi --- bu tezroq va soddaroq, lekin binar sizning platformangiz va arxitekturangizga mos kelishi kerak.
Masalan, [ripgrep'ning relizlar sahifasi](https://github.com/BurntSushi/ripgrep/releases) da Linux (x86_64, ARM), macOS (Intel, Apple Silicon) va Windows uchun oldindan qurilgan binarlar ko'rsatilgan.


# Relizlar va versiyalash

Kod doimiy jarayon tarzida quriladi, lekin aniq (diskret) asosda reliz qilinadi.
Dasturiy ta'minot ishlab chiqishda (development) va ishlab chiqarish (production) muhitlari o'rtasida aniq farq bor.
Kodni prod'ga _yetkazib berish_dan oldin dev muhitida ishlashi isbotlanishi kerak.
Reliz jarayoni ko'plab bosqichlarni, shu jumladan testlash, qaramliklarni boshqarish, versiyalash, konfiguratsiya, yoyish va nashr qilishni o'z ichiga oladi.


Dasturiy kutubxonalar statik emas va vaqt o'tishi bilan xatolarni to'g'rilash (fixes) hamda yangi xususiyatlarni olgan holda rivojlanadi.
Biz bu evolyutsiyani kutubxonaning ma'lum bir vaqtdagi holatiga mos keladigan diskret versiya identifikatorlari orqali kuzatib boramiz.
Kutubxonaning xulq-atvoridagi o'zgarishlar muhim bo'lmagan funksiyalarni tuzatuvchi patchlardan (patchlar), o'z funksionalligini kengaytiruvchi yangi xususiyatlardan, eski versiyalar bilan moslashuvchanlikni (backwards compatibility) buzuvchi o'zgarishlargacha bo'lishi mumkin.
O'zgarishlar jurnali (changelog) versiyada qanday o'zgarishlar kiritilganini hujjatlashtiradi --- bular dasturchilar yangi reliz bilan bog'liq o'zgarishlarni yetkazish uchun ishlatadigan hujjatlardir.

Biroq, har bir qaramlikdagi davom etayotgan o'zgarishlarni kuzatib borish amaliy emas, ayniqsa biz tranzitiv qaramliklarni --- ya'ni bizning qaramliklarimizning qaramliklarini ko'rib chiqsak.

> Loyihangizning butun qaramlik daraxtini `uv tree` yordamida tasavvur qilishingiz mumkin, bu barcha paketlar va ularning tranzitiv qaramliklarini daraxt ko'rinishida ko'rsatadi.

Ushbu muammoni soddalashtirish uchun dasturiy ta'minotni qanday versiyalash haqida konvensiyalar mavjud va eng keng tarqalganlaridan biri [Semantik versiyalash](https://semver.org/) yoki SemVer.
Semantik versiyalash ostida versiya MAJOR.MINOR.PATCH ko'rinishidagi identifikatorga ega bo'ladi, bunda qiymatlarning har biri butun son qiymatini oladi. Qisqasi, yangilash:

- PATCH (masalan, 1.2.3 → 1.2.4) faqat xatoliklarni tuzatishlarni o'z ichiga olishi va to'liq orqaga mos bo'lishi kerak
- MINOR (masalan, 1.2.3 → 1.3.0) yangi funksiyalarni orqaga mos keladigan tarzda qo'shadi
- MAJOR (masalan, 1.2.3 → 2.0.0) kodni o'zgartirishni talab qilishi mumkin bo'lgan buzadigan (breaking) o'zgarishlarni bildiradi

> Bu soddalashtirish hisoblanadi va biz to'liq SemVer spetsifikatsiyasini o'qib chiqishni tavsiya qilamiz, masalan 0.1.3 dan 0.2.0 ga o'tish nima uchun buzuvchi o'zgarishlarga olib kelishi mumkinligini yoki 1.0.0-rc.1 nimani anglatishini tushunish uchun.
Python'ning paketlash qismi semantik versiyalashni mahalliy darajada qo'llab-quvvatlaydi, shuning uchun qaramliklarimizning versiyalarini ko'rsatganimizda turli xil spetsifikatorlardan foydalanishimiz mumkin:

`pyproject.toml` da qaramliklarimizning mos keluvchi versiyalari diapazonlarini cheklashning turli usullari mavjud:

```toml
[project]
dependencies = [
    "requests==2.32.3",  # Aniq versiya - faqat mana shu ma'lum versiya
    "click>=8.0",        # Minimal versiya - 8.0 yoki undan yangi
    "numpy>=1.24,<2.0",  # Diapazon - kamida 1.24, lekin 2.0 dan kichik
    "pandas~=2.1.0",     # Mos reliz - >=2.1.0 va <2.2.0
]
```

Versiya spetsifikatorlari ko'plab paketlar menejerlarida (npm, cargo, va hokazo) turlicha aniq semantikalar bilan mavjud. `~=` operatori Python'ning "mos reliz" operatoridir --- `~=2.1.0` degani "2.1.0 bilan mos keladigan har qanday versiya" ni bildiradi, bu `>=2.1.0` va `<2.2.0` ga tengdir. Bu taxminan SemVer ning moslik tushunchasiga amal qiluvchi npm va cargo dagi karetka (`^`) operatoriga ekvivalentdir.

Barcha dasturiy ta'minotlar ham semantik versiyalashdan foydalanmaydi. Keng tarqalgan alternativ - taqvim versiyalashi (CalVer), bunda versiyalar semantik ma'noga emas, balki reliz sanalariga asoslanadi. Masalan, Ubuntu `24.04` (Aprel 2024) va `24.10` (Oktyabr 2024) kabi versiyalardan foydalanadi. CalVer relizning qanchalik eski ekanligini ko'rishni osonlashtiradi, garchi u moslik haqida hech narsa bildirmasa ham.  Va nihoyat, semantik versiyalash ham bexato emas va ba'zida maintainer'lar minor yoki patch relizlarida bilib-bilmay buzuvchi o'zgarishlarni kiritib qo'yadilar.


# Qayta tiklanuvchanlik

Zamonaviy dasturiy ta'minot ishlanmalarida siz yozgan kod juda ko'p miqdordagi abstraksiya qatlamlari ustida yotadi.
Bunga dasturlash tilingiz runtime'i, uchinchi tomon kutubxonalari, operatsion tizim yoki hatto apparaturaning (hardware) o'zi ham kiradi.
Ushbu qatlamlarning birortasidagi har qanday farq kodingizning ishlash tarzini o'zgartirishi yoki hatto kutilganidek ishlashini to'xtatishi mumkin.
Bundan tashqari, hatto asosiy apparaturadagi farqlar ham dasturiy ta'minotni yetkazib berish qobiliyatingizga ta'sir qiladi.

Kutubxonani mixlash (pinning) diapazon o'rniga aniq versiyani ko'rsatishni bildiradi, masalan `requests>=2.0` o'rniga `requests==2.32.3`.

Paketlar menejeri vazifalaridan biri qaramliklar --- va tranzitiv qaramliklar --- tomonidan taqdim etilgan barcha cheklovlarni hisobga olish va keyin barcha cheklovlarni qanoatlantiradigan yaroqli versiyalar ro'yxatini chiqarishdir.
Keyin qayta tiklanuvchanlik maqsadida aniq versiyalar ro'yxatini faylga saqlash mumkin; bunday fayllar _lock fayllari_ deb ataladi.

```console
$ uv lock
Resolved 12 packages in 45ms

$ cat uv.lock | head -20
version = 1
requires-python = ">=3.11"

[[package]]
name = "certifi"
version = "2024.8.30"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/...", hash = "sha256:..." }
wheels = [
    { url = "https://files.pythonhosted.org/...", hash = "sha256:..." },
]
...
```

Qaramliklarni versiyalash va qayta tiklanuvchanlik bilan ishlashdagi asosiy farq kutubxonalar va ilovalar/xizmatlar o'rtasidagi tafovutdir.
Kutubxona import qilinishga va o'zining qaramliklariga ega bo'lishi mumkin bo'lgan boshqa kodlar tomonidan ishlatilishiga mo'ljallangan, shuning uchun juda qat'iy versiya cheklovlarini ko'rsatish foydalanuvchining boshqa qaramliklari bilan ziddiyatlarni keltirib chiqarishi mumkin.
Bunga qarama-qarshi o'laroq, ilovalar yoki xizmatlar dasturiy ta'minotning oxirgi iste'molchilari hisoblanadi va odatda o'z funksiyalarini dasturlash interfeysi orqali emas, balki foydalanuvchi interfeysi yoki API orqali namoyish etadi.
Kutubxonalar uchun kengroq paket ekotizimi bilan moslikni maksimal darajada oshirish uchun versiya diapazonlarini belgilash yaxshi amaliyotdir. Ilovalar uchun aniq versiyalarni mixlash (pinning) qayta tiklanuvchanlikni kafolatlaydi --- dasturni ishga tushirgan har bir kishi aynan bir xil qaramliklardan foydalanadi.


Maksimal qayta tiklanuvchanlikni talab qiladigan loyihalar uchun [Nix](https://nixos.org/) va [Bazel](https://bazel.build/) kabi vositalar _germetik_ (hermetic) qurishlarni taqdim etadi --- bu yerda har bir kirish qiymati (input), shu jumladan kompilyatorlar, tizim kutubxonalari va hatto qurish muhitining o'zi ham mixlanadi va mazmuni bo'yicha manzilga yo'naltiriladi (content-addressed). Bu qurish qachon yoki qayerda ishlashidan qat'i nazar, bitma-bit o'xshash natijalarni kafolatlaydi.

> Siz hatto kompyuteringiz sozlamalarining to'liq yangi nusxalarini osongina ishga tushirish va ularni versiyalar nazoratidagi konfiguratsiya fayllari orqali boshqarish uchun butun kompyuteringiz o'rnatilishini boshqarish uchun NixOS dan foydalanishingiz mumkin.

Dasturiy ta'minotni ishlab chiqishdagi cheksiz ziddiyatlardan biri shundaki, yangi dasturiy ta'minot versiyalari kutilgan yoki kutilmaganda buzilishlarni keltirib chiqaradi, boshqa tomondan esa, eski dasturiy ta'minot versiyalari vaqt o'tishi bilan xavfsizlik zaifliklari bilan xavf ostida qoladi.
Buni bizning ilovamizni yangi dasturiy ta'minot versiyalariga nisbatan testlaydigan va yangi qaramlik versiyalari chiqarilganida avtomatlashtirishga ega bo'lgan uzluksiz integratsiya (continuous integration) qatorlari (pipeline) yordamida hal qilishimiz mumkin (buni [Kod sifati va CI](/2026/code-quality/) darsida ko'ramiz), masalan [Dependabot](https://github.com/dependabot) kabi.

CI testlari mavjud bo'lsa ham, dasturiy ta'minot versiyalarini yangilashda muammolar baribir yuzaga keladi, ko'pincha bu dev va prod muhitlari o'rtasidagi muqarrar mos kelmaslik sababli yuz beradi.
Bunday hollarda eng yaxshi yo'l _orqaga qaytarish_ (rollback) rejasiga ega bo'lishdir, bu yerda versiyani yangilash bekor qilinadi va o'rniga ma'lum bo'lgan yaxshi versiya qayta yoyiladi.

# Virtual mashinalar va konteynerlar

Siz murakkabroq qaramliklarga suyana boshlaganingiz sayin, kodingizning qaramliklari paketlar menejeri eplay oladigan chegaralardan chiqib ketishi ehtimoli bor.
Bunga keng tarqalgan sabablardan biri ma'lum bir tizim kutubxonalari yoki apparatura drayverlari bilan o'zaro ishlashga to'g'ri kelishidir.
Masalan, ilmiy hisoblash va sun'iy intellekt sohalarida GPU uskunalaridan foydalanish uchun dasturlar ko'pincha maxsus kutubxonalar va drayverlarga muhtoj bo'ladi.
Ko'pgina tizim darajasidagi qaramliklar (GPU drayverlari, maxsus kompilyator versiyalari, OpenSSL kabi umumiy kutubxonalar) hali ham butun tizim bo'ylab o'rnatishni talab qiladi.

An'anaviy ravishda bu kengroq qaramlik muammosi Virtual mashinalar (VM) bilan hal qilingan.
Virtual mashinalar butun kompyuterni abstraksiya qiladi va o'zining bag'ishlangan operatsion tizimiga ega bo'lgan mutlaqo izolyatsiyalangan muhitni ta'minlaydi.
Zamonaviyroq yondashuv bu konteynerlardir, ular ilovani o'z qaramliklari, kutubxonalari va fayl tizimi bilan birgalikda paketlaydi, biroq butun kompyuterni virtuallashtirish o'rniga hostning (mezbonning) operatsion tizimi kerneli bilan bo'lishadi.
Konteynerlar VMlarga qaraganda yengilroq hisoblanadi, chunki ular kernelni bo'lishadilar, bu esa ularni tezroq ishga tushirishga va ishlash jihatdan samaraliroq bo'lishga olib keladi.

Eng ommabop konteyner platformasi [Docker](https://www.docker.com/) hisoblanadi. Docker konteynerlarni qurish, tarqatish va ishga tushirishning standartlashtirilgan usulini joriy qildi. Ichki qismida Docker konteyner runtime sifatida containerd ni ishlatadi --- bu Kubernetes kabi boshqa vositalar ham ishlatadigan sanoat standartidir.

Konteynerni ishga tushirish juda oson. Masalan, Python interpretatorini konteyner ichida ishga tushirish uchun biz `docker run` dan foydalanamiz ( `-it` bayroqlari konteynerni terminal orqali interaktiv qiladi. Siz chiqqaningizda konteyner to'xtaydi.).

```console
$ docker run -it python:3.12 python
Python 3.12.7 (main, Nov  5 2024, 02:53:25) [GCC 12.2.0] on linux
>>> print("Hello from inside a container!")
Hello from inside a container!
```

Amalda dasturingiz butun fayl tizimiga bog'liq bo'lishi mumkin. Buni yengib o'tish uchun biz butun ilova fayl tizimini artefakt sifatida yetkazib beradigan konteyner tasvirlaridan (image) foydalanishimiz mumkin. Konteyner tasvirlari dasturiy ravishda yaratiladi. Docker yordamida biz Dockerfile sintaksisi orqali tasvirning qaramliklarini, tizim kutubxonalarini va konfiguratsiyasini aniq ko'rsatamiz:

```dockerfile
FROM python:3.12
RUN apt-get update
RUN apt-get install -y gcc
RUN apt-get install -y libpq-dev
RUN pip install numpy
RUN pip install pandas
COPY . /app
WORKDIR /app
RUN pip install .
```

Muhim farq: Docker **tasviri** paketlangan artefakt (shablonga o'xshash) bo'lsa, **konteyner** shu tasvirning ishga tushgan instansiyasi hisoblanadi. Siz bitta tasvirdan bir nechta konteynerlarni ishga tushirishingiz mumkin. Tasvirlar qatlamlarda quriladi, bu yerda Dockerfile'dagi har bir ko'rsatma (`FROM`, `RUN`, `COPY` va h.k.) yangi qatlamni yaratadi. Docker bu qatlamlarni keshlaydi, shuning uchun Dockerfile dagi bir qatorni o'zgartirsangiz, faqat o'sha qatlam va undan keyingi qatlamlar qayta qurilishi kerak bo'ladi.

Oldingi Dockerfile da bir nechta muammolar bor: u qisqartirilgan variant o'rniga to'liq Python tasviridan foydalanadi, alohida `RUN` buyruqlarini ishga tushirib keraksiz qatlamlarni yaratadi, versiyalar mixlanmagan, va u paket menejeri keshlarini tozalamaydi hamda keraksiz fayllarni qo'shib yuboradi. Boshqa keng tarqalgan xatolarga konteynerlarni xavfsiz bo'lmagan tarzda root sifatida ishlatish va qatlamlarda sirlarni adashib kiritib yuborish kabilar kiradi.

Quyida yaxshilangan versiyasi keltirilgan

```dockerfile
FROM python:3.12-slim
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc libpq-dev && \
    rm -rf /var/lib/apt/lists/*
COPY pyproject.toml uv.lock ./
RUN uv pip install --system -r uv.lock
COPY . /app
```

Oldingi misolda biz `uv`ni manbadan o'rnatish o'rniga, uni `ghcr.io/astral-sh/uv:latest` tasviridagi oldindan qurilgan binardan nusxalayotganimizni ko'ramiz. Bu _builder_ namunasi (pattern) sifatida tanilgan. Bu namuna bilan kodimizni kompilyatsiya qilish uchun zarur bo'lgan barcha vositalarni emas, faqat ilovani ishga tushirish uchun kerak bo'lgan yakuniy binarni (`uv` ushbu holatda) yuklashimiz kifoya.

Dockerning yodda saqlash kerak bo'lgan muhim cheklovlari bor. Birinchidan, konteyner tasvirlari ko'pincha platformaga mos ravishda tuziladi --- `linux/amd64` uchun qurilgan tasvir emulsiyasiz sekin bo'ladigan `linux/arm64` da (Apple Silicon Mac'larida) o'z holicha ishlamaydi. Ikkinchidan, Docker konteynerlari Linux kernelini talab qiladi, shuning uchun macOS va Windows da Docker aslidahuddi ostida yengil Linux VM ni ishga tushiradi va bu qo'shimcha yuklama (overhead) ni qo'shadi. Uchinchidan, Dockerning izolyatsiyasi VM larga qaraganda zaifroq --- konteynerlar host kernelni bo'lishadi va bu ko'p foydalanuvchili muhitlarda xavfsizlik muammosini keltirib chiqarishi mumkin.

> Hozirgi kunda, hatto "tizim bo'ylab" ishlaydigan kutubxonalar va dasturlarni har bir loyiha uchun alohida boshqarishda [nix flakes](https://serokell.io/blog/practical-nix-flakes) orqali nix'dan foydalanuvchi loyihalar ko'paymoqda.

# Konfiguratsiya

Dasturiy ta'minot o'z-o'zidan sozlanuvchandir. [Buyruqlar satri muhiti](/2026/command-line-environment/) darsida biz bayroqlar (flaglar), muhit o'zgaruvchilari yoki hatto konfiguratsiya fayllari (nuqtali fayllar) orqali parametrlar qabul qiladigan dasturlarni ko'rdik. Bu hatto murakkabroq ilovalar uchun ham amal qiladi va masshtabda konfiguratsiyani boshqarishning belgilangan qonuniyatlari mavjud.
Dasturiy ta'minot konfiguratsiyasi kodga kiritilmasligi (hardcode qilinmasligi), balki ishlash vaqtida (runtime'da) taqdim etilishi kerak.
Shulardan ikkitasi umumiy bo'lib: muhit o'zgaruvchilari va konfiguratsiya fayllaridir.

Muhit o'zgaruvchilari orqali sozlanadigan ilovaga misol:

```python
import os

DATABASE_URL = os.environ.get("DATABASE_URL", "sqlite:///local.db")
DEBUG = os.environ.get("DEBUG", "false").lower() == "true"
API_KEY = os.environ["API_KEY"]  # Required - will raise if not set
```

Ilovani shuningdek, konfiguratsiya fayli orqali sozlash mumkin (masalan, `yaml.load` orqali config yuklaydigan Python dasturi), `config.yaml`:

```yaml
database:
  url: "postgresql://localhost/myapp"
  pool_size: 5
server:
  host: "0.0.0.0"
  port: 8080
  debug: false
```

Konfiguratsiya haqida o'ylash uchun yaxshi o'ng qo'l qoidasi shundaki, bitta kod bazasi turli xil muhitlarga (ishlab chiqish, test, ishlab chiqarish) hech qanday kod o'zgartirishlarisiz, faqat konfiguratsiyani o'zgartirish orqali yoyilishi (deploy qilinishi) kerak.

Ko'pgina konfiguratsiya imkoniyatlari orasida API kalitlari kabi nozik ma'lumotlar (sirlar) ham bo'ladi.
Sirlarning adashib fosh qilinishiga yo'l qo'ymaslik uchun ularga ehtiyotkorlik bilan yondashish va ularni versiyalarni boshqarish tizimiga (version control) qo'shmaslik kerak.


# Xizmatlar va orkestratsiya

Zamonaviy ilovalar kamdan-kam hollarda yakkalanib qoladi. Odatiy veb ilova ma'lumotlarni doimiy saqlash uchun ma'lumotlar bazasiga, ishlash tezligi uchun keshga, fondagi vazifalar uchun xabar navbatiga va turli boshqa yordamchi xizmatlarga muhtoj bo'lishi mumkin. Hamma narsani bitta yaxlit (monolit) ilovaga yig'ish o'rniga, zamonaviy arxitekturalar funksionallikni mustaqil ravishda ishlab chiqilishi, joylashtirilishi va kengaytirilishi mumkin bo'lgan alohida xizmatlarga ajratadi.

Misol tariqasida, ilovamiz keshdan foydalanishi kerak bo'lsa, o'zimiznikini yaratish o'rniga [Redis](https://redis.io/) yoki [Memcached](https://memcached.org/) kabi sinalgan yechimlardan foydalanishimiz mumkin.
Biz Redisni ilovamiz qaramliklariga konteynerning bir qismi sifatida kiritishimiz mumkin, lekin bu Redis va ilovamiz o'rtasidagi barcha qaramliklarni uyg'unlashtirishni anglatadi, bu esa qiyin yoki hatto imkonsiz bo'lishi mumkin.
Buning o'rniga biz har bir ilovani o'z konteynerida alohida yoyishimiz mumkin.
Bu odatda mikroservis arxitekturasi deb ataladi, bu yerda har bir komponent tarmoq orqali, odatda HTTP API lar orqali bog'lanadigan mustaqil xizmat sifatida ishlaydi.

[Docker Compose](https://docs.docker.com/compose/) - ko'p konteynerli ilovalarni aniqlash va ishga tushirish uchun vosita. Konteynerlarni yakka o'zi boshqarish o'rniga, siz barcha xizmatlarni bitta YAML faylida e'lon qilasiz va ularni birgalikda boshqarasiz. Endi bizning to'liq ilovamiz bittadan ortiq konteynerlarni o'z ichiga oladi:

```yaml
# docker-compose.yml
services:
  web:
    build: .
    ports:
      - "8080:8080"
    environment:
      - REDIS_URL=redis://cache:6379
    depends_on:
      - cache

  cache:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

`docker compose up` buyrug'i yordamida ikkala xizmat ham birgalikda ishga tushadi va veb ilova `cache` host nomidan foydalangan holda Redis'ga ulanishi mumkin (Dockerning ichki DNS'i xizmat nomlarini avtomatik ravishda hal qiladi).
Docker Compose bizga bir yoki bir nechta xizmatni qanday yoyish (deploy) qilmoqchiligimizni e'lon qilish imkonini beradi va ularni birgalikda ishga tushirish orkestratsiyasi, ular o'rtasidagi tarmoqni o'rnatish va ma'lumotlarni doimiy saqlash uchun umumiy hajmlarni (volumes) boshqarishni o'z zimmasiga oladi.

Ishlab chiqarishga (production) yoyishlar uchun siz ko'pincha docker compose xizmatlaringiz boot vaqtida avtomatik ravishda ishga tushishini va xatolikda qayta ishga tushishini hohlaysiz. Keng tarqalgan yondashuv docker compose deploymentini boshqarish uchun systemd dan foydalanishdir:

```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My Application
Requires=docker.service
After=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/docker compose up -d
ExecStop=/usr/bin/docker compose down

[Install]
WantedBy=multi-user.target
```

Bu systemd unit fayli tizim yuklanganda (Docker tayyor bo'lgandan so'ng) ilovangiz ishga tushishini ta'minlaydi va `systemctl start myapp`, `systemctl stop myapp` hamda `systemctl status myapp` kabi standart boshqaruvlarni taqdim etadi.

Yoyish (deployment) talablari murakkablashib borayotgani sari --- bir nechta mashinalar miqyosida ko'paytirishga bo'lgan ehtiyoj, xizmatlar ishlamay qolganda xatolarga bardoshlilik (fault tolerance) va yuqori mavjudlik (high availability) kafolatlari --- tashkilotlar mashinalar klasterlarida minglab konteynerlarni boshqarishi mumkin bo'lgan Kubernetes (k8s) kabi murakkab konteyner orkestratsiya platformalariga murojaat qilishadi. Shu bilan birga, Kubernetes murakkab o'rganish jarayoniga va sezilarli operatsion qo'shimcha yuklamaga (overhead) ega, shuning uchun kichik loyihalar uchun bu ortiqcha hisoblanadi.

Ushbu ko'p konteynerli sozlama qisman mumkin bo'ldi, chunki zamonaviy xizmatlar bir-biri bilan standartlashtirilgan API lar, ya'ni HTTP REST API lar orqali aloqa qiladi. Masalan, dastur OpenAI yoki Anthropic kabi LLM provayderi bilan muloqot qilganda, aslidahuddi u o'z serverlariga HTTP so'rov yuboradi va javobni tahlil qiladi (parse qiladi):

```console
$ curl https://api.anthropic.com/v1/messages \
    -H "x-api-key: $ANTHROPIC_API_KEY" \
    -H "content-type: application/json" \
    -H "anthropic-version: 2023-06-01" \
    -d '{"model": "claude-sonnet-4-20250514", "max_tokens": 256,
         "messages": [{"role": "user", "content": "Explain containers vs VMs in one sentence."}]}'
```

# Nashr qilish

Kodingiz ishlashini isbotlaganingizdan so'ng, siz uni boshqalar yuklab olishi va o'rnatishi uchun tarqatishga (distribution) qiziqishingiz mumkin.
Tarqatish ko'p shakllarga ega bo'lib, siz ishlaydigan dasturlash tili va muhitlar bilan uzviy bog'liq.

Tarqatishning eng oddiy usuli - odamlar mahalliy kompyuterlariga (locally) yuklab olishlari va o'rnatishlari uchun artefaktlarni yuklash.
Bunga hozirgacha tez-tez duch kelamiz va siz buni asosan `.deb` fayllarining HTTP katalog ro'yxati bo'lgan [Ubuntu'ning paketlar arxivida](http://archive.ubuntu.com/ubuntu/pool/main/) ko'rishingiz mumkin.

Bugungi kunda GitHub ochiq manba kodlari va artefaktlarni nashr qilish (publishing) uchun amalda yagona standart (de facto) platformaga aylangan.
Garchi kod asosan hammaga ochiq bo'lsa-da, GitHub relizlari maintainer'larga oldindan tayyorlangan binarlar va boshqa artefaktlarni teglangan versiyalarga qo'shib qo'yish imkoniyatini taqdim etadi.


Paketlar menejerlari ba'zan to'g'ridan-to'g'ri GitHub'dan o'rnatishni (manbadan yoki oldindan qurilgan wheel'dan) qo'llab-quvvatlaydi:

```console
# Install from source (will clone and build)
$ pip install git+https://github.com/psf/requests.git

# Install from a specific tag/branch
$ pip install git+https://github.com/psf/requests.git@v2.32.3

# Install a wheel directly from a GitHub release
$ pip install https://github.com/user/repo/releases/download/v1.0/package-1.0-py3-none-any.whl
```

Aslida, Go kabi ba'zi tillar markazlashmagan (decentralized) tarqatish modelidan foydalanadi --- markaziy paket repozitoriysidan ko'ra, Go modullari to'g'ridan-to'g'ri manba kodi repozitoriylaridan tarqatiladi.
`github.com/gorilla/mux` kabi modul yo'llari kod qayerda saqlanishini ko'rsatadi va `go get` to'g'ridan-to'g'ri shu yerdan fetch qiladi. Biroq, `pip`, `cargo` yoki `brew` kabi aksariyat paketlar menejerlari qulay tarqatish va o'rnatish maqsadida avvaldan tayyorlangan paketlarning markaziy indekslariga ega. Agar biz ishga tushirsak

```console
$ uv pip install requests --verbose --no-cache 2>&1 | grep -F '.whl'
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl
```

biz `requests` wheel ni qayerdan fetch qilayotganimizni ko'ramiz. Fayl nomidagi `py3-none-any` ga e'tibor bering --- bu wheel har qanday Python 3 versiyasi, har qanday operatsion tizim (OS) va har qanday arxitektura bilan ishlashini anglatadi. Kompilyatsiya qilingan kodli paketlar uchun wheel platformaga xos bo'ladi:

```console
$ uv pip install numpy --verbose --no-cache 2>&1 | grep -F '.whl'
DEBUG Selecting: numpy==2.2.1 [compatible] (numpy-2.2.1-cp312-cp312-macosx_14_0_arm64.whl)
```

Bu yerda `cp312-cp312-macosx_14_0_arm64` bu wheel faqat ARM64 (Apple Silicon) arxitekturasi uchun macOS 14+ da CPython 3.12 gagina xos ekanligini ko'rsatadi. Agar siz boshqa platformada bo'lsangiz, `pip` boshqa wheel yuklab oladi yoki manbadan quradi.

Aksincha, biz yaratgan paketni boshqalar topishi uchun uni reyestrlaridan (registry) biriga joylashimiz zarur.
Python'da asosiy reyestr [Python Package Index (PyPI)](https://pypi.org) hisoblanadi.
O'rnatish singari, paketlarni nashr qilishning ham bir nechta usullari mavjud. PyPI ga paketlarni yuklash uchun zamonaviy usul `uv publish` buyrug'idir:

```console
$ uv publish --publish-url https://test.pypi.org/legacy/
Publishing greeting-0.1.0.tar.gz
Publishing greeting-0.1.0-py3-none-any.whl
```

Bu yerda biz haqiqiy PyPI reyestrini ifloslantirmasdan nashr qilish jarayonini testlash uchun mo'ljallangan alohida [TestPyPI](https://test.pypi.org) reyestridan foydalanmoqdamiz. Yuklaganingizdan so'ng uni TestPyPI orqali o'rnatish mumkin:

```console
$ uv pip install --index-url https://test.pypi.org/simple/ greeting
```

Dasturiy ta'minotni nashr qilishdagi asosiy e'tibor --- ishonch. Foydalanuvchilar o'zlari yuklab olgan paket haqiqatan ham sizga tegishli ekanligiga va o'zgartirilmaganligiga qanday ishonch hosil qilishadi? Paket reyestrlari paketning yaxlitligini tekshirish uchun cheksummalardan foydalanadi va ba'zi ekotizimlar mualliflikni kriptografik dalillashni ta'minlash uchun paketlarni imzolash imkoniyatini ham beradi.

Turli tillarning o'ziga xos paket reyestrlari mavjud: Rust uchun [crates.io](https://crates.io), JavaScript uchun [npm](https://www.npmjs.com), Ruby uchun [RubyGems](https://rubygems.org) va konteyner tasvirlari uchun [Docker Hub](https://hub.docker.com). Shu bilan birga, yopiq (shaxsiy) yoki ichki paketlar uchun tashkilotlar o'zlarining shaxsiy paket repozitoriylaridan foydalanishi (masalan, xususiy PyPI serveri yoki Docker reyestri) yoki bulut (cloud) provayderlarining boshqariladigan yechimlaridan foydalanishi mumkin.

Veb-xizmatni internetga yoyish (deploy qilish) qo'shimcha infratuzilmani talab qiladi: domen nomini ro'yxatdan o'tkazish, nomni serveringizga bog'lash uchun DNS konfiguratsiyasi va odatda HTTPS ulanishni qo'llab-quvvatlash hamda trafikni yo'naltirish uchun nginx kabi teskari proksini sozlash. Hujjatlar yoki statik saytlar kabi soddaroq vazifalar uchun, [GitHub Pages](https://pages.github.com/) kodingizning repozitoriysidan to'g'ridan-to'g'ri bepul hosting imkoniyatini beradi.

<!--
## Documentation

So far we have emphasized the deliverable _artifact_ as the main output of packaging and shipping code.
In addition to the artifact, we need to document for users the code's functionality, installation instructions, and usage examples.

Tools like [Sphinx](https://www.sphinx-doc.org/) (Python) and [MkDocs](https://www.mkdocs.org/) can automatically generate browsable documentation from docstrings and markdown files, often hosted on services like [Read the Docs](https://readthedocs.org/).
For HTTP-based APIs, the [OpenAPI specification](https://www.openapis.org/) (formerly Swagger) provides a standard format for describing API endpoints, which tools can use to generate interactive documentation and client libraries automatically. -->


# Mashqlar

1. O'z muhitingizni `printenv` buyrug'i bilan saqlang, bitta venv yarating va ishga tushiring, `printenv` buyrug'ini boshqa faylga yo'naltiring va `diff before.txt after.txt` ni ko'ring. Muhitda nima o'zgargan? Nimaga shell virtual muhitni (venv) ni ko'proq ma'qul ko'radi? (Maslahat: Aktivatsiya (ishga tushirish)dan oldin va keyin `$PATH` ga e'tibor qiling.) `which deactivate` buyrug'ini ishga tushiring va deactivate bash funksiyasi nima qilayotganini tahlil qiling.
1. `pyproject.toml` orqali bitta Python paketini yarating va uni virtual muhitda (venv) o'rnating. Bitta lock fayli yarating va uni tekshirib ko'ring.
1. Docker'ni o'rnating va undan Missing Semester saytini o'z kompyuteringizda (locally) docker compose orqali yurgizib ko'ring.
1. Sodda bitta Python ilovasi uchun Dockerfile yozing. So'ng Redis keshni o'z ilovangiz bilan ishga tushiradigan `docker-compose.yml` ni yozing.
1. TestPyPI ga bitta Python paketini nashr qiling (hech qanday qo'shimchasiz, oddiy loyiha bo'lsa uni haqiqiy PyPI ga yuklamang!). Keyin o'sha paketdan foydalanadigan Docker tasvirini (image) yarating va uni `ghcr.io` ga push qiling.
1. [GitHub Pages](https://docs.github.com/en/pages/quickstart) yordamida veb-sayt yarating. Qo'shimcha (non-credit): unga maxsus domenni (custom domain) moslashtiring.