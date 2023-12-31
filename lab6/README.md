Практика 6
================
Гренкова Анна БИСО-01-20

## Цель

1.  Закрепить навыки исследования данных журнала Windows Active
    Directory
2.  Изучить структуру журнала системы Windows Active Directory
3.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
4.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R

## Исходные данные

1.  ОС Windows 11
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.0
4.  Пакет dplyr

## План

1.  Установить пакет ‘dplyr’
2.  Импорт и подготовка данных
3.  Анализ данных

## Ход работы

### Шаг 0

#### Установка пакетов `dplyr`, `jsonlite`, `tidyr`, `xml2`, `rvest`

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
library(jsonlite)
```

    Warning: package 'jsonlite' was built under R version 4.3.2

``` r
library(tidyr)
```

    Warning: package 'tidyr' was built under R version 4.3.2

``` r
library(xml2)
```

    Warning: package 'xml2' was built under R version 4.3.2

``` r
library(rvest)
```

    Warning: package 'rvest' was built under R version 4.3.2

#### Импорт данных

``` r
url_yand <- "https://storage.yandexcloud.net/iamcth-data/dataset.tar.gz"
download.file(url_yand, destfile = tf <- tempfile(fileext = ".tar.gz"), mode = "wb")
temp_dir <- tempdir()
untar(tf, exdir = temp_dir)
json_files <- list.files(temp_dir, pattern="\\.json$", full.names = TRUE, recursive = TRUE)
data <- stream_in(file(json_files))
```

    opening file input connection.


     Found 500 records...
     Found 1000 records...
     Found 1500 records...
     Found 2000 records...
     Found 2500 records...
     Found 3000 records...
     Found 3500 records...
     Found 4000 records...
     Found 4500 records...
     Found 5000 records...
     Found 5500 records...
     Found 6000 records...
     Found 6500 records...
     Found 7000 records...
     Found 7500 records...
     Found 8000 records...
     Found 8500 records...
     Found 9000 records...
     Found 9500 records...
     Found 10000 records...
     Found 10500 records...
     Found 11000 records...
     Found 11500 records...
     Found 12000 records...
     Found 12500 records...
     Found 13000 records...
     Found 13500 records...
     Found 14000 records...
     Found 14500 records...
     Found 15000 records...
     Found 15500 records...
     Found 16000 records...
     Found 16500 records...
     Found 17000 records...
     Found 17500 records...
     Found 18000 records...
     Found 18500 records...
     Found 19000 records...
     Found 19500 records...
     Found 20000 records...
     Found 20500 records...
     Found 21000 records...
     Found 21500 records...
     Found 22000 records...
     Found 22500 records...
     Found 23000 records...
     Found 23500 records...
     Found 24000 records...
     Found 24500 records...
     Found 25000 records...
     Found 25500 records...
     Found 26000 records...
     Found 26500 records...
     Found 27000 records...
     Found 27500 records...
     Found 28000 records...
     Found 28500 records...
     Found 29000 records...
     Found 29500 records...
     Found 30000 records...
     Found 30500 records...
     Found 31000 records...
     Found 31500 records...
     Found 32000 records...
     Found 32500 records...
     Found 33000 records...
     Found 33500 records...
     Found 34000 records...
     Found 34500 records...
     Found 35000 records...
     Found 35500 records...
     Found 36000 records...
     Found 36500 records...
     Found 37000 records...
     Found 37500 records...
     Found 38000 records...
     Found 38500 records...
     Found 39000 records...
     Found 39500 records...
     Found 40000 records...
     Found 40500 records...
     Found 41000 records...
     Found 41500 records...
     Found 42000 records...
     Found 42500 records...
     Found 43000 records...
     Found 43500 records...
     Found 44000 records...
     Found 44500 records...
     Found 45000 records...
     Found 45500 records...
     Found 46000 records...
     Found 46500 records...
     Found 47000 records...
     Found 47500 records...
     Found 48000 records...
     Found 48500 records...
     Found 49000 records...
     Found 49500 records...
     Found 50000 records...
     Found 50500 records...
     Found 51000 records...
     Found 51500 records...
     Found 52000 records...
     Found 52500 records...
     Found 53000 records...
     Found 53500 records...
     Found 54000 records...
     Found 54500 records...
     Found 55000 records...
     Found 55500 records...
     Found 56000 records...
     Found 56500 records...
     Found 57000 records...
     Found 57500 records...
     Found 58000 records...
     Found 58500 records...
     Found 59000 records...
     Found 59500 records...
     Found 60000 records...
     Found 60500 records...
     Found 61000 records...
     Found 61500 records...
     Found 62000 records...
     Found 62500 records...
     Found 63000 records...
     Found 63500 records...
     Found 64000 records...
     Found 64500 records...
     Found 65000 records...
     Found 65500 records...
     Found 66000 records...
     Found 66500 records...
     Found 67000 records...
     Found 67500 records...
     Found 68000 records...
     Found 68500 records...
     Found 69000 records...
     Found 69500 records...
     Found 70000 records...
     Found 70500 records...
     Found 71000 records...
     Found 71500 records...
     Found 72000 records...
     Found 72500 records...
     Found 73000 records...
     Found 73500 records...
     Found 74000 records...
     Found 74500 records...
     Found 75000 records...
     Found 75500 records...
     Found 76000 records...
     Found 76500 records...
     Found 77000 records...
     Found 77500 records...
     Found 78000 records...
     Found 78500 records...
     Found 79000 records...
     Found 79500 records...
     Found 80000 records...
     Found 80500 records...
     Found 81000 records...
     Found 81500 records...
     Found 82000 records...
     Found 82500 records...
     Found 83000 records...
     Found 83500 records...
     Found 84000 records...
     Found 84500 records...
     Found 85000 records...
     Found 85500 records...
     Found 86000 records...
     Found 86500 records...
     Found 87000 records...
     Found 87500 records...
     Found 88000 records...
     Found 88500 records...
     Found 89000 records...
     Found 89500 records...
     Found 90000 records...
     Found 90500 records...
     Found 91000 records...
     Found 91500 records...
     Found 92000 records...
     Found 92500 records...
     Found 93000 records...
     Found 93500 records...
     Found 94000 records...
     Found 94500 records...
     Found 95000 records...
     Found 95500 records...
     Found 96000 records...
     Found 96500 records...
     Found 97000 records...
     Found 97500 records...
     Found 98000 records...
     Found 98500 records...
     Found 99000 records...
     Found 99500 records...
     Found 1e+05 records...
     Found 100500 records...
     Found 101000 records...
     Found 101500 records...
     Found 101904 records...
     Imported 101904 records. Simplifying...

    closing file input connection.

#### Преобразование данных

``` r
data <- data %>%
  mutate(`@timestamp` = as.POSIXct(`@timestamp`, format = "%Y-%m-%dT%H:%M:%OSZ", tz = "UTC")) %>%
  rename(timestamp = `@timestamp`, metadata = `@metadata`)
