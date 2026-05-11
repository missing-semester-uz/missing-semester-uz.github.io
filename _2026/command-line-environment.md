---
layout: lecture
title: "Buyruqlar satri muhiti"
description: >
  Buyruqlar satri qanday ishlashi, jumladan kirish/chiqish oqimlari, muhit o'zgaruvchilari va SSH orqali masofaviy mashinalar bilan ishlashni o'rganamiz.
thumbnail: /static/assets/thumbnails/2026/lec2.png
date: 2026-01-13
ready: true
video:
  aspect: 56.25
  id: ccBGsPedE9Q
---

Oldingi ma'ruzada ko'rib chiqqanimizdek, ko'pgina shell'lar boshqa dasturlarni ishga tushirish uchun oddiy ishga tushiruvchi (launcher) emas, balki amalda ular keng tarqalgan shablonlar va abstraksiyalarga boy bo'lgan to'liq dasturlash tilini taqdim etadi.
Biroq, ko'pgina dasturlash tillaridan farqli o'laroq, shell skriptlarida hamma dasturlarni ishga tushirish va ularni bir-biri bilan sodda va samarali aloqa qilishiga qaratilgan.

Xususan, shell skriptlari _konvensiyalar_ bilan chambarchas bog'liq. Buyruqlar satri interfeysi (CLI) dasturi kengroq shell muhitida yaxshi ishlashi uchun u rioya qilishi kerak bo'lgan ba'zi umumiy shablonlar mavjud.
Endi biz buyruqlar satri dasturlari qanday ishlashini tushunish uchun zarur bo'lgan ko'plab tushunchalarni, shuningdek, ulardan qanday foydalanish va sozlash bo'yicha keng tarqalgan konvensiyalarni ko'rib chiqamiz.

# Buyruqlar satri interfeysi

Aksariyat dasturlash tillarida funksiya yozish taxminan shunday ko'rinadi:

```
def add(x: int, y: int) -> int:
    return x + y
```

Bu yerda biz dasturning kirish va chiqish ma'lumotlarini aniq ko'rishimiz mumkin.
Bunga farqli o'laroq, shell skriptlari bir qarashda butunlay boshqacha ko'rinishi mumkin.

```shell
#!/usr/bin/env bash

if [[ -f $1 ]]; then
    echo "Target file already exists"
    exit 1
else
    if $DEBUG; then
        grep 'error' - | tee $1
    else
        grep 'error' - > $1
    fi
    exit 0
fi
```

Bunga o'xshash skriptlarda nima sodir bo'layotganini to'g'ri tushunish uchun avvalo shell dasturlari bir-biri bilan yoki shell muhiti bilan aloqa qilganda tez-tez uchraydigan bir nechta tushunchalarni kiritishimiz kerak:

- Argumentlar (Arguments)
- Oqimlar (Streams)
- Muhit o'zgaruvchilari (Environment variables)
- Qaytish kodlari (Return codes)
- Signallar (Signals)

## Argumentlar

Shell dasturlari ishga tushirilganda argumentlar ro'yxatini qabul qiladi.
Argumentlar shell'da oddiy satrlar (string) bo'lib, ularni qanday talqin qilish dasturning o'ziga bog'liq.
Masalan, biz `ls -l folder/` qilsak, biz `/bin/ls` dasturini `['-l', 'folder/']` argumentlari bilan ishga tushirayotgan bo'lamiz.

Shell skripti ichidan biz ularga maxsus shell sintaksisi orqali murojaat qilamiz.
Birinchi argumentga murojaat qilish uchun `$1` o'zgaruvchisidan, ikkinchisiga `$2` va hokazo to `$9` gacha murojaat qilamiz. Barcha argumentlarni ro'yxat sifatida olish uchun `$@` va argumentlar sonini olish uchun `$#` dan foydalanamiz. Bundan tashqari, dastur nomiga `$0` orqali murojaat qilishimiz mumkin.

Aksariyat dasturlar uchun argumentlar _bayroqlar_ (flags) va oddiy satrlar aralashmasidan iborat bo'ladi.
Bayroqlarni aniqlash mumkin, chunki ularning oldida chiziqcha (`-`) yoki qo'sh chiziqcha (`--`) bo'ladi.
Bayroqlar odatda ixtiyoriy bo'lib, ularning vazifasi dasturning ishlashini o'zgartirishdir.
Masalan, `ls -l` buyrug'i `ls` o'z chiqishini qanday formatlashini o'zgartiradi.

Siz `--all` kabi uzun nomli qo'sh chiziqchali bayroqlarni va ko'pincha bitta harf bilan keladigan `-a` kabi bitta chiziqchali bayroqlarni ko'rasiz.
Bir xil parametr har ikkala formatda ko'rsatilishi mumkin, `ls -a` va `ls --all` ekvivalentdir.
Bitta chiziqchali bayroqlar ko'pincha guruhlanadi, shuning uchun `ls -l -a` va `ls -la` ham ekvivalentdir.
Bayroqlarning ketma-ketligi ham odatda muhim emas, `ls -la` va `ls -al` bir xil natijani beradi.
Ba'zi bayroqlar juda keng tarqalgan va shell muhiti bilan ko'proq tanishganingiz sari siz intuitiv ravishda ularga murojaat qilasiz, masalan (`--help`, `--verbose`, `--version`).

> Bayroqlar shell konvensiyalariga yaxshi birinchi misoldir. Shell tili dasturimizdan ma'lum bir tarzda `-` yoki `--` dan foydalanishni talab qilmaydi.
Bizga `myprogram +myoption myfile` sintaksisiga ega dastur yozishga hech narsa to'sqinlik qilmaydi, lekin bu chalkashlikka olib kelishi mumkin, chunki chiziqchalardan foydalanish kutiladi.
> Amalda ko'pgina dasturlash tillari CLI bayroqlarini tahlil qilish (parsing) kutubxonalarini taqdim etadi (masalan, chiziqcha sintaksisi bilan argumentlarni tahlil qilish uchun python'da `argparse`).

CLI dasturlaridagi yana bir keng tarqalgan konvensiya - dasturlar bir xil turdagi o'zgaruvchan sondagi argumentlarni qabul qilishidir. Ushbu usulda argumentlar berilganda buyruq ularning har birida bir xil amaliyotni bajaradi.

```shell
mkdir src
mkdir docs
# is equivalent to
mkdir src docs
```

Bu sintaksis (syntax sugar) bir qarashda keraksiz bo'lib tuyulishi mumkin, ammo u _glob_ bilan birgalikda ishlatilganda haqiqatan ham kuchli bo'ladi.
Glob'lar - bu shell dasturni chaqirishdan oldin kengaytiradigan maxsus naqshlar (patterns).

Deylik, biz joriy katalogdagi barcha .py fayllarini rekursiv bo'lmagan tarzda o'chirmoqchimiz. Oldingi ma'ruzada o'rganganlarimizdan shuni quyidagicha ishga tushirish orqali amalga oshirishimiz mumkin edi:

```shell
for file in $(ls | grep -P '\.py$'); do
    rm "$file"
done
```

Lekin biz uni oddiygina `rm *.py` bilan almashtirishimiz mumkin!

Terminalga `rm *.py` deb yozganimizda, shell `/bin/rm` dasturini `['*.py']` argumenti bilan chaqirmaydi.
Buning o'rniga, shell joriy katalogda `*.py` namunasiga mos keladigan fayllarni qidiradi, bu yerda `*` istalgan turdagi noldan yoki undan ko'p belgilarga ega satrga mos kelishi mumkin.
Shunday qilib, agar bizning katalogimizda `main.py` va `utils.py` bo'lsa, u holda `rm` dasturi `['main.py', 'utils.py']` argumentlarini qabul qiladi.

Siz uchratadigan eng keng tarqalgan glob'lar bu shablon belgilari (wildcards): `*` (nolinchi yoki istalgancha), `?` (aynan bitta istalgan belgi) va jingalak qavslardir.
Jingalak qavslar `{}` naqshlarning vergul bilan ajratilgan ro'yxatini bir nechta argumentlarga kengaytiradi.

Amalda glob'larni misollar yordamida yaxshiroq tushunish mumkin.

```shell
touch folder/{a,b,c}.py
# Will expand to
touch folder/a.py folder/b.py folder/c.py

convert image.{png,jpg}
# Will expand to
convert image.png image.jpg

cp /path/to/project/{setup,build,deploy}.sh /newpath
# Will expand to
cp /path/to/project/setup.sh /path/to/project/build.sh /path/to/project/deploy.sh /newpath

# Globbing techniques can also be combined
mv *{.py,.sh} folder
# Will move all *.py and *.sh files
```

