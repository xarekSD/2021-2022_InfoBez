---
# Front matter
title: "Отчет по лабораторной работе 6"
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

Развить навыки администрирования ОС Linux. Получить первое практическое знакомство с технологией SELinux1.
Проверить работу SELinx на практике совместно с веб-сервером Apache.

# Подготовка лабораторного стенда:

1. В конфигурационном файле /etc/httpd/httpd.conf  задал параметр ServerName. (рис. -@fig:001). 

![Параметр ServerName](image/2.png){ #fig:001 width=60% height=60% }

2. Также проследил, чтобы пакетный фильтр был отключён или в своей рабочей конфигурации позволял подключаться к 80-у и 81-у портам протокола tcp. Отключил фильтр командами: iptables -F, iptables -P INPUT ACCEPT iptables -P OUTPUT ACCEPT. Так же добавил разрешающие правила.
(рис. -@fig:002), (рис. -@fig:003). 

![Отключение фильтра ](image/3.1.png){ #fig:002 width=70% height=70% }

![Отключение фильтра ](image/3.2.png){ #fig:003 width=70% height=70% }

# Выполнение лабораторной работы:

1. Вошел в систему с полученными учётными данными и убедился, что SELinux работает в режиме enforcing политики targeted с помощью команд getenforce и sestatus.(рис. -@fig:005). 

![Проверка режима и политики](image/4.png){ #fig:005 width=70% height=70% }

2. Обратился с помощью браузера к веб-серверу, запущенному на компьютере, и убедился, что последний работает: service httpd status(рис. -@fig:006), (рис. -@fig:007).

![Проверка через браузер](image/5.png){ #fig:006 width=70% height=70% }

![Проверка статуса](image/6.png){ #fig:007 width=70% height=70% }

3. Нашел веб-сервер Apache в списке процессов, определил его контекст безопасности. (рис. -@fig:008). 

![веб-сервер Apache](image/6.png){ #fig:008 width=70% height=70% }

4. Посмотрел текущее состояние переключателей SELinux для Apache с помощью команды: sestatus -bigrep httpd. Обратил внимание, что многие из них находятся в положении «off». (рис. -@fig:009). 

![Просмотр переключателей SELinux для Apache](image/7.png){ #fig:009 width=70% height=70% }

5. Посмотрел статистику по политике с помощью команды seinfo, также определил множество пользователей(8), ролей(14), типов(4959) (рис. -@fig:010). 

![Статистика](image/8.png){ #fig:010 width=70% height=70% }

6. Определил тип файлов и поддиректорий, находящихся в директории /var/www, с помощью команды: ls -lZ /var/www. 

7. Определил тип файлов, находящихся в директории /var/www/html: ls -lZ /var/www/html. 

8. Определил круг пользователей, которым разрешено создание файлов в директории /var/www/html. (рис. -@fig:011). 

![Определение типов файлов и круг пользователей](image/9.png){ #fig:011 width=70% height=70% }

9. Создал от имени суперпользователя (так как в дистрибутиве после установки только ему разрешена запись в директорию) html-файл /var/www/html/test.html(рис. -@fig:012). 

![Создание файла](image/10.png){ #fig:012 width=70% height=70% }

10. Проверил контекст созданного файла. httpd_sys_content_t (рис. -@fig:013). 

![Проверка](image/11.png){ #fig:013 width=70% height=70% }

11. Обратился к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1/test.html. Убедился, что файл был успешно отображён. (рис. -@fig:014). 

![Получение доступа к файлу через браузер](image/12.png){ #fig:014 width=50% height=50% }

12. Проверил контекст файла командой: ls -Z /var/www/html/test.html (рис. -@fig:015). 

13. Изменил контекст файла /var/www/html/test.html с httpd_sys_content_t на samba_share_t. После этого проверил, что контекст поменялся. (рис. -@fig:015). 

![Изменение контекста, проверка](image/13.png){ #fig:015 width=50% height=50% }

14. Попробовал ещё раз получить доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1/test.html. Получил сообщение об ошибке. (рис. -@fig:016).

![Получение доступа к файлу через браузер](image/14.png){ #fig:016 width=50% height=50% }

15. Проанализировал ситуацию. Файл не был отображён потому что мы изменили контекст файла. Просмотрел log-файлы веб-сервера Apache. Также просмотрел системный лог-файл: tail /var/log/messages (рис. -@fig:017). 

![Анализ ситуации](image/15.png){ #fig:017 width=50% height=50% }

16. Попробовал запустить веб-сервер Apache на прослушивание ТСР-порта 81 (а не 80, как рекомендует IANA и прописано в /etc/services). Для этого в файле /etc/httpd/httpd.conf нашел строчку Listen 80 и заменил её на Listen 81.(рис. -@fig:018). 

![Изменеие порта 80 на 81](image/16.png){ #fig:018 width=50% height=50% }

17. Проанализиировал лог-файлы. Просмотрел файлы /var/log/http/error_log, /var/log/http/access_log и /var/log/audit/audit.log. (рис. -@fig:019).

![Анализ и просмотр лог-файлов](image/17.png){ #fig:019 width=50% height=50% }

18. Выполнил команду: semanage port -a -t http_port_t -р tcp 81. После этого проверил список портов командой: semanage port -l | grep http_port_t. Убедился, что порт 81 появился в списке. (рис. -@fig:023).

![Выполнение и проверка](image/18.png){ #fig:023 width=50% height=50% }

19. Вернул контекст httpd_sys_cоntent__t к файлу /var/www/html/test.html: chcon -t httpd_sys_content_t /var/www/html/test.html. После этого попробовал получить доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1:81/test.html. Увидели содержимое файла — слово «test». (рис. -@fig:024), (рис. -@fig:025).

![Возвращение контекста](image/19.1.png){ #fig:024 width=50% height=50% }

![Получение доступа к файлу через браузер](image/19.2.png){ #fig:025 width=50% height=50% }

20. Исправил обратно конфигурационный файл apache, вернув Listen80. (рис. -@fig:026).

![Исправленный файл apache](image/20.png){ #fig:026 width=50% height=50% }

21. Удалил привязку http_port_t к 81 порту. (рис. -@fig:027).

![Удаление привязки к 81 порту](image/21.png){ #fig:027 width=50% height=50% }

22. Удалил файл /var/www/html/test.html. (рис. -@fig:028).

![Удаление файла /var/www/html/test.html](image/22.png){ #fig:028 width=50% height=50% }

# Выводы

Развил навыки администрирования ОС Linux. Получил первое практическое знакомство с технологией SELinux. Проверил работу SELinx на практике совместно с веб-сервером Apache.

# Список литературы{.unnumbered}

1. Кулябов Д. С., Королькова А. В., Геворкян М. Н. Информационная безопасность компьютерных сетей. Лабораторная работа № 5. Дискреционное разграничение прав 
в Linux. Исследование влияния дополнительных атрибутов