```

#### Общая структура

``` r
data %>% glimpse()
```

    Rows: 101,904
    Columns: 9
    $ timestamp <dttm> 2019-10-20 20:11:06, 2019-10-20 20:11:07, 2019-10-20 20:11:…
    $ metadata  <df[,4]> <data.frame[26 x 4]>
    $ event     <df[,4]> <data.frame[26 x 4]>
    $ log       <df[,1]> <data.frame[26 x 1]>
    $ message   <chr> "A token right was adjusted.\n\nSubject:\n\tSecurity ID:\…
    $ winlog    <df[,16]> <data.frame[26 x 16]>
    $ ecs       <df[,1]> <data.frame[26 x 1]>
    $ host      <df[,1]> <data.frame[26 x 1]>
    $ agent     <df[,5]> <data.frame[26 x 5]>

### Анализ

#### 1. Раскройте датафрейм избавившись от вложенных датафреймов. Для обнаружения таких можно использовать функцию dplyr::glimpse() , а для раскрытия вложенности – tidyr::unnest() . Обратите внимание, что при раскрытии теряются внешние названия колонок – это можно предотвратить если использовать параметр tidyr::unnest(…, names_sep = ) .

``` r
data$metadata %>% glimpse ()
```

    Rows: 101,904
    Columns: 4
    $ beat    <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlo…
    $ type    <chr> "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc"…
    $ version <chr> "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0",…
    $ topic   <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlo…

``` r
data$event %>% glimpse ()
```

    Rows: 101,904
    Columns: 4
    $ created <chr> "2019-10-20T20:11:09.988Z", "2019-10-20T20:11:09.988Z", "2019-…
    $ kind    <chr> "event", "event", "event", "event", "event", "event", "event",…
    $ code    <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, 10, 7, 7, 7, 4689,…
    $ action  <chr> "Token Right Adjusted Events", "Sensitive Privilege Use", "Pro…

``` r
data$log %>% glimpse ()
```

    Rows: 101,904
    Columns: 1
    $ level <chr> "information", "information", "information", "information", "inf…

``` r
data$winlog %>% glimpse ()
```

    Rows: 101,904
    Columns: 16
    $ event_data    <df[,234]> <data.frame[26 x 234]>
    $ event_id      <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, 10, 7, …
    $ provider_name <chr> "Microsoft-Windows-Security-Auditing", "Microsoft-Window…
    $ api           <chr> "wineventlog", "wineventlog", "wineventlog", "wineventlo…
    $ record_id     <int> 50588, 104875, 226649, 153525, 163488, 153526, 134651, 2…
    $ computer_name <chr> "HR001.shire.com", "HFDC01.shire.com", "IT001.shire.com"…
    $ process       <df[,2]> <data.frame[26 x 2]>
    $ keywords      <list> "Audit Success", "Audit Failure", <NULL>, <NULL>, <NULL>…
    $ provider_guid <chr> "{54849625-5478-4994-a5ba-3e3b0328c30d}", "{54849625-…
    $ channel       <chr> "security", "Security", "Microsoft-Windows-Sysmon/Opera…
    $ task          <chr> "Token Right Adjusted Events", "Sensitive Privilege Use"…
    $ opcode        <chr> "Info", "Info", "Info", "Info", "Info", "Info", "Info", …
    $ version       <int> NA, NA, 3, 3, 3, 3, 2, 3, 3, 3, 3, 3, 3, 3, NA, 3, 3, NA…
    $ user          <df[,4]> <data.frame[26 x 4]>
    $ activity_id   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    $ user_data     <df[,30]> <data.frame[26 x 30]>

``` r
data$ecs %>% glimpse ()
```

    Rows: 101,904
    Columns: 1
    $ version <chr> "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0",…

``` r
data$host %>% glimpse ()
```

    Rows: 101,904
    Columns: 1
    $ name <chr> "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", …

``` r
data$agent %>% glimpse ()
```

    Rows: 101,904
    Columns: 5
    $ ephemeral_id <chr> "b372be1f-ba0a-4d7e-b4df-79eac86e1fde", "b372be1f-ba0a-4d…
    $ hostname     <chr> "WECServer", "WECServer", "WECServer", "WECServer", "WECS…
    $ id           <chr> "d347d9a4-bff4-476c-b5a4-d51119f78250", "d347d9a4-bff4-47…
    $ version      <chr> "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.…
    $ type         <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "…

``` r
raskr_data <- data %>% unnest (c (metadata, event, log, winlog, ecs, host, agent), names_sep = ".") 
raskr_data %>% glimpse
```

    Rows: 101,904
    Columns: 34
    $ timestamp            <dttm> 2019-10-20 20:11:06, 2019-10-20 20:11:07, 2019-1…
    $ metadata.beat        <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlog…
    $ metadata.type        <chr> "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "…
    $ metadata.version     <chr> "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4…
    $ metadata.topic       <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlog…
    $ event.created        <chr> "2019-10-20T20:11:09.988Z", "2019-10-20T20:11:09.…
    $ event.kind           <chr> "event", "event", "event", "event", "event", "eve…
    $ event.code           <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, 10, 7…
    $ event.action         <chr> "Token Right Adjusted Events", "Sensitive Privile…
    $ log.level            <chr> "information", "information", "information", "inf…
    $ message              <chr> "A token right was adjusted.\n\nSubject:\n\tSecur…
    $ winlog.event_data    <df[,234]> <data.frame[26 x 234]>
    $ winlog.event_id      <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, …
    $ winlog.provider_name <chr> "Microsoft-Windows-Security-Auditing", "Microsoft…
    $ winlog.api           <chr> "wineventlog", "wineventlog", "wineventlog", "win…
    $ winlog.record_id     <int> 50588, 104875, 226649, 153525, 163488, 153526, 13…
    $ winlog.computer_name <chr> "HR001.shire.com", "HFDC01.shire.com", "IT001.shi…
    $ winlog.process       <df[,2]> <data.frame[26 x 2]>
    $ winlog.keywords      <list> "Audit Success", "Audit Failure", <NULL>, <NULL>,…
    $ winlog.provider_guid <chr> "{54849625-5478-4994-a5ba-3e3b0328c30d}", "{54…
    $ winlog.channel       <chr> "security", "Security", "Microsoft-Windows-Sysmo…
    $ winlog.task          <chr> "Token Right Adjusted Events", "Sensitive Privile…
    $ winlog.opcode        <chr> "Info", "Info", "Info", "Info", "Info", "Info", "…
    $ winlog.version       <int> NA, NA, 3, 3, 3, 3, 2, 3, 3, 3, 3, 3, 3, 3, NA, 3…
    $ winlog.user          <df[,4]> <data.frame[26 x 4]>
    $ winlog.activity_id   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ winlog.user_data     <df[,30]> <data.frame[26 x 30]>
    $ ecs.version          <chr> "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1…
    $ host.name            <chr> "WECServer", "WECServer", "WECServer", "WECSer…
    $ agent.ephemeral_id   <chr> "b372be1f-ba0a-4d7e-b4df-79eac86e1fde", "b372be1f…
    $ agent.hostname       <chr> "WECServer", "WECServer", "WECServer", "WECSe…
    $ agent.id             <chr> "d347d9a4-bff4-476c-b5a4-d51119f78250", "d347d9a4…
    $ agent.version        <chr> "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4…
    $ agent.type           <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlog…

#### 2. Минимизируйте количество колонок в датафрейме – уберите колоки с единственным значением параметра.

``` r
minimized_data <- raskr_data %>%
 select (-metadata.beat, -metadata.type, -metadata.version, -metadata.topic, -event.kind, -winlog.api, -agent.ephemeral_id, -agent.hostname, -agent.id, -agent.version, -agent.type) 
