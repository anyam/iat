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

С помощью команды функции glimpse()

``` r
airlines %>% glimpse()
```

    Rows: 16
    Columns: 2
    $ carrier <chr> "9E", "AA", "AS", "B6", "DL", "EV", "F9", "FL", "HA", "MQ", "O…
    $ name    <chr> "Endeavor Air Inc.", "American Airlines Inc.", "Alaska Airline…

#### 5. Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?

``` r
airlines %>%
  nrow()
```

    [1] 16

#### 6. Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

``` r
flights %>% filter (origin=='JFK', month==5) %>% nrow()
```

    [1] 9397

#### 7. Какой самый северный аэропорт?

``` r
airports %>% filter(lat == max(lat))
```

    # A tibble: 1 × 8
      faa   name                      lat   lon   alt    tz dst   tzone
      <chr> <chr>                   <dbl> <dbl> <dbl> <dbl> <chr> <chr>
    1 EEN   Dillant Hopkins Airport  72.3  42.9   149    -5 A     <NA> 

#### 8. Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?

``` r
airports %>% 
  filter(alt == max(alt))
```

    # A tibble: 1 × 8
      faa   name        lat   lon   alt    tz dst   tzone         
      <chr> <chr>     <dbl> <dbl> <dbl> <dbl> <chr> <chr>         
    1 TEX   Telluride  38.0 -108.  9078    -7 A     America/Denver

#### 9. Какие бортовые номера у самых старых самолётов?

``` r
planes %>% arrange(year) %>% select(tailnum) %>% head(5)
```

    # A tibble: 5 × 1
      tailnum
      <chr>  
    1 N381AA 
    2 N201AA 
    3 N567AA 
    4 N378AA 
    5 N575AA 

#### 10. Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия)?

``` r
airports %>% filter (name == 'John F Kennedy Intl') 
```

    # A tibble: 1 × 8
      faa   name                  lat   lon   alt    tz dst   tzone           
      <chr> <chr>               <dbl> <dbl> <dbl> <dbl> <chr> <chr>           
    1 JFK   John F Kennedy Intl  40.6 -73.8    13    -5 A     America/New_York

``` r
weather %>% filter (origin == 'JFK' & month == 9) %>% 
  group_by (origin) %>% summarise (ave_tempreture_F = mean (temp, na.rm = TRUE)) %>% mutate(ave_tempreture_C = 5 / 9 * (ave_tempreture_F - 32))
```

    # A tibble: 1 × 3
      origin ave_tempreture_F ave_tempreture_C
      <chr>             <dbl>            <dbl>
    1 JFK                66.9             19.4

#### 11. Самолёты какой авиакомпании совершили больше всего вылетов в июне?

``` r
flights %>% group_by (carrier) %>% filter (month == 6) %>%  summarize (flights = n()) %>% arrange (desc (flights)) %>% head (1)
```

    # A tibble: 1 × 2
      carrier flights
      <chr>     <int>
    1 UA         4975

#### 12. Самолёты какой авиакомпании задерживались чаще других в 2013 году?

``` r
flights %>% group_by (carrier) %>% filter (year == 2013)  %>% summarize (times = sum (arr_delay > 0, na.rm = TRUE)) %>% arrange (desc (times)) %>% head (1)
```

    # A tibble: 1 × 2
      carrier times
      <chr>   <int>
    1 EV      24484

## Оценка результата

Установлен пакеты dplyr и nycflights13. Получены ответы на вопросы
задания. Проведена работа с использованием функций пакета dplyr и
датафреймов пакета nycflights13.

## Вывод

Изучены функции пакета dplyr. Получены практические навыки работы с
изученными функциями и датафреймами пакета nycflights13.