> Ba'zi shell'lar (masalan, zsh) yanada rivojlangan glob turlarini qo'llab-quvvatlaydi, masalan `**`, bu rekursiv yo'llarni kiritish uchun kengaytiriladi. Ya'ni `rm **/*.py` barcha .py fayllarini rekursiv ravishda o'chirib tashlaydi.

## Oqimlar

Qachonki biz quyidagi kabi pipeline'ni ishga tushirsak

```shell
cat myfile | grep -P '\d+' | uniq -c
```

biz `grep` dasturi `cat` va `uniq` dasturlari bilan aloqa qilayotganini ko'ramiz.

Bu yerdagi muhim jihat shundaki, barcha uchala dastur bir vaqtning o'zida ishlamoqda.
Boshqacha aytganda, shell avval cat'ni, keyin grep'ni va keyin uniq'ni chaqirmayapti.
Buning o'rniga, uchala dastur ham yaratiladi va shell cat chiqishini grep kirishiga, grep chiqishini esa uniq kirishiga ulaydi.
`|` operatoridan foydalanganda, shell zanjirdagi bitta dasturdan keyingisiga o'tadigan ma'lumotlar oqimlarida ishlaydi.

Biz ushbu parallellikni ko'rsatishimiz mumkin, pipeline'dagi barcha buyruqlar darhol ishga tushadi:

```console
$ (sleep 15 && cat numbers.txt) | grep -P '^\d$' | sort | uniq  &
[1] 12345
$ ps | grep -P '(sleep|cat|grep|sort|uniq)'
  32930 pts/1    00:00:00 sleep
  32931 pts/1    00:00:00 grep
  32932 pts/1    00:00:00 sort
  32933 pts/1    00:00:00 uniq
  32948 pts/1    00:00:00 grep
```

Biz ko'rib turibmizki, `cat`'dan tashqari barcha jarayonlar darhol ishlamoqda. Shell barcha jarayonlarni yaratadi va ularning birortasi tugashidan oldin ularning oqimlarini ulaydi. `cat` faqat sleep tugagandan so'ng ishga tushadi va `cat` natijasi grep'ga va hokazo uzatiladi.

Har bir dasturning standart kirish deb ataladigan bitta kirish oqimi mavjud (stdin). Pipeline qilinganida, stdin avtomatik tarzda ulanadi. Skript ichida ko'plab dasturlar fayl nomi sifatida `-` belgisini qabul qiladi, bu "stdin'dan o'qish" ni anglatadi:

```shell
# These are equivalent when data comes from a pipe
echo "hello" | grep "hello"
echo "hello" | grep "hello" -
```

Xuddi shunday, har bir dasturda ikkita chiqish oqimi mavjud: stdout va stderr.
Standart chiqish (stdout) eng ko'p uchraydigan hisoblanadi va aynan u dastur chiqishini pipeline'dagi keyingi buyruqqa yo'naltirish (pipe) uchun ishlatiladi.
Standart xato (stderr) qo'shimcha oqim bo'lib, u chiqish zanjiridagi keyingi buyruq tomonidan tahlil qilinmasdan turib dasturlar ogohlantirishlar va boshqa turdagi muammolar haqida xabar berishlari uchun mo'ljallangan.

```console
$ ls /nonexistent
ls: cannot access '/nonexistent': No such file or directory
$ ls /nonexistent | grep "pattern"
ls: cannot access '/nonexistent': No such file or directory
# The error message still appears because stderr is not piped
$ ls /nonexistent 2>/dev/null
# No output - stderr was redirected to /dev/null
```

Shell ushbu oqimlarni qayta yo'naltirish (redirect) uchun sintaksisni taqdim etadi. Buni ko'rsatuvchi ba'zi misollar:

```shell
# Redirect stdout to a file (overwrite)
echo "hello" > output.txt

# Redirect stdout to a file (append)
echo "world" >> output.txt

# Redirect stderr to a file
ls foobar 2> errors.txt

# Redirect both stdout and stderr to the same file
ls foobar &> all_output.txt

# Redirect stdin from a file
grep "pattern" < input.txt

# Discard output by redirecting to /dev/null
cmd > /dev/null 2>&1
```

Unix falsafasini o'zida mujassam etgan yana bir kuchli vosita - bu [`fzf`](https://github.com/junegunn/fzf), loyqa qidiruv vositasi. U stdin'dan qatorlarni o'qiydi va filtrlash va tanlash uchun interaktiv interfeysni taqdim etadi:

```console
$ ls | fzf
$ cat ~/.bash_history | fzf
```

`fzf` ni ko'plab shell amallari bilan birlashtirish mumkin. Shell'ni sozlashni muhokama qilganda biz uning ko'proq ishlatilishini ko'ramiz.

## Muhit o'zgaruvchilari

Bash'da o'zgaruvchilarga qiymat berish uchun `foo=bar` sintaksisidan foydalanamiz, so'ngra `$foo` sintaksisi bilan o'zgaruvchining qiymatiga murojaat qilamiz.
E'tibor bering, `foo = bar` yaroqsiz sintaksis hisoblanadi, chunki shell uni `foo` dasturini `['=', 'bar']` argumentlari bilan chaqirish deb qabul qiladi.
Shell skriptida bo'sh joy belgisining roli argumentlarni ajratishdir.
Bunday xatti-harakatlar chalkash bo'lishi va unga o'rganish qiyin bo'lishi mumkin, shuning uchun buni yodda tuting.

Shell o'zgaruvchilarining tiplari yo'q, ularning barchasi satrlardir.
E'tibor bering, shell'da satrli ifodalarni yozishda bitta tirnoq va qo'shtirnoq o'zaro almashinuvchan emas.
`'` bilan ajratilgan satrlar literal satrlar bo'lib, o'zgaruvchilarni kengaytirmaydi, buyruq almashtirishni (command substitution) amalga oshirmaydi yoki maxsus ketma-ketliklarni qayta ishlamaydi, ammo `"` bilan ajratilgan satrlar esa bularni bajaradi.

```shell
foo=bar
echo "$foo"
# prints bar
echo '$foo'
# prints $foo
```

Buyruqning chiqishini o'zgaruvchiga saqlash uchun biz _buyruq almashtirish_ (command substitution) ni ishlatamiz.
Qachonki biz

```shell
files=$(ls)
echo "$files" | grep README
echo "$files" | grep ".py"
```

ni ishga tushirsak, ls'ning chiqishi (aniqrog'i stdout) keyinchalik kirishimiz mumkin bo'lgan `$files` o'zgaruvchisiga saqlanadi.
`$files` o'zgaruvchisining tarkibi ls chiqishidagi yangi qator belgilari (newlines) ni o'z ichiga oladi, shuning uchun `grep` kabi dasturlar har bir element bilan alohida ishlashni biladi.

Yana bir kamroq ma'lum bo'lgan o'xshash xususiyat bu _jarayonni almashtirish_ (process substitution). `<( CMD )` `CMD` ni ishga tushiradi va chiqishni vaqtinchalik faylga qo'yadi va `<()` ni ushbu faylning nomi bilan almashtiradi.
Bu buyruqlar ma'lumotlarni STDIN orqali emas, fayl orqali o'tkazishni kutganda foydali bo'ladi.
Masalan, `diff <(ls src) <(ls docs)` buyrug'i `src` va `docs` kataloglaridagi fayllar o'rtasidagi farqlarni ko'rsatadi.

Qachonki shell dasturi boshqa dasturni chaqirganda u tez-tez _muhit o'zgaruvchilari_ deb ataladigan o'zgaruvchilar to'plamini uzatadi.
Shell ichidan biz `printenv` buyrug'ini ishga tushirish orqali joriy muhit o'zgaruvchilarini topishimiz mumkin.
Muhit o'zgaruvchisini to'g'ridan-to'g'ri berish uchun buyruqdan oldin o'zgaruvchini tayinlashimiz mumkin:

> Muhit o'zgaruvchilari odatda BARCHA_BOSH_HARFLAR (masalan, `HOME`, `PATH`, `DEBUG`) da yoziladi. Bu texnik jihatdan talab qilinmasa ham, ko'pincha kichik harflarda bo'ladigan lokal shell o'zgaruvchilaridan muhit o'zgaruvchilarini ajratishga yordam beruvchi konvensiya hisoblanadi.

```shell
TZ=Asia/Tokyo date  # prints the current time in Tokyo
echo $TZ  # this will be empty, since TZ was only set for the child command
```

Shu bilan bir qatorda, biz joriy muhitni o'zgartiruvchi, o'rnatilgan `export` funksiyasidan foydalanishimiz mumkin va shunday qilib barcha bola jarayonlar o'zgaruvchini meros qilib oladi:

```shell
export DEBUG=1
# All programs from this point onwards will have DEBUG=1 in their environment
bash -c 'echo $DEBUG'
# prints 1
```

