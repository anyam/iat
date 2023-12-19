Практика 2
================
Гренкова Анна БИСО-01-20

# Основы обработки данных с помощью R с использованием пакета dplyr.

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных;
2.  Закрепить знания базовых типов данных языка R;
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции `select()`, `filter()`, `mutate()`,
    `arrange()`, `group_by()`.

## Исходные данные

1.  ОС Windows 11
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.0
4.  Пакет `dplyr`

## План

1.  Установить пакет `dplyr`
2.  Выполнить задания на наборе данных `starwars` и ответить на
    поставленные вопросы.

## Ход работы

### Шаг 0

Установка пакета `dplyr` с помощью команды `install.packages("dplyr")`.

    install.packages("dplyr")

Подключение пакета к проекту с помощью команды мощью `library(dplyr)`.

``` r
library(dplyr)
```

    Warning: package 'dplyr' was built under R version 4.3.2


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

### Ответы на вопросы

#### Сколько строк в датафрейме?

``` r
starwars %>% nrow()
```

    [1] 87

The `echo: false` option disables the printing of code (only output is
displayed).
