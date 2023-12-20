Практика 4
================
Гренкова Анна БИСО-01-20

### Цель работы

1.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

1.  ОС Windows 11
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.0
4.  Пакет `dplyr`
5.  файл dns.log
6.  файл header.csv

## План

1.  Установить пакет `dplyr`
2.  Импортировать данные DNS
3.  Выполнить задания.

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

``` r
library(readr)
```

    Warning: package 'readr' was built under R version 4.3.2

#### Импорт данных

Импорт DNS файла

``` r
dns2 <- read_delim("dns.log", col_names = FALSE, delim = "\t")
```

    Rows: 427935 Columns: 23
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: "\t"
    chr (13): X2, X3, X5, X7, X9, X10, X11, X12, X13, X14, X15, X21, X22
    dbl  (5): X1, X4, X6, X8, X20
    lgl  (5): X16, X17, X18, X19, X23

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
dns1 <- head((dns2), n = nrow(dns2) - 2)
dns1
```

    # A tibble: 427,933 × 23
                X1 X2    X3       X4 X5       X6 X7       X8 X9    X10   X11   X12  
             <dbl> <chr> <chr> <dbl> <chr> <dbl> <chr> <dbl> <chr> <chr> <chr> <chr>
     1 1331901006. CWGt… 192.… 45658 192.…   137 udp   33008 "*\\… 1     C_IN… 33   
     2 1331901015. C36a… 192.…   137 192.…   137 udp   57402 "HPE… 1     C_IN… 32   
     3 1331901016. C36a… 192.…   137 192.…   137 udp   57402 "HPE… 1     C_IN… 32   
     4 1331901017. C36a… 192.…   137 192.…   137 udp   57402 "HPE… 1     C_IN… 32   
     5 1331901006. C36a… 192.…   137 192.…   137 udp   57398 "WPA… 1     C_IN… 32   
     6 1331901007. C36a… 192.…   137 192.…   137 udp   57398 "WPA… 1     C_IN… 32   
     7 1331901007. C36a… 192.…   137 192.…   137 udp   57398 "WPA… 1     C_IN… 32   
     8 1331901006. ClEZ… 192.…   137 192.…   137 udp   62187 "EWR… 1     C_IN… 32   
     9 1331901007. ClEZ… 192.…   137 192.…   137 udp   62187 "EWR… 1     C_IN… 32   
    10 1331901008. ClEZ… 192.…   137 192.…   137 udp   62187 "EWR… 1     C_IN… 32   
    # ℹ 427,923 more rows
    # ℹ 11 more variables: X13 <chr>, X14 <chr>, X15 <chr>, X16 <lgl>, X17 <lgl>,
    #   X18 <lgl>, X19 <lgl>, X20 <dbl>, X21 <chr>, X22 <chr>, X23 <lgl>

Импорт CSV файла

``` r
header <- read.csv ("header.csv")
```

#### Добавление пропущенных данных

``` r
colnames (dns1) <- read.csv ("header.csv", header = FALSE, skip = 1)$V1
```

#### Преобразование данных в нужный формат

``` r
field <- header [,1]
colnames (dns1) <- field
```

#### Просмотр общей структуры данных с помощью функции `glimpse ()`

``` r
dns1 %>% glimpse ()
```

    Rows: 427,933
    Columns: 23
    $ `ts `          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901…
    $ `uid `         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz…
    $ id.orig_h      <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", …
    $ id.orig_p      <dbl> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ id.resp_h      <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255",…
    $ id.resp_p      <dbl> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, …
    $ `proto `       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp",…
    $ `trans_id `    <dbl> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187,…
    $ `query `       <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x…
    $ `qclass `      <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", …
    $ `qclass_name ` <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET",…
    $ `qtype `       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "…
    $ `qtype_name `  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", …
    $ `rcode `       <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `rcode_name `  <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-",…
    $ `QR `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `AA `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `TC RD `       <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, …
    $ `RA `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `Z `           <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1…
    $ `answers `     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `TTLs `        <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `rejected `    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…

### Ответы на вопросы

#### 1. Сколько участников информационного обмена в

сети Доброй Организации?

``` r
member1 <- unique (dns1$`id.orig_h`)
member2 <- unique (dns1$`id.resp_h`)
member <- union (member1 , member2)
member %>% length ()
```

    [1] 1359

#### 2.Какое соотношение участников обмена внутри

сети и участников обращений к внешним ресурсам?

``` r
ip_gen <- c ("10.", "100.([6-9]|1[0-1][0-9]|12[0-7]).", "172.((1[6-9])|(2[0-9])|(3[0-1])).", "192.168.")
ip_in <- member [grep (paste (ip_gen, collapse = "|"), member)]
count_ip_in <- sum (member %in% ip_in)
count_ip_ex <- length (member) - count_ip_in

mem_ratio <- count_ip_in / count_ip_ex
mem_ratio
```

    [1] 15.57317

#### 3. Найдите топ-10 участников сети, проявляющих

наибольшую сетевую активность

``` r
top_10 <- dns1 %>% group_by (id.orig_h) %>% summarise (activ = n ()) %>% arrange(desc (activ))

top_10 %>% head(10)
```

    # A tibble: 10 × 2
       id.orig_h       activ
       <chr>           <int>
     1 10.10.117.210   75943
     2 192.168.202.93  26522
     3 192.168.202.103 18121
     4 192.168.202.76  16978
     5 192.168.202.97  16176
     6 192.168.202.141 14967
     7 10.10.117.209   14222
     8 192.168.202.110 13372
     9 192.168.203.63  12148
    10 192.168.202.106 10784

#### 4. Найдите топ-10 доменов, к которым обращаются

пользователи сети и соответственное количество обращений

``` r
top <- dns1 %>% group_by (`query `) %>% summarise (req_count = n ()) %>% arrange (desc (req_count)) 

top %>% head(10)
```

    # A tibble: 10 × 2
       `query `                                                            req_count
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x0…     10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7247
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569

#### 5. Опеределите базовые статистические

характеристики (функция summary() ) интервала времени между
последовательным обращениями к топ-10 доменам.

#### 6. Часто вредоносное программное обеспечение

использует DNS канал в качестве канала управления, периодически
отправляя запросы на подконтрольный злоумышленникам DNS сервер. По
периодическим запросам на один и тот же домен можно выявить скрытый DNS
канал. Есть ли такие IP адреса в исследуемом датасете?

### Обогащение данных

#### Определите местоположение (страну, город) и

организацию-провайдера для топ-10 доменов.
