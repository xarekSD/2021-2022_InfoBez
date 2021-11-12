---
# Front matter
title: "Отчет по лабораторной работе 5"
subtitle: "Дисциплина: Информационная безопасность"
author: "Савченков Дмитрий Андреевич, НПИбд-02-18"

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

Изучение механизмов изменения идентификаторов, применения SetUID- и Sticky-битов. Получение практических навыков работы в консоли с дополнительными 
атрибутами. Рассмотрение работы механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.

# Выполнение лабораторной работы

1. Создание программы

1.1. Вошел в систему от имени пользователя guest. 

1.2. Создал программу *simpleid.c* по шаблону из методички. (рис. [-@fig:001])

![Программа *simpleid.c*](image/1.png){ #fig:001 width=70% }

1.3. Скомплилировал программу и убедилась, что файл программы создан: `gcc simpleid.c -o simpl`. (рис. [-@fig:002])

1.4. Выполнил программу simpleid: `./simpl`. (рис. [-@fig:002])

1.5. Выполнил системную программу id: `id`. (рис. [-@fig:002]) Полученный мной результат совпадает с данными предыдущего пункта задания.

![Компиляция и выполнение программы simpleid](image/2.png){ #fig:002 width=70% }

1.6. Усложнил программу, добавив вывод действительных идентификаторов согласно шаблону из методички. Для получившейся программы оставил название simpleid.c. 
(рис. [-@fig:003])

![Программа *simpleid2.c*](image/3.png){ #fig:003 width=70% }

1.7. Скомпилировал и запустил simpleid.c: `gcc simpleid.c -o simpl` и `./simpl`. (рис. [-@fig:004])

![Компиляция и выполнение программы simpleid2](image/4.png){ #fig:004 width=70% }

1.8. От имени суперпользователя выполнил команды: `chown root:guest /home/guest/simpl` и `chmod u+s /home/guest/simpl`. (рис. [-@fig:005])

1.9. Повысил временно свои права с помощью su. (рис. [-@fig:005]) Первая команда меняет владельца файла, а вторая добавляет SetUID-бит.

1.10. Выполнил проверку правильности установки новых атрибутов и смены владельца файла simpl: `ls -l simpl`. (рис. [-@fig:005])

1.11. Запустил simpl и id: `./simpl` и `id`. (рис. [-@fig:005])

![Смена пользователя. Установка SetUID-бита. Выполнение программы simpl](image/5.png){ #fig:005 width=70% }

1.12. Проделал то же самое относительно SetGID-бита. (рис. [-@fig:006])

![Установка SetGID-бита. Выполнение программы simpl](image/6.png){ #fig:006 width=70% }

1.13. Создал программу readfile.c по шаблону из методички. (рис. [-@fig:007])

![Программа *readfile.c*](image/7.png){ #fig:007 width=70% }

1.14. Откомпилировал её: `gcc readfile.c -o readfile`. (рис. [-@fig:008])

1.15. Сменил владельца у файла readfile.c и изменил права так, чтобы только суперпользователь (root) мог прочитать его, a guest не мог. (рис. [-@fig:008])

![Работа с программой *readfile.c*](image/8.png){ #fig:008 width=70% }

1.16. Проверил, что пользователь guest не может прочитать файл readfile.c. (рис. [-@fig:009])

![Запрет на чтение программы *readfile.c* для guest](image/9.png){ #fig:009 width=70% }

1.17. Сменил у программы readfile владельца (рис. [-@fig:008]) и установил SetUID-бит (рис. [-@fig:010]).

![Установка SetUID-бита на программу readfile](image/10.png){ #fig:010 width=70% }

1.18. Проверил, может ли программа readfile прочитать файл *readfile.c*. (рис. [-@fig:011])

![Программа readfile читает *readfile.c*](image/11.png){ #fig:011 width=70% }

1.19. Проверил, может ли программа readfile прочитать файл */etc/shadow*. (рис. [-@fig:012])

![Программа readfile читает */etc/shadow*](image/12.png){ #fig:012 width=70% }


2. Исследование Sticky-бита

2.1. Выяснил, установлен ли атрибут Sticky на директории /tmp, для чего выполнил команду: `ls -l / | grep tmp`. (рис. [-@fig:013])

2.2. От имени пользователя guest создал файл file01.txt в директории /tmp со словом test: `echo "test" > /tmp/file01.txt`. (рис. [-@fig:013])

2.3. Просмотрел атрибуты у только что созданного файла и разрешил чтение и запись для категории пользователей «все остальные»: `ls -l /tmp/file01.txt`, 
`chmod o+rw /tmp/file01.txt` и `ls -l /tmp/file01.txt`. (рис. [-@fig:013])

![Исследование Sticky-бита от имени guest](image/13.png){ #fig:013 width=70% }

2.4. От пользователя guest2 (не являющегося владельцем) попробовал прочитать файл /tmp/file01.txt: `cat /tmp/file01.txt`. (рис. [-@fig:014])

2.5. От пользователя guest2 попробовал дозаписать в файл */tmp/file01.txt* слово test2 командой: `echo "test2" >> /tmp/file01.txt`. (рис. [-@fig:014]) 
Операция прошла успешно.

2.6. Проверил содержимое файла командой: `cat /tmp/file01.txt`. (рис. [-@fig:014])

2.7. От пользователя guest2 попробовал записать в файл */tmp/file01.txt* слово test3, стерев при этом всю имеющуюся в файле информацию командой:
`echo "test3" > /tmp/file01.txt`. (рис. [-@fig:014]) Операция прошла успешно.

2.8. Проверил содержимое файла командой: `cat /tmp/file01.txt`. (рис. [-@fig:014])

2.9. От пользователя guest2 попробовал удалить файл */tmp/file01.txt* командой: `rm /tmp/fileOl.txt`. (рис. [-@fig:014]) Операция была не позволена.

![Работа с *file01.txt* от имени guest2 при наличии Sticky-бита](image/14.png){ #fig:014 width=70% }

2.10. Повысил свои права до суперпользователя следующей командой: `su -`, и выполнил после этого команду, снимающую атрибут t (Sticky-бит) с директории 
*/tmp*: `chmod -t /tmp`. (рис. [-@fig:015])

2.11. Покинул режим суперпользователя командой: `exit`. (рис. [-@fig:015])

![Снятие Sticky-бита с */tmp*](image/15.png){ #fig:015 width=70% }

2.12. От пользователя guest2 проверил, что атрибута t у директории */tmp* нет: `ls -l / | grep tmp`. (рис. [-@fig:016])

2.13. Повторил предыдущие шаги. (рис. [-@fig:016]) Теперь удалось удалить файл.

![Работа с *file01.txt* от имени guest2 без Sticky-бита](image/16.png){ #fig:016 width=70% }

2.14. Да, мне удалось удалить файл от имени пользователя, не являющегося его владельцем.

2.15. Повысил свои права до суперпользователя и вернул атрибут t на директорию /tmp: `su -`, `chmod +t /tmp` и `exit`. (рис. [-@fig:017])

![Возвращение Sticky-бита на */tmp*](image/17.png){ #fig:017 width=70% }

# Выводы

Изучил механизмы изменения идентификаторов, применения SetUID- и Sticky-битов. Получил практические навыки работы в консоли с дополнительными атрибутами. 
Рассмотрел работу механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.

# Список литературы{.unnumbered}

1. Кулябов Д. С., Королькова А. В., Геворкян М. Н. Информационная безопасность компьютерных сетей. Лабораторная работа № 5. Дискреционное разграничение прав 
в Linux. Исследование влияния дополнительных атрибутов