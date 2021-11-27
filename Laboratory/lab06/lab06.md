---
# Front matter
title: "Отчёт по лабораторной работе 6"
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

Развить навыки администрирования ОС Linux. Получить первое практическое знакомство с технологией SELinux1. Проверить работу SELinx на практике совместно с веб-сервером Apache.


# Подготовка лабораторного стенда

1. Установим/обновим (за суперпользователя) веб-сервер Apache с помощью команды 

yum install httpd - [Рисунок 1](Images/1.png){ #fig:001 width=70% }.

2. В конфигурационном файле /etc/httpd/httpd.conf зададим параметр ServerName, чтобы при запуске веб-сервера не выдавались лишние сообщения об ошибках, не относящихся к лабораторной работе.

ServerName test.ru - [Рисунок 2](Images/2.png){ #fig:001 width=70% }.

3. Проследим, чтобы пакетный фильтр был отключен или в своей рабочей конфигурации позволял подключаться к 80-му и 81-му портам протокола tcp. Добавим разрешающие правила с помощью команд: 

iptables -I INPUT -p tcp --dport 80 -j ACCEPT

iptables -I INPUT -p tcp --dport 81 -j ACCEPT

iptables -I OUTPUT -p tcp --sport 80 -j ACCEPT

iptables -I OUTPUT -p tcp --sport 81 -j ACCEPT - [Рисунок 3](Images/3.png){ #fig:001 width=70% }.

Можно было бы также отключить фильтр командами:

iptables -F 

iptables -P INPUT ACCEPT iptables -P OUTPUT ACCEPT.

# Выполнение лабораторной работы

1. Войдём в систему с полученными учётными данными и убедимся, что SELinux работает в режиме enforcing политики targeted с помощью команд 

getenforce

sestatus - [Рисунок 4](Images/4.png){ #fig:001 width=70% }.

2. Обратимся с помощью браузера к веб-серверу, запущенному на нашем компьютере, и убедимся, что последний работает:

service httpd status - [Рисунок 5](Images/5.png){ #fig:001 width=70% }.

3. Найдём веб-сервер Apache в списке процессов, определим его контекст безопасности. Используем команду

ps auxZ | grep httpd - [Рисунок 6](Images/6.png){ #fig:001 width=70% }.

В нашем случае контекст безопасности unconfined_u:system_r:httpd_t.

4. Посмотрим текущее состояние переключателей SELinux для Apache с помощью команды

sestatus -bigrep httpd - [Рисунок 8](Images/8.png){ #fig:001 width=70% }.

Многие из переключателей находятся в положении «off».

5. Посмотрим статистику по политике с помощью команды

seinfo - [Рисунок 9](Images/9.png){ #fig:001 width=70% }.

Таким образом, определим множество пользователей, ролей и типов. Пользователей: 8, ролей: 14, типов: 4793.

6. Определим тип файлов и поддиректорий, находящихся в директории /var/www, с помощью команды

ls -lZ /var/www - [Рисунок 9.1](Images/9.1.png){ #fig:001 width=70% }.

7. Определим тип файлов, находящихся в директории /var/www/html:

ls -lZ /var/www/html - [Рисунок 9.2](Images/9.2.png){ #fig:001 width=70% }.

8. Определим круг пользователей, которым разрешено создание файлов в директории /var/www/html.

[Рисунок 10](Images/10.png){ #fig:001 width=70% }.

Видно, что только суперпользователь может создать файл в данной директории.

9. Создадим от имени суперпользователя (так как в дистрибутиве после установки только ему разрешена запись в директорию) html-файл
/var/www/html/test.html следующего содержания:

<html>

<body>test</body>

</html>

[Рисунок 11](Images/11.png){ #fig:001 width=70% }.

10. Проверим контекст созданного нами файла. 

[Рисунок 12](Images/12.png){ #fig:001 width=70% }.

Контекст, присваиваемый по умолчанию вновь созданным файлам в директории /var/www/html: unconfined_u:object_r:httpd_sys_content_t.

11. Обратитесь к файлу через веб-сервер, введя в браузере адрес

*http://127.0.0.1/test.html*

Убедимся, что файл был успешно отображён - [Рисунок 13](Images/13.png){ #fig:001 width=70% }.

12. Изучим справку man httpd_selinux и выясним, какие контексты файлов определены для httpd. Сопоставим их с типом файла
test.html.

ls -Z /var/www/html/test.html - [Рисунок 14](Images/14.png){ #fig:001 width=70% }.

Т.к. по умолчанию пользователи CentOS являются свободными (unconfined) от типа, созданному нами файлу test.html был сопоставлен SELinux, пользователь unconfined_u. Это первая часть контекста. Далее политика ролевого разделения доступа RBAC используется процессами, но не файлами, поэтому роли не имеют никакого значения для файлов. Роль object_r используется по умолчанию для файлов на «постоянных» носителях и на сетевых файловых системах. Тип httpd_sys_content_t позволяет процессу httpd получить доступ к файлу. Благодаря наличию последнего типа мы получили доступ к файлу при обращении к нему через браузер.

13. Изменим контекст файла /var/www/html/test.html с httpd_sys_content_t на любой другой, к которому процесс httpd не
должен иметь доступа, например, на samba_share_t:

chcon -t samba_share_t /var/www/html/test.html

ls -Z /var/www/html/test.html - [Рисунок 15](Images/15.png){ #fig:001 width=70% }.

Как можно видеть, контекст успешно сменился.

14. Попробуем ещё раз получить доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1/test.html. 

[Рисунок 16](Images/16.png){ #fig:001 width=70% }.

Мы получили сообщение об ошибке.

15. Проанализируем ситуацию, просмотрев log-файлы веб-сервера Apache, системный log-файл и audit.log при условии уже запущенных процессов setroubleshootd и audtd.

[Рисунок 17](Images/17.png){ #fig:001 width=70% }, [Рисунок 18](Images/18.png){ #fig:001 width=70% }, [Рисунок 19](Images/19.png){ #fig:001 width=70% }.

Исходя из log-файлов, мы можем заметить, что проблема в измененном контексте на шаге 13, т.к. процесс httpd не имеет доступа на samba_share_t. В системе оказались запущены процессы setroubleshootd и audtd, поэтому ошибки, связанные с измененным контекстом, также есть в файле /var/log/audit/audit.log.

16. Попробуем запустить веб-сервер Apache на прослушивание ТСР-порта 81 (а не 80, как рекомендует IANA и прописано в /etc/services). Для
этого в файле /etc/httpd/httpd.conf найдём строчку Listen 80 и заменим её на Listen 81.

[Рисунок 20](Images/20.png){ #fig:001 width=70% }.

17. Перезапустим веб-сервер Apache и попробуем обратиться к файлу через веб-сервер, введя в браузере firefox адрес *http://127.0.0.1/test.html*

[Рисунок 21](Images/21.png){ #fig:001 width=70% }.

Из того, что при запуске файла через браузер появилась ошибка, можно сделать предположение, что в списках портов, работающих с веб-сервером Apache, отсутствует порт 81.

18. Проанализируем лог-файлы:

tail -nl /var/log/messages - [Рисунок 22](Images/22.png){ #fig:001 width=70% }.

Во всех log-файлах появились записи, кроме /var/log/messages.

19. Выполним команду

semanage port -a -t http_port_t -р tcp 81

После этого проверим список портов командой

semanage port -l | grep http_port_t - [Рисунок 23](Images/23.png){ #fig:001 width=70% }.

Убедились, что порт 81 появился в списке.

20. Попробуем теперь запустить веб-сервер Apache еще раз.

[Рисунок 24](Images/24.png){ #fig:001 width=70% }.

21. Вернем контекст httpd_sys_content_t к файлу /var/www/html/test.html:

chcon –t httpd_sys_content_t /var/www/html/test.html - [Рисунок 25](Images/25.png){ #fig:001 width=70% }.

После этого вновь попробуем получить доступ к файлу через веб-сервер, введя в браузере firefox адрес *http://127.0.0.1:81/test.html*

[Рисунок 26](Images/26.png){ #fig:001 width=70% }

Увидели слово содержимое файла - слово «test».

22. Исправим обратно конфигурационный файл apache, вернув Listen 80. 

[Рисунок 27](Images/27.png){ #fig:001 width=70% }.

23. Удалим привязку http_port_t к 81 порту: semanage port –d –t http_port_t –p tcp 81. Данную команду выполнить невозможно на моей версии CentOS, поэтому получаем ошибку.

[Рисунок 28](Images/28.png){ #fig:001 width=70% }.

24. Удалим файл /var/www/html/test.html:

rm /var/www/html/test.html - [Рисунок 29](Images/29.png){ #fig:001 width=70% }.

# Выводы

В ходе выяполнения работы я развила навыки администрирования ОС Linux. Получила первое практическое знакомство с технологией SELinux. Проверила работу SELinux на практике совместно с веб-сервером Apache.


