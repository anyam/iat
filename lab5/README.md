Практика 5
================
Гренкова Анна БИСО-01-20

# Исследование информации о состоянии беспроводных сетей

## Цель работы

1.  Получить знания о методах исследования радиоэлектронной обстановки.
2.  Составить представление о механизмах работы Wi-Fi сетей на канальном
    и сетевом уровне модели OSI.
3.  Закрепить практические навыки использования языка программирования
    для обработки данных
4.  Закрепить знания основных функций обработки данных экосистемы
    `tidyverse` языка R

## Исходные данные

1.  ОС Windows 11
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.0
4.  Файл `mir.csv-01.csv`

## План

1.  Импортировать данные из файла `mir.csv-01.csv`
2.  Провести анализ точек доступа
3.  Провести анализ данных клиенто.
4.  Выполнить задания.

## Ход работы

### Шаг 0

#### Установка пакета `dplyr` с помощью команды `library(dplyr)`

``` r
library(dplyr)
```

    Warning: package 'dplyr' was built under R version 4.3.2


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

#### Импорт данных

``` r
mir <- read.csv("mir.csv-01.csv", nrows = 167)
```

``` r
mir2 <- read.csv("mir.csv-01.csv", skip = 169)
```

#### Преобразование данных

``` r
mir <- mir %>%  mutate_at(vars(BSSID, Privacy, Cipher, Authentication, LAN.IP, ESSID), trimws) %>% mutate_at(vars(BSSID, Privacy, Cipher, Authentication, LAN.IP, ESSID), na_if, "") %>% mutate_at(vars(First.time.seen, Last.time.seen), as.POSIXct, format = "%Y-%m-%d %H:%M:%S")
```

``` r
mir2 <- mir2 %>% 
  mutate_at(vars(Station.MAC, BSSID, Probed.ESSIDs), trimws) %>%
  mutate_at(vars(Station.MAC, BSSID, Probed.ESSIDs), na_if, "")

mir2 <- mir2 %>% mutate_at(vars(First.time.seen, Last.time.seen), as.POSIXct, format = "%Y-%m-%d %H:%M:%S") %>% mutate_at(vars(Power, X..packets), as.integer) %>% filter(!is.na(BSSID))
```

    Warning: There were 2 warnings in `mutate()`.
    The first warning was:
    ℹ In argument: `Power = .Primitive("as.integer")(Power)`.
    Caused by warning:
    ! NAs introduced by coercion
    ℹ Run `dplyr::last_dplyr_warnings()` to see the 1 remaining warning.

### Анализ данных