minimized_data %>% glimpse
```

    Rows: 101,904
    Columns: 23
    $ timestamp            <dttm> 2019-10-20 20:11:06, 2019-10-20 20:11:07, 2019-1…
    $ event.created        <chr> "2019-10-20T20:11:09.988Z", "2019-10-20T20:11:09.…
    $ event.code           <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, 10, 7…
    $ event.action         <chr> "Token Right Adjusted Events", "Sensitive Privile…
    $ log.level            <chr> "information", "information", "information", "inf…
    $ message              <chr> "A token right was adjusted.\n\nSubject:\n\tSecur…
    $ winlog.event_data    <df[,234]> <data.frame[26 x 234]>
    $ winlog.event_id      <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, …
    $ winlog.provider_name <chr> "Microsoft-Windows-Security-Auditing", "Microsoft…
    $ winlog.record_id     <int> 50588, 104875, 226649, 153525, 163488, 153526, 13…
    $ winlog.computer_name <chr> "HR001.shire.com", "HFDC01.shire.com", "IT001.shi…
    $ winlog.process       <df[,2]> <data.frame[26 x 2]>
    $ winlog.keywords      <list> "Audit Success", "Audit Failure", <NULL>, <NULL>,…
    $ winlog.provider_guid <chr> "{54849625-5478-4994-a5ba-3e3b0328c30d}", "{54…
    $ winlog.channel       <chr> "security", "Security", "Microsoft-Windows-Sysmo…
    $ winlog.task          <chr> "Token Right Adjusted Events", "Sensitive Privile…
    $ winlog.opcode        <chr> "Info", "Info", "Info", "Info", "Info", "Info", "…
    $ winlog.version       <int> NA, NA, 3, 3, 3, 3, 2, 3, 3, 3, 3, 3, 3, 3, NA, 3…
    $ winlog.user          <df[,4]> <data.frame[26 x 4]>
    $ winlog.activity_id   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ winlog.user_data     <df[,30]> <data.frame[26 x 30]>
    $ ecs.version          <chr> "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1…
    $ host.name            <chr> "WECServer", "WECServer", "WECServer", "WECSer…

#### 3. Какое количество хостов представлено в данном датасете?

``` r
uniq <- raskr_data %>% select (agent.hostname) %>% unique 
kolvo = count (uniq, "agent.hostname")
kolvo
```

    # A tibble: 1 × 2
      `"agent.hostname"`     n
      <chr>              <int>
    1 agent.hostname         1

#### 4. Подготовьте датафрейм с расшифровкой Windows Event_ID, приведите типы данных к типу их значений.

``` r
EventID_page <- xml2::read_html ("https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor")
data_EventID <- rvest::html_table (EventID_page) [[1]]
data_EventID %>% glimpse
```

    Rows: 381
    Columns: 4
    $ `Current Windows Event ID` <chr> "4618", "4649", "4719", "4765", "4766", "47…
    $ `Legacy Windows Event ID`  <chr> "N/A", "N/A", "612", "N/A", "N/A", "N/A", "…
    $ `Potential Criticality`    <chr> "High", "High", "High", "High", "High", "Hi…
    $ `Event Summary`            <chr> "A monitored security event pattern has occ…

``` r
data_EventID <- data_EventID %>% mutate_at (vars (`Legacy Windows Event ID`,`Current Windows Event ID`), as.integer) 
```

    Warning: There were 2 warnings in `mutate()`.
    The first warning was:
    ℹ In argument: `Legacy Windows Event ID = .Primitive("as.integer")(`Legacy
      Windows Event ID`)`.
    Caused by warning:
    ! NAs introduced by coercion
    ℹ Run `dplyr::last_dplyr_warnings()` to see the 1 remaining warning.

``` r
data_EventID %>% glimpse ()
```

    Rows: 381
    Columns: 4
    $ `Current Windows Event ID` <int> 4618, 4649, 4719, 4765, 4766, 4794, 4897, 4…
    $ `Legacy Windows Event ID`  <int> NA, NA, 612, NA, NA, NA, 801, NA, NA, 550, …
    $ `Potential Criticality`    <chr> "High", "High", "High", "High", "High", "Hi…
    $ `Event Summary`            <chr> "A monitored security event pattern has occ…

#### 5. Есть ли в логе события с высоким и средним уровнем значимости? Сколько их?

``` r
group <- data_EventID %>% group_by(`Potential Criticality`)  
znach = count(group, `Potential Criticality`)
znach %>% filter(`Potential Criticality` == "High" | `Potential Criticality` == "Medium")
```

    # A tibble: 2 × 2
    # Groups:   Potential Criticality [2]
      `Potential Criticality`     n
      <chr>                   <int>
    1 High                        9
    2 Medium                     79

## Оценка результата

Установлен пакет dplyr. Получены ответы на вопросы задания. Проведена
работа с использованием функций пакета dplyr. Проведён анализ и
обработка импортированных данных.

## Вывод

Изучены функции пакета dplyr. Получены практические навыки работы с
изученными функциями.
