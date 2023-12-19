Практика 3
================
Гренкова Анна БИСО-01-20

## Основы обработки данных с помощью R с использованием tidyverse

### Цель работы

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных.
2.  Закрепить знания основных функций обработки данных экосистемы
    `tidyverse` языка R.
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции `select()`, `filter()`, `mutate()`,
    `arrange()`, `group_by()`.

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.2.2
4.  Пакет `dplyr`
5.  Пакет `nycflights13`

## План

1.  Установить пакет `dplyr`
2.  Установить пакет `nycflights13`
3.  Выполнить задания.

## Ход работы

### Шаг 0

Установка пакета `dplyr` с помощью команды `install.packages("dplyr")`.

    install.packages ("dplyr")

Подключение пакета к проекту с помощью команды мощью `library (dplyr)`.

``` r
library (dplyr)
```

    Warning: package 'dplyr' was built under R version 4.3.2


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

Установка пакета `nycflights13` с помощью команды
`install.packages("nycflights13")`.

    install.packages("nycflights13")

Подключение пакета к проекту с помощью команды с помощью
`library(nycflights1)`.

``` r
library(nycflights13)
```

    Warning: package 'nycflights13' was built under R version 4.3.2
