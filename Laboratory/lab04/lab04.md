---
# Front matter
title: "Отчёт по лабораторной работе 4"
author: "Макухина Анастасия Вадимовна"

# Generic otions
lang: ru-RU
toc-title: "Содержание"

# Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

# Pdf output format
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
### Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Misc options
indent: true
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Получение практических навыков работы в консоли с расширенными атрибутами файлов.


# Выполнение лабораторной работы

1. От имени пользователя guest определим расширенные атрибуты файла /home/guest/dir1/file1 командой:

lsattr /home/guest/dir1/file1 - [Рисунок 1](Images/1.png){ #fig:001 width=70% }.

2. Установим на файл file1 права, разрешающие чтение и запись для владельца файла командой:

chmod 600 file1 - [Рисунок 2](Images/2.png){ #fig:001 width=70% }.

3. Попробуем установить на файл /home/guest/dir1/file1 расширенный атрибут «а» от имени пользователя guest:

chattr +a /home/guest/dir1/file1 - [Рисунок 3](Images/3.png){ #fig:001 width=70% }.

Был получен отказ от выполнения операции.

4. Повысим свои права с помощью команды su. Попробуем вновь установить расширенный атрибут «а» на файл /home/guest/dir1/file1 от имени суперпользователя:

su

chattr +a /home/guest/dir1/file1 - [Рисунок 4](Images/4.png){ #fig:001 width=70% }.

5. От пользователя guest проверим правильность установления атрибута:

su guest

lsattr /home/guest/dir1/file1 - [Рисунок 5](Images/5.png){ #fig:001 width=70% }.

Атрибут установлен правильно.

6. Выполним дозапись в файл file1 слова «test» командой echo "test" >> /home/guest/dir1/file1. После этого выполним чтение файла file1 командой cat /home/guest/dir1/file1.

echo "test" >> /home/guest/dir1/file1

cat /home/guest/dir1/file1 - [Рисунок 6](Images/6.png){ #fig:001 width=70% }.

Слово test было успешно записано в file1.

7. Попробуем стереть информацию имеющуюся в файле file1 командой:

echo "abcd" > /home/guest/dirl/file1 - [Рисунок 7](Images/7.png){ #fig:001 width=70% }.

Попробуем переименовать файл:

mv /home/guest/dir1/file1 /home/guest/dir1/file2 - [Рисунок 7](Images/7.png){ #fig:001 width=70% }.

В обоих случаях получим отказ.

8. Попробуем установить на файл file1 права, например, запрещающие чтение и запись для владельца файла с помощью команды:

chmod 000 file1 - [Рисунок 8](Images/8.png){ #fig:001 width=70% }.

Снова получим отказ.

9. Снимим расширенный атрибут «а» с файла /home/guest/dirl/file1 от имени суперпользователя командой:

su

chattr -a /home/guest/dir1/file1 - [Рисунок 9](Images/9.png){ #fig:001 width=70% }.

Повторим операции, которые ранее не удавалось выполнить.

echo "abcd" > /home/guest/dirl/file1

mv /home/guest/dir1/file1 /home/guest/dir1/file2

chmod 000 file1 - [Рисунок 9](Images/9.png){ #fig:001 width=70% }.

Все операции удалось выполнить.

10. Повторим действия по шагам, заменив атрибут «a» атрибутом «i»:

[Рисунок 10](Images/10.png){ #fig:001 width=70% }.

Получим аналогичные результаты за исключением того, что дозапись информации в файл с установленным атрибутом «i» не удалась.

# Выводы

В ходе выяполнения работы я повысила свои навыки использования интерфейса командой строки (CLI), познакомилась на примерах с тем,
как используются расширенные атрибуты при разграничении доступа. Опробовала действие на практике расширенных атрибутов «а» и «i».