O'zgaruvchini o'chirish uchun `unset` buyrug'idan foydalaning, masalan `unset DEBUG`.

> Muhit o'zgaruvchilari shell konvensiyasining yana bir namunasidir. Ular ko'plab dasturlarning ishlashini to'g'ridan-to'g'ri emas, bilvosita o'zgartirish uchun ishlatilishi mumkin. Masalan, shell joriy foydalanuvchi uy katalogining yo'li bilan `$HOME` muhit o'zgaruvchisini o'rnatadi. Shundan so'ng dasturlar ushbu ma'lumotni olish uchun `—home /home/alice` kabi aniq bayroq talab qilmasdan, ushbu o'zgaruvchiga ulanishi mumkin. Yana bir keng tarqalgan misol bu `$TZ`, ko'pgina dasturlar belgilangan vaqt mintaqasiga ko'ra sana va vaqtlarni formatlash uchun bundan foydalanadilar.

## Qaytish kodlari

Oldin ko'rib o'tganimizdek, shell dasturining asosiy natijasi stdout/stderr oqimlari va fayl tizimi ta'sirlari orqali yetkaziladi.

Odatiy holda shell skripti nolinchi chiqish kodini qaytaradi.
Konvensiyaga ko'ra, nol hamma narsa yaxshi o'tganligini bildiradi, noldan farqli qiymat esa qandaydir muammolarga duch kelganini anglatadi.
Noldan farqli chiqish kodini qaytarish uchun biz `exit NUM` shell buyrug'idan foydalanishimiz kerak.
Biz `$?` maxsus o'zgaruvchisiga kirish orqali oxirgi bajarilgan buyruqning qaytish kodiga kirishimiz mumkin.

