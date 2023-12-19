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
`library(nycflights13)`.

``` r
library(nycflights13)
```

    Warning: package 'nycflights13' was built under R version 4.3.2

### Ответы на вопросы

#### 1. Сколько встроенных в пакет nycflights13 датафреймов?

5 датафреймов: airlines, airports, flights, planes, weather

#### 2. Сколько строк в каждом датафрейме?

``` r
airlines %>% nrow ()
```

    [1] 16

``` r
airports %>% nrow ()
```

    [1] 1458

``` r
flights %>% nrow ()
```

    [1] 336776

``` r
planes %>% nrow ()
```

    [1] 3322

``` r
weather %>% nrow ()
```

    [1] 26115

#### 3. Сколько столбцов в каждом датафрейме?

``` r
airlines %>% ncol()
```

    [1] 2

``` r
airports %>% ncol ()
```

    [1] 8

``` r
flights %>% ncol ()
```

    [1] 19

``` r
planes %>% ncol ()
```

    [1] 9

``` r
weather %>% ncol ()
```

    [1] 15

#### 4. Как просмотреть примерный вид датафреймов?

#### 5. Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?

#### 6. Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

#### 7. Какой самый северный аэропорт?

#### 8. Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?

#### 9. Какие бортовые номера у самых старых самолётов?

#### 10. Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия)?

#### 11. Самолёты какой авиакомпании совершили больше всего вылетов в июне?

#### 12. Самолёты какой авиакомпании задерживались чаще других в 2013 году?
