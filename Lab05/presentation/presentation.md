---
## Front matter
lang: ru-RU
title: "Отчет по лабораторной работе 5"
author: |
	Savchenkov Dmitriy Andreevich\inst{1}
institute: |
	\inst{1}RUDN University, Moscow, Russian Federation
date: 12 November, 2021 Moscow, Russian Federation

## Formatting
toc: false
slide_level: 2
theme: metropolis
header-includes: 
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
aspectratio: 43
section-titles: true
---

# **Цель выполнения лабораторной работы**

## Цель

Изучение механизмов изменения идентификаторов, применения SetUID- и Sticky-битов. Получение практических навыков работы в консоли с дополнительными 
атрибутами. Рассмотрение работы механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.

# **Результаты выполнения лабораторной работы**

# **Создание программы**

## Программа *simpleid2.c*

![](image/3.png){ #fig:001 width=45% }
![](image/4.png){ #fig:002 width=45% }

## SetUID-бит

![](image/5.png){ #fig:003 width=90% }

## SetGID-бит

![](image/6.png){ #fig:004 width=90% }

## Программа *readfile.c*, *readfile*

![](image/8.png){ #fig:005 width=45% }
![](image/10.png){ #fig:006 width=45% }

# **Исследование Sticky-бита**

## Работа с *file01.txt* от имени guest2 при наличии Sticky-бита на */tmp*

![](image/14.png){ #fig:007 width=90% }

## Работа с *file01.txt* от имени guest2 без Sticky-бита на */tmp*

![](image/16.png){ #fig:008 width=70% }

# **Выводы по лабораторной работе**

## Выводы

Изучил механизмы изменения идентификаторов, применения SetUID- и Sticky-битов. Получил практические навыки работы в консоли с дополнительными атрибутами. 
Рассмотрел работу механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.

## {.standout}

Спасибо за внимание!
