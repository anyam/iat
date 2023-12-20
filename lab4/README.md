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

#### Импорт данных

Импорт DNS файла

``` r
dns <- read.csv ("dns.log", header = FALSE, sep = "\t", encoding = "UTF-8")
```

Импорт CSV файла

``` r
header <- read.csv ("header.csv")
```

#### Добавление пропущенных данных

``` r
colnames(dns) <- read.csv("header.csv", header = FALSE, skip = 1)$V1
```

#### Преобразование данных в нужный формат

``` r
field<-header[,1]
colnames(dns)<-field
dns$ts <- as.POSIXct(dns$ts, origin="1970-01-01")
```

#### Просмотр общей структуры данных с помощью функции `glimpse ()`

``` r
glimpse (dns)
```

    Rows: 427,935
    Columns: 24
    $ `ts `          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901…
    $ `uid `         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz…
    $ `id `          <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", …
    $ ``             <int> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ `proto `       <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255",…
    $ `trans_id `    <int> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, …
    $ `query `       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp",…
    $ `qclass `      <int> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187,…
    $ `qclass_name ` <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x…
    $ `qtype `       <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", …
    $ `qtype_name `  <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET",…
    $ `rcode `       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "…
    $ `rcode_name `  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", …
    $ `QR `          <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `AA `          <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-",…
    $ `TC RD `       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `RA `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `Z `           <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, …
    $ `answers `     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `TTLs `        <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1…
    $ `rejected `    <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ NA             <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ NA             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ ts             <dttm> 2012-03-16 16:30:05, 2012-03-16 16:30:15, 2012-03-16 1…

### Ответы на вопросы

#### 1. Сколько участников информационного обмена в

сети Доброй Организации?

``` r
member1 <- unique(dns$id.orig_h)
member2 <- unique(dns$id.resp_h)
member <- union(member1 , member2)
member %>% length()
```

    [1] 0

#### 2.Какое соотношение участников обмена внутри

сети и участников обращений к внешним ресурсам?

``` r
ip_gen <- c("10.", "100.([6-9]|1[0-1][0-9]|12[0-7]).", "172.((1[6-9])|(2[0-9])|(3[0-1])).", "192.168.")
ip_in <- member[grep(paste(ip_gen, collapse = "|"), member)]
count_ip_in <- sum(member %in% ip_in)
count_ip_ex <- length(member) - count_ip_in

mem_ratio <- count_ip_in / count_ip_ex
mem_ratio
```

    [1] NaN

#### 3. Найдите топ-10 участников сети, проявляющих

наибольшую сетевую активность

#### 4. Найдите топ-10 доменов, к которым обращаются

пользователи сети и соответственное количество обращений

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
