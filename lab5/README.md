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

#### Точки доступа

##### 1. Определить небезопасные точки доступа (без шифрования – OPN)

``` r
wifi <- mir %>% filter (grepl ("OPN", Privacy)) %>% select (BSSID, ESSID) %>% arrange (BSSID) %>% distinct

wifi
```

                   BSSID         ESSID
    1  00:03:7A:1A:03:56       MT_FREE
    2  00:03:7F:12:34:56       MT_FREE
    3  00:25:00:FF:94:73          <NA>
    4  00:26:99:F2:7A:E0          <NA>
    5  00:26:99:F2:7A:EF          <NA>
    6  00:3E:1A:5D:14:45       MT_FREE
    7  00:53:7A:99:98:56       MT_FREE
    8  00:AB:0A:00:10:10          <NA>
    9  02:67:F1:B0:6C:98       MT_FREE
    10 02:BC:15:7E:D5:DC       MT_FREE
    11 02:CF:8B:87:B4:F9       MT_FREE
    12 E0:D9:E3:48:FF:D2          <NA>
    13 E0:D9:E3:49:00:B1          <NA>
    14 E8:28:C1:DC:0B:B2          <NA>
    15 E8:28:C1:DC:33:12          <NA>
    16 E8:28:C1:DC:B2:40 MIREA_HOTSPOT
    17 E8:28:C1:DC:B2:41  MIREA_GUESTS
    18 E8:28:C1:DC:B2:42          <NA>
    19 E8:28:C1:DC:B2:50  MIREA_GUESTS
    20 E8:28:C1:DC:B2:51          <NA>
    21 E8:28:C1:DC:B2:52 MIREA_HOTSPOT
    22 E8:28:C1:DC:BD:50  MIREA_GUESTS
    23 E8:28:C1:DC:BD:52 MIREA_HOTSPOT
    24 E8:28:C1:DC:C6:B0  MIREA_GUESTS
    25 E8:28:C1:DC:C6:B1          <NA>
    26 E8:28:C1:DC:C6:B2          <NA>
    27 E8:28:C1:DC:C8:30  MIREA_GUESTS
    28 E8:28:C1:DC:C8:31          <NA>
    29 E8:28:C1:DC:C8:32 MIREA_HOTSPOT
    30 E8:28:C1:DC:FF:F2          <NA>
    31 E8:28:C1:DD:04:40 MIREA_HOTSPOT
    32 E8:28:C1:DD:04:41  MIREA_GUESTS
    33 E8:28:C1:DD:04:42          <NA>
    34 E8:28:C1:DD:04:50  MIREA_GUESTS
    35 E8:28:C1:DD:04:51          <NA>
    36 E8:28:C1:DD:04:52 MIREA_HOTSPOT
    37 E8:28:C1:DE:47:D0  MIREA_GUESTS
    38 E8:28:C1:DE:47:D1          <NA>
    39 E8:28:C1:DE:47:D2 MIREA_HOTSPOT
    40 E8:28:C1:DE:74:30          <NA>
    41 E8:28:C1:DE:74:31          <NA>
    42 E8:28:C1:DE:74:32 MIREA_HOTSPOT

##### 2. Определить производителя для каждого обнаруженного устройства

-   00:03:7A Taiyo Yuden Co., Ltd.
-   00:03:7F Atheros Communications, Inc.
-   00:25:00 Apple, Inc.
-   00:26:99 Cisco Systems, Inc
-   E0:D9:E3 Eltex Enterprise Ltd.
-   E8:28:C1 Eltex Enterprise Ltd.

##### 3. Выявить устройства, использующие последнюю версию протокола шифрования WPA3, и названия точек доступа, реализованных на этих устройствах

``` r
mir %>% filter (grepl ("WPA3", Privacy)) %>% select(BSSID, ESSID, Privacy)
```

                  BSSID              ESSID   Privacy
    1 26:20:53:0C:98:E8               <NA> WPA3 WPA2
    2 A2:FE:FF:B8:9B:C9         Christie’s WPA3 WPA2
    3 96:FF:FC:91:EF:64               <NA> WPA3 WPA2
    4 CE:48:E7:86:4E:33 iPhone (Анастасия) WPA3 WPA2
    5 8E:1F:94:96:DA:FD iPhone (Анастасия) WPA3 WPA2
    6 BE:FD:EF:18:92:44            Димасик WPA3 WPA2
    7 3A:DA:00:F9:0C:02  iPhone XS Max 🦊🐱🦊 WPA3 WPA2
    8 76:C5:A0:70:08:96               <NA> WPA3 WPA2

##### 4.Отсортировать точки доступа по интервалу времени, в течение которого они находились на связи, по убыванию.

##### 5. Обнаружить топ-10 самых быстрых точек доступа.

##### 6. Отсортировать точки доступа по частоте отправки запросов (beacons) в единицу времени по их убыванию.

#### Данные клиентов

##### 1. Определить производителя для каждого обнаруженного устройства

##### 2. Обнаружить устройства, которые НЕ рандомизируют свой MAC адрес

##### 3. Кластеризовать запросы от устройств к точкам доступа по их именам. Определить время появления устройства в зоне радиовидимости и время выхода его из нее.

##### 4. Оценить стабильность уровня сигнала внури кластера во времени. Выявить наиболее стабильный кластер.