Shell'da mos ravishda mantiqiy AND va OR amallarini bajarish uchun `&&` va `||` mantiqiy operatorlari mavjud.
Oddiy dasturlash tillarida uchraydigan operatorlardan farqli o'laroq, shell'dagi operatorlar dasturlarning qaytish kodlariga asoslanadi.
Bularning har ikkalasi [qisqa tutashuvchi](https://en.wikipedia.org/wiki/Short-circuit_evaluation) (short-circuiting) operatorlardir.
Bu shuni anglatadiki, ular avvalgi buyruqlarning muvaffaqiyati yoki muvaffaqiyatsizligiga asoslanib buyruqlarni shartli ravishda ishga tushirish uchun ishlatilishi mumkin, bunda muvaffaqiyat qaytish kodining nol yoki nol emasligi bilan belgilanadi. Ba'zi misollar:

```shell
# echo will only run if grep succeeds (finds a match)
grep -q "pattern" file.txt && echo "Pattern found"

# echo will only run if grep fails (no match)
grep -q "pattern" file.txt || echo "Pattern not found"

# true is a shell program that always succeeds
true && echo "This will always print"

# and false is a shell program that always fails
false || echo "This will always print"
```

Xuddi shu tamoyil `if` va `while` operatorlariga ham tegishli bo'lib, ularning ikkalasi ham qaror qabul qilish uchun qaytish kodlaridan foydalanadi:

```shell
# if uses the return code of the condition command (0 = true, nonzero = false)
if grep -q "pattern" file.txt; then
    echo "Found"
fi

# while loops continue as long as the command returns 0
while read line; do
    echo "$line"
done < file.txt
```

## Signallar

Ba'zi hollarda siz dasturni ishlayotganda to'xtatishingiz kerak bo'ladi, masalan, buyruqning bajarilishi juda uzoq vaqt talab qilsa.
Dasturni to'xtatishning eng oson usuli bu `Ctrl-C` tugmalarini bosishdir va buyruq ehtimol to'xtaydi.
Lekin bu aslida qanday ishlaydi va nima uchun ba'zida jarayonni to'xtata olmaydi?

```console
$ sleep 100
^C
$
```

> Esda tuting, bu yerda `^C` terminalda yozilganda `Ctrl-C` qanday ko'rsatilishini bildiradi.

Ichki tomondan bu yerda nima yuz bergani:

1. Biz `Ctrl-C` ni bosdik
2. Shell belgilarnning maxsus kombinatsiyasini aniqladi
3. Shell jarayoni `sleep` jarayoniga SIGINT signalini yubordi
4. Signal `sleep` jarayonining bajarilishini to'xtatdi

Signallar maxsus aloqa mexanizmi hisoblanadi.
Jarayon signalni qabul qilganda, u o'z ishini to'xtatadi, signal bilan ishlaydi va potensial ravishda signal bergan ma'lumot asosida bajarilish oqimini o'zgartiradi. Shuning uchun signallar _dasturiy to'xtatilishlardir_ (software interrupts).

Bizning holatda, `Ctrl-C` ni terish shell'ga jarayonga `SIGINT` signalini yuborish so'rovini anglatadi.
Quyida `SIGINT` ni qamrab oladigan va e'tiborsiz qoldiradigan, endi to'xtamaydigan Python dasturining minimal namunasi keltirilgan. Ushbu dasturni yo'q qilish uchun endi `Ctrl-\` tugmachasini yozib `SIGQUIT` signalidan foydalanishimiz mumkin.

```python
#!/usr/bin/env python
import signal, time

def handler(signum, time):
    print("\nI got a SIGINT, but I am not stopping")

signal.signal(signal.SIGINT, handler)
i = 0
while True:
    time.sleep(.1)
    print("\r{}".format(i), end="")
    i += 1
```

Bu yerda agar biz ushbu dasturga ikki marta `SIGINT`, so'ngra `SIGQUIT` yuborsak nima bo'ladi. E'tibor bering, `^` belgisini terminalda yozishda `Ctrl` qanday ko'rsatilishi mumkin.

```console
$ python sigint.py
24^C
I got a SIGINT, but I am not stopping
26^C
I got a SIGINT, but I am not stopping
30^\[1]    39913 quit       python sigint.py
```

Garchand `SIGINT` va `SIGQUIT` ikkalasi ham odatda terminalga tegishli so'rovlar bilan bog'liq bo'lsa-da, jarayondan chiroyli tarzda chiqishni so'rash uchun umumiyroq signal - `SIGTERM` signali hisoblanadi.
Ushbu signalni yuborish uchun biz `kill -TERM <PID>` sintaksisiga ega bo'lgan [`kill`](https://www.man7.org/linux/man-pages/man1/kill.1.html) buyrug'idan foydalanishimiz mumkin.

Signallar jarayonni o'ldirishdan boshqa amallarni ham bajarishi mumkin. Masalan, `SIGSTOP` jarayonni to'xtatib turadi. Terminalda `Ctrl-Z` deb yozish shell'ni Terminal to'xtatilishi (ya'ni `SIGSTOP` ning terminal uchun versiyasi) ma'nosini bildiruvchi `SIGTSTP` signalini yuborishga majbur qiladi.

Siz to'xtatilgan vazifani mos ravishda [`fg`](https://www.man7.org/linux/man-pages/man1/fg.1p.html) yoki [`bg`](https://man7.org/linux/man-pages/man1/bg.1p.html) yordamida old (foreground) yoki orqa (background) fonda davom ettirishingiz mumkin.

[`jobs`](https://www.man7.org/linux/man-pages/man1/jobs.1p.html) buyrug'i joriy terminal seansi bilan bog'liq bo'lgan chala vazifalar (jobs) ro'yxatini chiqaradi.
Siz ularga pidi orqali ham murojaat qilishingiz mumkin (buni bilish uchun [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html) dan foydalansangiz bo'ladi).
Intuitivroq qilib aytganda, siz vazifaga uning `jobs` tomonidan ko'rsatiladigan vazifa raqamidan so'ng foiz belgisini qo'ygan holda ham murojaat qilishingiz mumkin. Orqa fon (background)ga o'tkazilgan oxirgi vazifaga murojaat qilish uchun siz `$!` maxsus parametridan foydalanishingiz mumkin.

Yana shuni bilish kerakki, buyruqdagi `&` suffiksi buyruqni orqa fonda ishlatib, sizga so'rov satrini qaytaradi, garchi u baribir shell'ning STDOUT idan foydalansa ham va bu zerikarli bo'lishi mumkin (bunday hollarda shell oqimini qayta yo'naltirish - redirections dan foydalaning). Ekvivalent tarzda, allaqachon ishlayotgan dasturni fonda ishlashga o'tkazish uchun `Ctrl-Z` ni, keyin esa `bg` ni yozishingiz mumkin.

E'tibor bering, orqa fonga (background) tushirilgan jarayonlar hali ham terminalingizning bola jarayonlari hisoblanadi va siz terminalni yopsangiz, ular o'lib qoladi (chunki bu yana boshqa bir `SIGHUP` signalini jo'natadi).
Buning oldini olish uchun siz dasturni [`nohup`](https://www.man7.org/linux/man-pages/man1/nohup.1.html) (SIGHUPni e'tiborsiz qoldirish uchun ishlatiladigan wrapper) orqali ishga tushirishingiz yoki jarayon allaqachon boshlangan bo'lsa `disown`dan foydalanishingiz mumkin.
Yoki quyidagi bo'limda ko'rib chiqadiganimizdek, terminal multipleksoridan (terminal multiplexer) foydalansangiz ham bo'ladi.

Quyida ushbu tushunchalarning ba'zilarini ko'rsatish uchun namunaviy seans keltirilgan.

```
$ sleep 1000
^Z
[1]  + 18653 suspended  sleep 1000

$ nohup sleep 2000 &
[2] 18745
appending output to nohup.out

$ jobs
[1]  + suspended  sleep 1000
[2]  - running    nohup sleep 2000

$ kill -SIGHUP %1
[1]  + 18653 hangup     sleep 1000

$ kill -SIGHUP %2   # nohup protects from SIGHUP

$ jobs
[2]  + running    nohup sleep 2000

$ kill %2
[2]  + 18745 terminated  nohup sleep 2000
```

`SIGKILL` maxsus signaldir, chunki jarayon uni qabul qila olmaydi va u doimo jarayonni darhol yakunlaydi. Lekin bu yetim (orphaned) bola jarayonlarni qoldirish kabi salbiy ta'sirlarga ega bo'lishi mumkin.

Siz bular haqida va boshqa signallar haqida ko'proq ma'lumotni [bu yerdan](https://en.wikipedia.org/wiki/Signal_(IPC)) yoxud [`man signal`](https://www.man7.org/linux/man-pages/man7/signal.7.html) yoki `kill -l` ni yozib bilishingiz mumkin.

Shell skriptlarida signallar olinganda buyruqlarni bajarish uchun o'rnatilgan `trap` dan foydalanishingiz mumkin. Bu tozalash amaliyotlari uchun qulay:

```shell
#!/usr/bin/env bash
cleanup() {
    echo "Cleaning up temporary files..."
    rm -f /tmp/mytemp.*
}
trap cleanup EXIT  # Run cleanup when script exits
trap cleanup SIGINT SIGTERM  # Also on Ctrl-C or kill
```

# Masovafiy mashinalar

Dasturchilar o'z ishlarida masofaviy serverlar bilan ishlashlari tobora kengayib bormoqda. Bunday paytda ishlash uchun eng ko'p qo'llaniladigan vosita SSH (Secure Shell) bo'lib, u bizga masofaviy serverga ulanishga yordam beradi va sizga oldindan ma'lum bo'lgan shell interfeysini taqdim etadi. Biz serverga quyidagi kabi buyruq yordamida ulanamiz:

```bash
ssh alice@server.mit.edu
```

Bu yerda biz `server.mit.edu` serverida `alice` foydalanuvchisi sifatida ssh orqali ulanishga harakat qilyapmiz.

`ssh` ning tez-tez ko'zdan qochiriladigan xususiyatlaridan biri shundaki, buyruqlarni interaktiv bo'lmagan tarzda ishga tushirish qobiliyatidir. `ssh` buyruqning stdin'ni yuborishni va stdout'ni olishni to'g'ri boshqaradi, shuning uchun biz uni boshqa buyruqlar bilan birlashtirishimiz mumkin:

```shell
# here ls runs in the remote, and wc runs locally
ssh alice@server ls | wc -l

# here both ls and wc run in the server
ssh alice@server 'ls | wc -l'

```

> [Mosh](https://mosh.org/) o'rnating, bu uzilishlarni hal qila oladigan, uxlash holatiga kirish / chiqish, tarmoqlarni o'zgartirish va yuqori kechikish tarmoqlari bilan shug'ullanishi mumkin bo'lgan SSH muqobilidir.

`ssh` bizga masofaviy serverda buyruqlarni bajarishga ruxsat berishi uchun biz bunga ruxsatimiz borligini isbotlashimiz kerak.
Biz buni parollar yoki ssh kalitlari orqali amalga oshirishimiz mumkin.
Kalitlarga asoslangan autentifikatsiya (Key-based authentication) mijozning maxfiy shaxsiy kalitini oshkor qilmasdan serverga ko'rsatishini isbotlash uchun ochiq kalitli kriptografiya (public-key cryptography) dan foydalanadi.
Kalitlarga asoslangan autentifikatsiya ham qulayroq, ham xavfsizroq bo'lganligi sababli uni afzal ko'rganingiz ma'qul.
E'tibor bering, shaxsiy kalit (ko'pincha `~/.ssh/id_rsa` va yaqin vaqtlardan beri `~/.ssh/id_ed25519`) samarali ravishda parolingiz vazifasini bajaradi, shuning uchun unga aynan shunday munosabatda bo'ling va uning tarkibini hech qachon baham ko'rmang.

Juftlik hosil qilish uchun siz [`ssh-keygen`](https://www.man7.org/linux/man-pages/man1/ssh-keygen.1.html) ni ishlatishingiz mumkin.
```bash
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/id_ed25519
```

Agar siz qachondir GitHub'ga push qilishni SSH kalitlari orqali sozlagan bo'lsangiz, demak siz [bu yerda](https://help.github.com/articles/connecting-to-github-with-ssh/) tasvirlangan bosqichlarni bajargansiz va sizda yaroqli kalitlar juftligi allaqachon mavjud. Parolingiz (passphrase) bor-yo'qligini tekshirish va uni tasdiqlash uchun `ssh-keygen -y -f /path/to/key` ni bajarishingiz mumkin.

Server tomonida `ssh` qaysi mijozlarga ruxsat berishini aniqlash uchun `.ssh/authorized_keys` ga qaraydi. Ochiq kalitdan nusxa olish uchun siz quyidagidan foydalanishingiz mumkin:

```bash
cat .ssh/id_ed25519.pub | ssh alice@remote 'cat >> ~/.ssh/authorized_keys'

# or more simply (if ssh-copy-id is available)

ssh-copy-id -i .ssh/id_ed25519 alice@remote
```

Buyruqlarni ishga tushirishdan tashqari, ssh o'rnatadigan aloqa orqali fayllarni serverga va u yerdan mijozga xavfsiz tarzda o'tkazish uchun foydalanilishi mumkin. [`scp`](https://www.man7.org/linux/man-pages/man1/scp.1.html) eng an'anaviy vosita bo'lib, uning sintaksisi `scp path/to/local_file remote_host:path/to/remote_file` ko'rinishida bo'ladi. [`rsync`](https://www.man7.org/linux/man-pages/man1/rsync.1.html) lokal va masofaviy qurilmalardagi aynan bir xil fayllarni aniqlab, ularni qayta ko'chirib o'tishni oldini olish orqali `scp` dan ko'ra ancha yaxshilanadi. Shuningdek u simvolik havolalar (symlink), ruxsatnomalar ustidan nozikroq boshqaruvni ta'minlaydi hamda ilgarigi to'xtatilgan ko'chirishni davom ettira oladigan `--partial` bayrog'i kabi qo'shimcha imkoniyatlarga ega. `rsync` `scp` bilan bir xil sintaksisga ega.

SSH mijozi sozlamalari `~/.ssh/config` da joylashgan bo'lib, u bizga xostlarni aniqlash hamda ular uchun odatiy sozlamalarni belgilash imkonini beradi. Bu konfiguratsiya faylini faqatgina `ssh` o'qimaydi, u `scp`, `rsync`, `mosh` va boshqa dasturlar tomonidan ham o'qiladi.

```bash
Host vm
    User alice
    HostName 172.16.174.141
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

# Configs can also take wildcards
Host *.mit.edu
    User alice
```

# Terminal Multiplexers

Buyruqlar satri interfeysidan foydalanganda ko'pincha bir vaqtning o'zida bir nechta amallarni bajarishni xohlaysiz.
Masalan, matn muharriri va o'z dasturingizni yonma-yon ishga tushirishni istashingiz mumkin.
Bunga yangi terminal oynalarini ochish orqali erishish mumkin bo'lsa-da, terminal multipleksoridan foydalanish ko'proq imkoniyatlarga ega yechimdir.

[`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html) kabi terminal multipleksorlari sizga terminal oynalarini panel va qatlamlardan foydalangan holda multiplekslash imkonini beradi, shunda siz bir nechta shell seanslari bilan samarali aloqada bo'lishingiz mumkin.
Bundan tashqari terminal multipleksorlari joriy terminal seansini uzish (detach) va unga boshqa vaqtda qaytadan ulanish imkonini beradi.
Shu sababli, masofaviy mashinalar bilan ishlaganda terminal multipleksorlari juda qulaydir, chunki bu `nohup` va shunga o'xshash hiyla-nayranglardan foydalanish zaruriyatidan qutqaradi.

Bugungi kunda eng ommabop terminal multipleksori bu [`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html) dir. `tmux` juda ham moslashuvchan bo'lib, tegishli klaviatura yorliqlari (keybindings) yordamida siz bir nechta oyna (tab) va panel (pane)larni yaratishingiz va ular orqali tezkor harakatlanishingiz mumkin.

`tmux` siz uning klaviatura qisqartmalarini bilishingizni kutadi va ularning barchasi `<C-b> x` shakliga ega bo'lib, bu (1) `Ctrl+b` ni bosish, (2) `Ctrl+b` ni qo'yib yuborish va (3) `x` ni bosishni bildiradi. `tmux` quyidagi ob'yektlar ierarxiyasiga ega:
- **Sessions** - seans bitta yoki undan ko'p oynaga ega mustaqil ish stoli hisoblanadi
    + `tmux` yangi seansni ishga tushiradi.
    + `tmux new -s NAME` uni belgilangan nom bilan ishga tushiradi.
    + `tmux ls` joriy seanslarni ko'rsatadi
    + `tmux` ichida `<C-b> d` deb terish joriy seansni uzadi (detaches)
    + `tmux a` oxirgi seansga ulaydi (attaches). Qaysi birini belgilash uchun `-t` bayrog'idan foydalanishingiz mumkin

- **Windows** - muharrirlar yoki brauzerlardagi oynalarga (tab) o'xshaydi, ular ayni seansning vizual tarzda ajratilgan qismlaridir
    + `<C-b> c` Yangi oyna yaratadi. Uni yopish uchun shell'larda `<C-d>` ni qilib ularni yakunlashingiz mumkin
    + `<C-b> N` _N_ inchi oynaga o'tish. E'tibor bering, ular raqamlangan bo'ladi
    + `<C-b> p` Oldingi oynaga o'tish
    + `<C-b> n` Keyingi oynaga o'tish
    + `<C-b> ,` Joriy oynaning nomini o'zgartirish
    + `<C-b> w` Joriy oynalar ro'yxatini chiqarish

- **Panes** - vim'dagi split'lar kabi, panellar bitta vizual ekranda bir nechta shell bo'lishiga imkon beradi.
    + `<C-b> "` Joriy panelni gorizontal ajratish
    + `<C-b> %` Joriy panelni vertikal ajratish
    + `<C-b> <direction>` Belgilangan _direction_ dagi panelga o'tish. Bu yerda yo'nalish yo'naltiruvchi klavishlarni anglatadi.
    + `<C-b> z` Joriy panelni masshtablashni (zoom) ochib-yopadi
    + `<C-b> [` Scrollback'ni boshlaydi. Keyin siz tanlashni boshlash uchun `<space>` ni va nusxa olish uchun `<enter>` ni bosishingiz mumkin.
    + `<C-b> <space>` Panel ko'rinishlari orasida navbatma-navbat aylanadi.

> tmux haqida ko'proq ma'lumot olish uchun [ushbu](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) qisqa qo'llanma va [bu](https://linuxcommand.org/lc3_adv_termmux.php) batafsilroq tavsifni o'qib chiqing.

tmux va SSH kabi vositalar yordamida har qanday mashinada o'z muhitingizni uydagidek his qilishni xohlaysiz. Bu erda shell'ni sozlash bo'limi keladi.

# Customizing the Shell

Ko'plab buyruqlar satri dasturlari odatda _nuqtali fayllar_ (dotfiles) deb nomlanuvchi oddiy matnli fayllar orqali sozlanadi
(chunki fayl nomlari `.` bilan boshlanadi, masalan `~/.vimrc`, shuning uchun u `ls` ko'rsatadigan kataloglar ro'yxatida yashirin bo'ladi).

> Nuqtali fayllar navbatdagi yana bir shell konvensiyasidir. Ularning oldidagi nuqta shunchaki (ro'yxatni chiqarishda) ularni "yashirish" uchun (ha, yana bir konvensiya).

Shell'lar bunday fayllar orqali sozlanadigan dasturlar uchun bitta misoldir. Shell ishga tushirilganda o'z sozlamalarini yuklash uchun ko'plab fayllarni o'qiydi.
Shell turingizga va login hamda interaktiv seans boshlash/boshlamasligingizga qarab bu jarayon ancha murakkablashib ketishi mumkin.
[Ushbu havola](https://web.archive.org/web/20260329133158/https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html) ushbu mavzu yuzasidan ajoyib resursdir.

`bash` uchun `~/.bashrc` yoki `~/.bash_profile` fayllarini o'zgartirish ko'pgina tizimlarda ishlaydi.
Nuqtali fayllar orqali sozlanishi mumkin bo'lgan vositalarga boshqa ba'zi misollar:

- `bash` - `~/.bashrc`, `~/.bash_profile`
- `git` - `~/.gitconfig`
- `vim` - `~/.vimrc` va `~/.vim` katalogi
- `ssh` - `~/.ssh/config`
- `tmux` - `~/.tmux.conf`

Shell'ga dasturlarni topishda yangi joylarni (paths) qo'shish keng tarqalgan konfiguratsiya amaliyoti hisoblanadi. Siz dastur o'rnatish davomida quyidagi kabi shablonni uchratishingiz mumkin:

```shell
export PATH="$PATH:path/to/append"
```

Bu yerda biz shell'ga PATH o'zgaruvchisini o'zining joriy qiymatiga yangi qatorni qo'shgan holda yangilash va barcha bola jarayonlarga shu yangi qiymatni meros qilib o'tishni aytmoqdamiz.
Bu orqali endi bola jarayonlar `path/to/append` orqali yo'llarga yozilgan dasturlarni izlab topa oladi.

Shell'ingizni sozlash ko'pincha yangi buyruqlar satri vositalarini o'rnatishni anglatadi. Paketlar menejeri (Package managers) buni osonlashtiradi. Ular dasturiy ta'minotni yuklab olish, o'rnatish va yangilashni boshqaradi. Turli xil operatsion tizimlarda turli xil paketlar menejeri mavjud: macOS'da [Homebrew](https://brew.sh/), Ubuntu/Debian'da `apt`, Fedora'da `dnf` va Arch'da `pacman` foydalaniladi. Biz kodni yetkazib berish (shipping code) ma'ruzasida paketlar menejerini batafsil ko'rib chiqamiz.

macOS'da Homebrew yordamida ikkita foydali vositani o'rnatish misoli quyidagicha:

```shell
# ripgrep: a faster grep with better defaults
brew install ripgrep

# fd: a faster, user-friendly find
brew install fd
```

Bular o'rnatilgach, siz `grep` o'rniga `rg` dan va `find` o'rniga `fd` dan foydalanishingiz mumkin.

> **`curl | bash` haqida ogohlantirish**: Siz tez-tez `curl -fsSL https://example.com/install.sh | bash` kabi o'rnatish ko'rsatmalarini ko'rasiz. Ushbu usul skriptni yuklab oladi va uni darhol bajaradi, bu qulay, lekin xavfli; siz o'zingiz tekshirmagan kodni ishga tushirayapsiz. Xavfsizroq yondashuv avval yuklab olish, ko'rib chiqish va keyin bajarishdir:
> ```shell
> curl -fsSL https://example.com/install.sh -o install.sh
> less install.sh  # review the script
> bash install.sh
> ```
> Ba'zi o'rnatuvchilar biroz xavfsizroq usuldan foydalanadi: `/bin/bash -c "$(curl -fsSL https://url)"`, bu hech bo'lmaganda joriy shell o'rniga skriptni bash izohlashini ta'minlaydi.

Siz tizimga o'rnatilmagan buyruqni ishga tushirmoqchi bo'lsangiz, terminalingizda `command not found` (buyruq topilmadi) degan xabarni ko'rasiz. [command-not-found.com](https://command-not-found.com) veb-sayti istalgan buyruqni turli operatsion tizimlar yoki paketlar menejeri yordamida qanday o'rnatish mumkinligini bilib olishingiz mumkin bo'lgan foydali resursdir.

Yana bir foydali vosita - bu [`tldr`](https://tldr.sh/) bo'lib, u soddalashtirilgan, misollarga yo'naltirilgan qo'llanma (man) sahifalarini taqdim etadi. Uzoq hujjatlarni o'qish o'rniga umumiy foydalanish misollarini tezda ko'rishingiz mumkin:

```console
$ tldr fd
  An alternative to find.
  Aims to be faster and easier to use than find.

  Recursively find files matching a pattern in the current directory:
      fd "pattern"

  Find files that begin with "foo":
      fd "^foo"

  Find files with a specific extension:
      fd --extension txt
```

Ba'zida sizga yangi dastur kerak emas, balki shunchaki ba'zi parametrlari berilgan mavjud dastur uchun qisqartma kerak bo'ladi xolos. Bu erda alias'lar sizga yordamga keladi.

Shuningdek biz o'rnatilgan `alias` shell buyrug'i yordamida buyruqlarning o'z alias'larini (qisqartmalarini) yaratishimiz mumkin.
Shell alias'i – boshqa buyruqning qisqartmasi bo'lib, iborani baholashdan (evaluate) avval shell o'zi avtomatik tarzda uni keraklisiga almashtiradi.
Bash'dagi alias quyidagicha tuzilishga ega bo'ladi:

```bash
alias alias_name="command_to_alias arg1 arg2"
```

> E'tibor bering, tenglik = belgisining atrofida bo'sh joy bo'lmasligi lozim, chunki [`alias`](https://www.man7.org/linux/man-pages/man1/alias.1p.html) bu bir dona argumentni qabul qiladigan shell buyrug'idir.

Alias'lar ko'plab qulayliklarga ega:

```bash
# Make shorthands for common flags
alias ll="ls -lh"

# Save a lot of typing for common commands
alias gs="git status"
alias gc="git commit"

# Save you from mistyping
alias sl=ls

# Overwrite existing commands for better defaults
alias mv="mv -i"           # -i prompts before overwrite
alias mkdir="mkdir -p"     # -p make parent dirs as needed
alias df="df -h"           # -h prints human readable format

# Alias can be composed
alias la="ls -A"
alias lla="la -l"

# To ignore an alias run it prepended with \
\ls
# Or disable an alias altogether with unalias
unalias la

# To get an alias definition just call it with alias
alias ll
# Will print ll='ls -lh'
```

Alias'larning ham cheklovlari bor, ular buyruq orasidan turib boshqa bir argumentlar qabul qila olmaydi. Buyruqni batafsil va mukammalroq sozlash uchun asosan shell funksiyalaridan foydalangan ma'qul.

Aksariyat shell'lar tarixda orqaga qidirish (reverse history search) funksiyasi uchun `Ctrl-R` ni qo'llab-quvvatlaydi. `Ctrl-R` ni bosing va oldingi buyruqlar orasidan qidirish uchun yoza boshlang. Oldinroq fzf haqida tushuncha bergan edik, u noaniq qidirishlar qila oluvchi kuchli uskuna hisoblanadi. Agar shell sozlangan bo'lsa fzf va `Ctrl-R` terminaldagi tarixingizdan noaniq qidiruvlarni amalga oshira oluvchi yanada mukammalroq interaktiv oynaga aylanadi.

Nuqtali fayllaringizni qanday tashkillashtirishingiz kerak? Ular o'zlarining alohida papkasida saqlanishi lozim va barchasi uchun versiya nazorati **symlink** (simvolik havola) qilinib skriptlangan bo'lishi kerak. Bu quyidagicha afzalliklarga ega:

- **O'rnatish oson**: yangi qurilmaga o'tib eski ishlarni qayta tiklash bir necha daqiqa vaqt oladi, xolos.
- **Portativlik**: sizning uskunangiz qayerda ishlasangiz ham deyarli hamma bilan ishlashga qodir.
- **Sinxronizatsiya**: xohlagan vaqtingizda ma'lum bir papkani yangilaysiz va qolgan barcha fayllar shu zahoti yangilanadi.
- **O'zgarishni kuzatish (Change tracking)**: muhandis ekanligingizni hisobga olsak, siz o'zingizning muhandislik tajribangizda ba'zi fayllarga yillar davomida xizmat ko'rsatishingizga to'g'ri keladi, shunday bo'lsada, bunday hollarda tarixni saqlaydigan ilovalarning mavjud bo'lishi ishni tezlashtiradi.

Ushbu nuqtali fayllar ichiga qanday papka yoki resurs joylash lozim?
Turli xil asbob uskunalarning internetdagi manbalaridan yoki bo'lmasa [man pages](https://en.wikipedia.org/wiki/Man_page) orqali ularni sozlashni o'rganishingiz mumkin. O'rganishning yana bir ajoyib usuli - bu maxsus dasturlar haqida internetdagi blog postlarni qidirish, bu yerda mualliflar o'zlarining afzal ko'rgan sozlamalari haqida gapirib berishadi. Nuqtali fayllar haqida va uni o'rganish haqida bilishning yana bir ajoyib usuli boshqalarning qanday tashkillaganligini ko'rib chiqishdir, deyarli ko'p sonli [repositories](https://github.com/search?o=desc&q=dotfiles&s=stars&type=Repositories) GitHub'da mavjud, hamda uning eng ko'p ko'rilgan turini [mana bu havolada ko'rsangiz bo'ladi](https://github.com/mathiasbynens/dotfiles) (albatta hech kim sizga ularning ishlarini butkul ko'chirib olishni maslahat bermaydi). [Bu yerda](https://dotfiles.github.io/) bu haqida yana bitta foydali manba qoldirdik.

Ma'ruzada qatnashgan va dars berganlarning ham dotfiles manbalari ochiq formatda Github sahifalarida ko'rishga mavjud: [Anish](https://github.com/anishathalye/dotfiles), [Jon](https://github.com/jonhoo/configs), [Jose](https://github.com/jjgo/dotfiles).

Shell'ni yanada qulaylashtirishda **Fermvorklar va Pluginlar** ning ham o'rni beqiyosdir. Umumiy maqsadlarda foydalanish uchun mashhur bo'lgan frameworklar [prezto](https://github.com/sorin-ionescu/prezto) va [oh-my-zsh](https://ohmyz.sh/), shuningdek alohida xususiyatlarni ishlashini mo'ljallangan Pluginlarga:

- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) - to'g'ri/noto'g'ri yozilayotgan buyruqlarni ranglarda ajratib borishni amalga oshiradi
- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) - eski buyruqlar tarixidan so'zni yoza boshlashingizdanoq tavsiya ko'rsatib boradi
- [zsh-completions](https://github.com/zsh-users/zsh-completions) - kod va buyruqlarni to'liq bajaradi (to'ldiradi)
- [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search) - xuddi fish kabi qidirish tarixini qayd etadi
- [powerlevel10k](https://github.com/romkatv/powerlevel10k) - tezkor, turlanuvchi oyna dizaynini tortiq etadi.

[fish](https://fishshell.com/) kabi shell'larning o'zida ham qaysidir ma'noda tepadagi imkoniyatlarning ba'zilari beriladi.

> Tepadagi imkoniyatlarga ega bo'lish uchun doim ham katta-katta loyiha yoki feremworklardan (oh-my-zsh kabi) foydalanish shart emas. Ba'zan unga doir bo'lgan maxsus plaginlardan foydalanish, sizga qulaylikdan tashqari tezlik ham in'om etadi. Keng qamrovdagi katta feremworklar shell ishga tushish tezligini sekinlashtirib yuborishi mumkin. Shunday ekan, faqat o'zingiz foydalanadigan maxsus plaginlarni birma bir o'rnating, deb aytgan bo'lardik.

# AI in the Shell

Terminalda AI (sun'iy intellekt) uskunalarini qo'shishning turli yo'llari mavjud. Bu yerda bir nechta turli darajadagi integratsiya usullari ko'rsatilgan:

**Buyruqlarni yaratish (Command generation)**: [`simonw/llm`](https://github.com/simonw/llm) kabi vositalar tabiiy til ta'riflaridan (natural language descriptions) kelib chiqib, terminal buyruqlarini yaratishga yordam berishi mumkin:

```console
$ llm cmd "find all python files modified in the last week"
find . -name "*.py" -mtime -7
```

**Pipeline'larni integratsiyalash (Pipeline integration)**: LLM'larni terminal pipeline'lariga ma'lumotlarni qayta ishlash yoki o'zgartirish maqsadida ulash mumkin. Ular ayniqsa bir xil qolipga tushmagan turli xildagi qiyin va regular expression'lar bilan ham to'g'irlab bo'lmaydigan holatlarda ishlaganda juda foydalidir:

```console
$ cat users.txt
Contact: john.doe@example.com
User 'alice_smith' logged in at 3pm
Posted by: @bob_jones on Twitter
Author: Jane Doe (jdoe)
Message from mike_wilson yesterday
Submitted by user: sarah.connor
$ INSTRUCTIONS="Extract just the username from each line, one per line, nothing else"
$ llm "$INSTRUCTIONS" < users.txt
john.doe
alice_smith
bob_jones
jdoe
mike_wilson
sarah.connor
```

Bu yerda biz o'zgaruvchining oralarida (words, sentences) probel qatnashganligi sababli `"$INSTRUCTIONS"` dan foydalanamiz hamda faylni stdin'ga `< users.txt` fayl mazmunini yo'naltirish (redirect) uchun ishlatamiz.

**AI shells**: [Claude Code](https://docs.anthropic.com/en/docs/claude-code) singari vositalar (tools), asosan, meta-shell ko'rinishida bo'lib, ular berilgan inglizcha (natural language) gap va iboralarni shell buyruqlari, harakatlari va shuningdek turli og'ir bosqichli amaliyotlarga olib keladigan buyruqlarga ham tarjima qilib (ya'ni uni asl shell'ning tiliga tushirib) foydalanuvchiga tortiq qila oladi.

# Terminal Emulators

Shell'ni moslashtirganingiz bilan bir qatorda, qaysi **terminal emulyatori**dan va uning qanday parametrlari va imkoniyatlaridan foydalanmoqchiligingiz to'g'risida o'ylab ko'rishni maslahat berardik.
Terminal emulyatori (terminal emulator) sizning shell'ingiz ishga tushadigan grafikli asbob bo'lib xizmat qiluvchi dasturdir. Bunday imkoniyatlarni xususida taklif qila oluvchi ko'plab Terminal emulatorlari mavjud.

Terminalga juda ko'plab yoki yuzlab va minglab ishingiz yuzasidan soatlaringizni ajratganligingiz sababli bu haqda mulohaza yuritish arzimas narsa emas va biz buni foydali ham deya olamiz. Quyidagilarni o'z terminalingizda xohishingizga ko'ra o'zgartira olishingiz mumkin va bu tavsiya ham etiladi:

- Shriftlarni tanlash (Font choice)
- Ranglar jilosi (Color scheme)
- Tugmalar ketma-ketligi (Keyboard shortcuts)
- Tablar va Panellarni sozlash (Tab/Pane support)
- History - orqaga qaytish bo'yicha cheklovlarni tahrirlash (Scrollback configuration)
- Tezlik va Unumdorlikni (Performance) oshirish (ko'plab [Alacritty](https://github.com/alacritty/alacritty) yoki [Ghostty](https://ghostty.org/) singari yangi terminallar tezlik oshishi jarayonida GPU acceleration dan ham foydalanishadi).

# Mashqlar

## Argumentlar va Globlar

1. Bazi buyruqlarni terminalingizda shunday holatda uchratishingiz ham mumkin ekan: `cmd --flag -- --notaflag`. Buyruqdagi `--` bayrog'i (flag) ba'zi holatlarda terminaldagi komandalar kiritib bo'linganini anglatadi va ulardan so'ng hech qanday bayroq argumenti deb atalmaydi balki ular komandaning o'zgarmas qismlari argumentlariga (positional argument) aylanadi deydi. Buning foydali tarafi haqida ma'lumot qidirib toping yoki `touch -- -myfile` ni terminalingizda tekshirib ko'ring. Endi, qo'shib yaratib ko'rgan shu faylni terminaldan o'chirishga harakat qiling (`--` belgisiz).

1. [`man ls`](https://www.man7.org/linux/man-pages/man1/ls.1.html) ni o'qing va quyidagicha tartibda chiqarib beruvchi `ls` buyrug'ini yozing:
    - Barcha fayllarni (yashirilgan fayllarni ham qoshib) ko'rsatsin.
    - O'lchami odam tushunadigan shaklda bo'lsin (masalan: 454279954 o'rniga 454M kabi)
    - Fayllarni eng yangisidan boshlab tartiblasin.
    - Ranglarda ajratib olib chiqqan holda.

    Yakuniy natija xuddi mana bunday ko'rinishda bo'lsin:

    ```
    -rw-r--r--   1 user group 1.1M Jan 14 09:53 baz
    drwxr-xr-x   5 user group  160 Jan 14 09:53 .
    -rw-r--r--   1 user group  514 Jan 14 06:42 bar
    -rw-r--r--   1 user group 106M Jan 13 12:12 foo
    drwx------+ 47 user group 1.5K Jan 12 18:08 ..
    ```

1. Jarayonni almashtirish (Process substitution) `<(command)` sizga xuddi buyruq chiqishi fayldan uzatilayotganiday his qilish imkonini beradi. `diff` dan `printenv` va `export` buyruqlarining chiqishini tekshirish uchun ushbu `process substitution` dan foydalaning va qanday natijaga erishishni ko'ring. Nima uchun ularning chiqishi har xil bo'ldi? (Maslahat: quyidagi buyruqni tekshirib ko'ring: `diff <(printenv | sort) <(export | sort)`).

## Muhit o'zgaruvchilari

1. Har safar chaqirilganda huddi joriy oynani qaysidir bir usul (xotirada saqlash nazarda tutilgan bo'lishi mumkin) bilan saqlab olib ishlata oladigan bash funksiyasi `marco` yozing. Keyin yana bitta shunaqangi bash funksiyasi `polo` yozing va bu polo o'zida siz hohlagan papkaga yoki qaerga bolsada sizni bir marta polo buyrug'ini tushirishdanoq o'sha `marco` xotirasiga olib borgan papkaga yuborsin. Buni `cd` buyrug'i orqali ishlash eng oson usuli, yoki `marco.sh` yoki `source marco.sh` dan ham foydalansa bo'ladi.

## Qaytish kodlari

1. Sizning qurilmangizdagi buyruqlaringizdan biri kamdan kam hollardagina xatolik bilan ishlamay to'xtab qoladi deb faraz qilaylik, xato shundaki qachon u aynan nima bo'lganda to'xtashini aniqlashimiz uchun butun boshli operatsiyani qayta tushirish bilan uni muvofaqqiyatsizlikka uchrashini tekshirishimiz vaqtimizni va qimmatli resurslarni talon taroj qilish bo'ladi xolos. Bash terminalni operatsion skriptga almashtiring xotoki skript o'z xatolik va oqimlarni oxirida to'g'ri chop eta olsa ham. Agar script qachon tugashini aniqlab unga hisoblovchini qo'shib quysak ham yaxshi yechim bo'lardi xolos.

    ```bash
    #!/usr/bin/env bash

    n=$(( RANDOM % 100 ))

    if [[ n -eq 42 ]]; then
       echo "Something went wrong"
       >&2 echo "The error was using magic numbers"
       exit 1
    fi

    echo "Everything went according to plan"
    ```

## Signallar va vazifalarni boshqarish

1. Terminalni `sleep 10000` bilan boshlang so'ng uni `Ctrl-Z` qiling va `bg` jarayonini ishga tushiring. Endi o'zingiz uni tekshirib ko'ring o'z pidini uning jarayoni bilan ishlangan yoki ishlash holatiga qaytib ketgan deb, [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html) o'z pidida va [`pkill`](https://man7.org/linux/man-pages/man1/pgrep.1.html) ham uning jarayonida ishlamagan holda yoki hech narsa qaytarmay deysizmi shunday tekshirib koring (Maslahat - `af` flaglaridan ham foydalanib koring).

1. Ehtimol sen qachondir nimadir bitmagunicha keyingisini to'xtatib yoki unga ulangan holda ishlash haqida eshitgan bo'lishing mumkun. Uni qanday qilib bitiradi va davomiyligi ham shunda ishlanadi deb so'rab qo'ymoqchisiz. Bularning bari `sleep 60 &` buyrug'i bilan amalga kelsa barchasi qolganini o'zi bitkazadi deb faraz qilmayapsizmi? Aslida ish yechimi uni ishlatib korish va bash komandalari to`g'ri ko'rib chiqilsa uning [`wait`](https://www.man7.org/linux/man-pages/man1/wait.1p.html) buyrug'i hamma muammo echimi yordamchi bo'lib xizmat qiladi. Endi `ls` deyarli fon rejimida bitgunicha uni terminaldan chiqaverib yoki qanday ishlaydi o'zingiz tekshirib ishlatib ko'ring.

    To'g'ri barcha shunday turli bash sessiyalarday bo'lmagan, lekin undan chiqa olish `wait` ga ishloladigan bash larda bo'lgan kabi boshqa buyruqlar ham o'ziniki kabidir. Keyin kill degan buyrug'imiz boru deya aytishga haqlisan, bazi terminal xususiyatlarini avval gapirmadik uning haqida sababi `kill` boshqa turkum jarayonlarini chiqish jarayonini nolligiga va noldan farqliligiga nisbatan olinadigan buyruqlardandir. Ya'ni terminal signallaridan kelib chiqgan buyruqdirki unga jarayonlarda qanday bitib shunga doir qiziq yechimlarini siz ham ishlatib koring. Oddiygina yana bitmaguncha yoki davomigacha turadigan deb aytib ketadiganlariga qanday misol uchun biron `pidwait` o'zimiz xoxlagancha deb koring, va pid bilan baribir qanday ishlangan unga `sleep` buyruq qo'shilishi oqibatni nima bo'ladi hammasini kuzatib chiqing deyman.

## Fayllar va ruxsatlar

1. (Advanced) Oxirgi ishlangan bir necha ishlangan buyruqlarga qarata papkalar va shuningdek fayllarni qidirib ularning vaqti buyicha saralab uni bash ga yoki shunga o'xshash bo'lsa deydi xamma qanaqib hamma eng yangilarini rekursiv tarizda ishlatishini aniqlab terminalingizda ishlating qani buning ehtiyoji siz uchun ish bera oladimi deydi bu yerda?

## Terminal multipleksorlari

1. O'zingiz shunchaki bu [tutorialni](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) ochib o'qib undan asboblarni `tmux` deb va boshqa bo'limini qidirib o'rganib chiqib va ko'rsatmalar asosidagi barcha kerak deb topgan qo'shimchali narsalarni ham [qadamlarga amal qilib sozlab koring](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/) siz uchun qo'shimcha yaxshi bilim bo'la oladi.

## Aliaslar va nuqlari fayllar

1. Biz kop narsa yoki yozuvlar yozganimizda doim to'g'ri chiroyli chiqmasligiga yoki uning bashqa sababi oddiy harf almashtirib misol uchun `cd` deb ko'rganmiz. Agar bash deb o'shani `dc` deb yoza boshlaganda shu hato o'rnini almashtiruvchi bash komanda yarating yoki o'shanga o'xshab o'zingiz alias ko'ring deydi terminalda yaratsangiz bo'ladi deymiz.

1. Bunga o'xshash xotiralardagi misollarni hammasini ishlatib va top ko'pchilik `history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10` kabilarda ishlating koring qandaydir misollar yoqilsa Bash bo'yicha unda yana ko'pi unga o'xshatib qo'llanishiga ham ko'rishingiz mumkinligiga `history 1` qilinishi uni boshqacharoq bo'lishini bildiradi, Bash misolidami yo bu holat undan boshqa misol ZSH yoki boshqadadir.

1. Endilikda ushbu ishni amalga oshirishda albatta uning hamma muhim bo'lgan o'sha versiyalarni ham hammasi o'zinikiga deb `dotfiles` da papka ko'ring.

1. Hozir yana ishlangan o'rnatmalari yoki papka uning o'zini ba'zisini bash`ga (eng kamiga o'zini komanda unga mos qilinish uchun ` $PS1 ` bilan) ni sozlab koring.

1. Qo'shimchalarga yana qanday narsa o'rnatmalar uchun siz har hil scriptlar yordamga yoki bironta deb unga osongina yangi joy olib u yerda uni skriptlanganini qo'llab kuring o'zingiz deymiz qiyin emas yoki boshqa deb aytolamiz albatta o'rganishingiz un `ln -s` qilib uni tayyor bo'lgan o'sha joyga `ln -s` da kabi ishlatsangiz bo'ladi o'xshash [tayyor narsalar](https://dotfiles.github.io/utilities/) kabi foydali misol.

1. Test va ko'plab qurilma bo'ylab terminal skriptiga bir necha mashinalardan olingan holda ishlay olasizmi.

1. Yuqoridagilarga xos dotfiles hamma ishlashinggizdagi resurslarni GitHub da o'z dotfiles manziliga jamlang ko'ring deymiz bunisi haqida

1. Endi yakuni github da qilingan ishlar o'zi ishonch hosil qilishiga ulashish deb ko'ramiz hamma qila oladiganingiz shulardir hozir.

## Masofaviy mashinalar (SSH)

Biror Linux virtual mashinasini (virtual machine) ushbu mashqlar uchun o'rnating yoki boridan foydalanib ham tursangiz bo'laveradi eng osonrog'ini tanlab ishlatgan bo'lardingiz agar o'rnatish haqida to'liq tushunchangiz yoki biron tayyor holingiz kabi o'ylashda unga tayyorlanmoqchi [ushbu (virtual machine](https://hibbard.eu/install-ubuntu-virtual-box/)) deb berilgan darsdan tushunib o'rnating hoziroq uni ishga kirishingiz vaqti.

1. So'ng `~/.ssh/` larga o'ting u yerdan juftligingiz SSH bo'lishga yo undan kelib ishlanayotgani deb bu erga va ularda bormikin deb ssh-keygenlarni yoki agar o'sha paytida o'sha degan bilan yoki umuman bo'lmasachi deyilganlaridanchi uning ko'rinishi shu `ssh-keygen -a 100 -t ed25519` bo'ladi o'zingiz yarata oling deymiz bular un va uni o'zingiz boshqalarga qulay deb va unda ko'rinishi bo'lib turuvchi parolni himoya uchun ham oling va unga qilingani `ssh-agent` uni yodga saqlash deyilishi bo'ladi xuddi u erdan keluvchi bu[manba havolasidan](https://www.ssh.com/ssh/agent) bilib olsangiz uning ma'nosi.

1. Ushbu sozlamalarni endi o'zingizning `.ssh/config` kabi o'rniga kirib bo'lgach quydagi tartib shunga asosan bular uchun deyilgan ko'rinishida berib tuziting deydi ularni:

    ```bash
    Host vm
        User username_goes_here
        HostName ip_goes_here
        IdentityFile ~/.ssh/id_ed25519
        LocalForward 9999 localhost:8888
    ```

1. Serveringizdagi bo'lish ehtimol qilinuvchilarni keyingilarga ham u qadar deya yuborilishiga yoki qopiyasiga shu serverni ishlatilgan va yo'qni nusxalash yozilganlaringizdan biri ushbu buyruq ishlatilinib qo'yasiz holos serverga ularni qaysingdir uzi `ssh-copy-id vm`.

1. Buyruqlar qatorini yozganingiz bilanoq hozir qanaqilib shu python deb uni ishlata oluvchisida ishlashni xohlayman desangizu uning shuni misoldagi kabilar bilan server qilinuvchida ishga tushing hozir buni `python -m http.server 8888` bu orqali unda unga ko'rinuvchi ishlagan ishiga va keyin borilgandagi holatini ko'rgan kabi ishlatsin keyin o'zingiz qaysi bir brauzeringiz bilan `http://localhost:9999` deb chaqirib ko'rsangiz o'sha siz ishlashini mo'ljallagan joydan to'g'ri ishlanganligi unga chiqadi ishlagan holda bo'ladi shular deb misollar kelajakda kerak deb qilasiz ko'rsatuvda qilinganga hammani bu holati un ishini yengilligi.

1. Asl SSH ga kirilgandagi config deb yozilib uning papkasigacha borilgach endi sizning unga berib chiqargan o'zingiz bilmaysizmi yoki u orqali deysiz va tepadagi ma'lum bo'ladiki buyruq shu `sudo vim /etc/ssh/sshd_config` deb undan ham yana ba'zilarni ya'ni bu ruxsati uchun `PasswordAuthentication` qilib yoki shundan parolingiz umuman qilinishiga rozi holatini `PermitRootLogin` qilamiz deb uniki serveri barcha deyilishini qiling `sudo service sshd restart` ko'rib beruvchisini sozlagan deyilishigayam kabi o'qib tekshiring ko'ring o'xshaydimi serverini bilganiga keyin yana xavfsizini qaytib kiring koring yana xar narsasiga sshing ga qilinuvchi ham ishlash un qila oldingizmi yana bilishni o'sha ssh serverga.

1. (Challenge) Biz shu qilinganiga ko'ra bu ishiga terminal VM mashinalarimiz u ish bo'ladigandan qilinadiganidan qilinmagandan yoki tepadagisi shunday [`mosh`](https://mosh.org/) uning ishlay olishidan va unga aloqaga server va unga boshqacharoq bo'lishlar yo o'chirilish nimasidir orqasida umuman bormay yoxud shunday ekan tarmoqni uzib unga ulab va unga deymizu endi u ulana oldimikin qanaqilib unday ula olsa bu ham server tiklanirolmikin yana deb ulashgami.

1. (Challenge) Bunisida endigina qanchadir ulashlardan yoki endi o'rganilganida ssh deganga qo'yilib buni o'z vaqtida bilmoq yoki bormikin `ssh` kabi -N ham flaglari bilan yana -f ham endi ularni foni va yo'naltirish (background port forwarding) yoxud o'zi haqida yo bo'lmasam boshqasi xatto endi siz qo'shimchasini hammasiniki bilan nimada va ishida qandaydir nimasi bor bunisi endi faqat deb o'zingiz buyruq yoqib uni shulardan iborat bir natijani port forwarding bo'yicha ko'rsatuvni bajaring qanaqa amalda yozib bilishda yodlashga arziydi ushbu siz kabi.