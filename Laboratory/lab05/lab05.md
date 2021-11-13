---
# Front matter
title: "Отчёт по лабораторной работе 5"
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

Изучение механизмов изменения идентификаторов, применения SetUID- и Sticky-битов. Получение практических навыков работы в консоли с дополнительными атрибутами. Рассмотрение работы механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.


# Выполнение лабораторной работы

1. Войдём в систему от имени пользователя guest

[Рисунок 1](Images/1.png){ #fig:001 width=70% }.

2. Создайте программу simpleid.c:

nano simpleid.c

#include <sys/types.h>

#include <unistd.h>

#include <stdio.h>

int

main ()

{

uid_t uid = geteuid ();

gid_t gid = getegid ();

printf ("uid=%d, gid=%d\n", uid, gid);

return 0;

}

[Рисунок 2](Images/2.png){ #fig:001 width=70% }, [Рисунок 3](Images/3.png){ #fig:001 width=70% }.

3. Скомплилируем программу и убедимся, что файл программы создан:

gcc simpleid.c -o simpleid - [Рисунок 4](Images/4.png){ #fig:001 width=70% }.


4. Выполним программу simpleid:

./simpleid - [Рисунок 5](Images/5.png){ #fig:001 width=70% }.

5. Выполните системную программу id:

id - [Рисунок 6](Images/6.png){ #fig:001 width=70% }.

Результаты одинаковы.

6. Усложним программу, добавив вывод действительных идентификаторов:

#include <sys/types.h>

#include <unistd.h>

#include <stdio.h>

int

main ()

{

uid_t real_uid = getuid ();

uid_t e_uid = geteuid ();

gid_t real_gid = getgid ();

gid_t e_gid = getegid () ;

printf ("e_uid=%d, e_gid=%d\n", e_uid, e_gid);

printf ("real_uid=%d, real_gid=%d\n", real_uid,

,→ real_gid);

return 0;

}

Получившуюся программу назовём simpleid2.c - [Рисунок 7](Images/7.png){ #fig:001 width=70% }.

7. Скомпилируем и запустим simpleid2.c:

gcc simpleid2.c -o simpleid2

./simpleid2 - [Рисунок 8](Images/8.png){ #fig:001 width=70% }.

8. От имени суперпользователя выполним команды:

chown root:guest /home/guest/simpleid2

chmod u+s /home/guest/simpleid2 - [Рисунок 9](Images/9.png){ #fig:001 width=70% }.

9. Повысим временно свои права с помощью su.

su - [Рисунок 10](Images/10.png){ #fig:001 width=70% }.

10. Выполним проверку правильности установки новых атрибутов и смены владельца файла simpleid2:

ls -l simpleid2 - [Рисунок 11](Images/11.png){ #fig:001 width=70% }.

11. Запустим simpleid2 и id:

./simpleid2

id - [Рисунок 12](Images/12.png){ #fig:001 width=70% }.

Результаты одинаковы.

12. Проделайте тоже самое относительно SetGID-бита - [Рисунок 13](Images/13.png){ #fig:001 width=70% }.

13. Создадим программу readfile.c:

#include <fcntl.h>

#include <stdio.h>

#include <sys/stat.h>

#include <sys/types.h>

#include <unistd.h>

int

main (int argc, char* argv[])

{

unsigned char buffer[16];

size_t bytes_read;

int i;

int fd = open (argv[1], O_RDONLY);

do

{

bytes_read = read (fd, buffer, sizeof (buffer));

for (i =0; i < bytes_read; ++i) printf("%c", buffer[i]);

}

while (bytes_read == sizeof (buffer));

close (fd);

return 0;

}

[Рисунок 14](Images/14.png){ #fig:001 width=70% }.

14. Откомпилируем её

gcc readfile.c -o readfile - [Рисунок 15](Images/15.png){ #fig:001 width=70% }.

15. Сменим владельца у файла readfile.c (или любого другого текстового файла в системе) и изменим права так, чтобы только суперпользователь (root) мог прочитать его, a guest не мог.

[Рисунок 16](Images/16.png){ #fig:001 width=70% }.

16. Проверим, что пользователь guest не может прочитать файл readfile.c

[Рисунок 17](Images/17.png){ #fig:001 width=70% }.

17. Сменим у программы readfile владельца и установим SetU’D-бит

[Рисунок 18](Images/18.png){ #fig:001 width=70% }.

18. Проверим, может ли программа readfile прочитать файл readfile.c

[Рисунок 19](Images/19.png){ #fig:001 width=70% }, [Рисунок 20](Images/20.png){ #fig:001 width=70% }.

19. Проверим, может ли программа readfile прочитать файл /etc/shadow

[Рисунок 21](Images/21.png){ #fig:001 width=70% }.

20. Выясним, установлен ли атрибут Sticky на директории /tmp, для чего выполним команду:

ls -l / | grep tmp - [Рисунок 22](Images/22.png){ #fig:001 width=70% }.

21. От имени пользователя guest создадим файл file01.txt в директории /tmp со словом test:

echo "test" > /tmp/file01.txt - [Рисунок 23](Images/23.png){ #fig:001 width=70% }.

22. Просмотрим атрибуты у только что созданного файла и разрешим чтение и запись для категории пользователей «все остальные»:

ls -l /tmp/file01.txt

chmod o+rw /tmp/file01.txt

ls -l /tmp/file01.txt - [Рисунок 24](Images/24.png){ #fig:001 width=70% }.

23. От пользователя guest2 (не являющегося владельцем) попробуем прочитать файл /tmp/file01.txt:

cat /tmp/file01.txt - [Рисунок 25](Images/25.png){ #fig:001 width=70% }.

24. От пользователя guest2 попробуем дозаписать в файл /tmp/file01.txt слово test2 командой:

echo "test2" > /tmp/file01.txt - [Рисунок 26](Images/26.png){ #fig:001 width=70% }.

Да, удалось.

25. Проверим содержимое файла командой:

cat /tmp/file01.txt - [Рисунок 27](Images/27.png){ #fig:001 width=70% }.

26. От пользователя guest2 попробуем записать в файл /tmp/file01.txt слово test3, стерев при этом всю имеющуюся в файле информацию командой:

echo "test3" > /tmp/file01.txt - [Рисунок 28](Images/28.png){ #fig:001 width=70% }.

27. Проверьте содержимое файла командой:

cat /tmp/file01.txt - [Рисунок 29](Images/29.png){ #fig:001 width=70% }.

28. От пользователя guest2 попробуем удалить файл /tmp/file01.txt командой:

rm /tmp/fileOl.txt - [Рисунок 30](Images/30.png){ #fig:001 width=70% }.

Не удалось.

29. Повысим свои права до суперпользователя следующей командой

su

и выполним после этого команду, снимающую атрибут t (Sticky-бит) с директории /tmp:

chmod -t /tmp - [Рисунок 31](Images/31.png){ #fig:001 width=70% }.

30. Покинем режим суперпользователя командой

exit - [Рисунок 32](Images/32.png){ #fig:001 width=70% }.

31. От пользователя guest2 проверим, что атрибута t у директории /tmp нет:

ls -l / | grep tmp - [Рисунок 33](Images/33.png){ #fig:001 width=70% }.

32. Повторите предыдущие шаги - [Рисунок 34](Images/34.png){ #fig:001 width=70% }.

Изменений не произошло.

33. Проверим, можно ли удалить файл от имени пользователя, не являющегося его владельцем - [Рисунок 35](Images/35.png){ #fig:001 width=70% }.

Удалось.

34. Повысим свои права до суперпользователя и вернём атрибут t на директорию /tmp:

su -

chmod +t /tmp

exit - [Рисунок 36](Images/36.png){ #fig:001 width=70% }.

# Выводы

В ходе выяполнения работы я изученла механизмы изменения идентификаторов, применения SetUID- и Sticky-битов, полученила практические навыки работы в консоли с дополнительными атрибутами, рассмотрела работы механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.
