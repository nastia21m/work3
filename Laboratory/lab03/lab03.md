---
# Front matter
title: "Отчёт по лабораторной работе 1"
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

Получение практических навыков работы в консоли с атрибутами файлов для групп пользователей.


# Выполнение лабораторной работы

1. В установленной операционной системе создаём учётную запись пользователя guest (использую учётную запись администратора):

* su root - переход в уч. запись
* useradd guest - создание записи

[Рисунок 1](Images/01.png){ #fig:001 width=70% }

2. Зададим пароль для пользователя guest:

passwd guest - [Рисунок 2](Images/02.png){ #fig:001 width=70% }

3. Аналогично создаём второго пользователя guest2. - [Рисунок 3](Images/1.png){ #fig:001 width=70% }.

4. Добавим пользователя guest2 в группу guest:

gpasswd -a guest2 guest - [Рисунок 4](Images/2.png){ #fig:001 width=70% }.

5. Осуществим вход в систему от двух пользователей на двух разных консолях: guest на первой консоли и guest2 на второй консоли - [Рисунок 5](Images/3.png){ #fig:001 width=70% }.

6. Для обоих пользователей командой pwd определим директорию, в которой мы находимся - [Рисунок 6](Images/4.png){ #fig:001 width=70% }.

Результаты соответствуют приглашениям командных строк.

7. Уточним имя пользователя, его группу, кто входит в неё
и к каким группам принадлежит он сам. Определим командами
groups guest и groups guest2, в какие группы входят пользователи guest и guest2. 

* id -Gn 
* id -G.

[Рисунок 7](Images/5.png){ #fig:001 width=70% }, [Рисунок 8](Images/6.png){ #fig:001 width=70% }.

Все команды дают одинаковый результаты.

8. Сравним полученную информацию с содержимым файла /etc/group.
Просмотрим файл командой:

cat /etc/group - [Рисунок 9](Images/7.png){ #fig:001 width=70% }.

Получим тот же результат.

9. От имени пользователя guest2 выполним регистрацию пользователя
guest2 в группе guest командой:

newgrp guest - [Рисунок 10](Images/8.png){ #fig:001 width=70% }.

10. От имени пользователя guest изменим права директории /home/guest,
разрешив все действия для пользователей группы:

chmod g+rwx /home/guest - [Рисунок 11](Images/9.png){ #fig:001 width=70% }.

11. От имени пользователя guest снимии с директории /home/guest/dir1
все атрибуты командой:

chmod 000 dirl - [Рисунок 12](Images/10.png){ #fig:001 width=70% }.

Проверим правильность снятия атрибутов:

ls -l dir1 - [Рисунок 13](Images/11.png){ #fig:001 width=70% }.

Меняя атрибуты у директории dir1 и файла file1 от имени пользователя guest и делая проверку от пользователя guest2, заполним табл. 3.1,
определив опытным путём, какие операции разрешены, а какие нет. Если операция разрешена, заносим в таблицу знак «+», если не разрешена,
знак «-».

[Таблица 3.1(1)](Images/12.png){ #fig:001 width=70% }, [Таблица 3.1(2)](Images/13.png){ #fig:001 width=70% }, [Таблица 3.1(3)](Images/14.png){ #fig:001 width=70% }.

На основании заполненной таблицы определим те или иные минимально необходимые права для выполнения пользователем guest2 операций
внутри директории dir1 и заполним табл. 3.2.

[Таблица 3.2](Images/15.png){ #fig:001 width=70% }.

# Выводы

В ходе выяполнения работы я преобрела практические навыки работы в консоли с атрибутами файлов для групп пользователей.