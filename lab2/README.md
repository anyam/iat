Практика 2
================
Гренкова Анна БИСО-01-20

# Основы обработки данных с помощью R с использованием пакета dplyr.

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных;
2.  Закрепить знания базовых типов данных языка R;
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции `select ()`, `filter ()`, `mutate ()`,
    `arrange ()`, `group_by ()`.

## Исходные данные

1.  ОС Windows 11
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.0
4.  Пакет `dplyr`

## План

1.  Установить пакет `dplyr`
2.  Выполнить задания.

## Ход работы

### Шаг 0

Установка пакета `dplyr` с помощью команды `install.packages ("dplyr")`.

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

### Ответы на вопросы

#### 1. Сколько строк в датафрейме?

``` r
starwars %>% nrow ()
```

    [1] 87

#### 2. Сколько столбцов в датафрейме?

``` r
starwars %>% ncol ()
```

    [1] 14

#### 3. Как просмотреть примерный вид датафрейма?

``` r
starwars %>% glimpse ()
```

    Rows: 87
    Columns: 14
    $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Or…
    $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 2…
    $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.…
    $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", N…
    $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "…
    $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue",…
    $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, …
    $ sex        <chr> "male", "none", "none", "male", "female", "male", "female",…
    $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "femini…
    $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "T…
    $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Huma…
    $ films      <list> <"A New Hope", "The Empire Strikes Back", "Return of the J…
    $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imp…
    $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1",…

#### 4. Сколько уникальных рас персонажей (species) представлено в данных?

``` r
starwars %>% select (species) %>% 
  unique ()
```

    # A tibble: 38 × 1
       species       
       <chr>         
     1 Human         
     2 Droid         
     3 Wookiee       
     4 Rodian        
     5 Hutt          
     6 <NA>          
     7 Yoda's species
     8 Trandoshan    
     9 Mon Calamari  
    10 Ewok          
    # ℹ 28 more rows

#### 5. Найти самого высокого персонажа.

``` r
starwars %>% filter (height == max(height, na.rm = TRUE)) %>% 
  select (name, height)
```

    # A tibble: 1 × 2
      name        height
      <chr>        <int>
    1 Yarael Poof    264

#### 6. Найти всех персонажей ниже 170

``` r
starwars %>% filter (height == max (height, na.rm = TRUE)) %>% select (name, height)
```

    # A tibble: 1 × 2
      name        height
      <chr>        <int>
    1 Yarael Poof    264

#### 7. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать по формуле I = m/h^2 , где m – масса (weight), а h – рост (height).

``` r
starwars %>% mutate (bmi = mass / ((height / 100) ^ 2)) %>% select (name, bmi)
```

    # A tibble: 87 × 2
       name                 bmi
       <chr>              <dbl>
     1 Luke Skywalker      26.0
     2 C-3PO               26.9
     3 R2-D2               34.7
     4 Darth Vader         33.3
     5 Leia Organa         21.8
     6 Owen Lars           37.9
     7 Beru Whitesun Lars  27.5
     8 R5-D4               34.0
     9 Biggs Darklighter   25.1
    10 Obi-Wan Kenobi      23.2
    # ℹ 77 more rows

#### 8. Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по отношению массы (mass) к росту (height) персонажей.

``` r
starwars %>% mutate (lenght = mass / height) %>% arrange (desc (lenght)) %>% head (10) %>% select (name, lenght)
```

    # A tibble: 10 × 2
       name                  lenght
       <chr>                  <dbl>
     1 Jabba Desilijic Tiure  7.76 
     2 Grievous               0.736
     3 IG-88                  0.7  
     4 Owen Lars              0.674
     5 Darth Vader            0.673
     6 Jek Tono Porkins       0.611
     7 Bossk                  0.595
     8 Tarfful                0.581
     9 Dexter Jettster        0.515
    10 Chewbacca              0.491

#### 9. Найти средний возраст персонажей каждой расы вселенной Звёздных войн

``` r
starwars %>% group_by (species) %>% summarise (average_age = mean (birth_year, na.rm = TRUE))
```

    # A tibble: 38 × 2
       species   average_age
       <chr>           <dbl>
     1 Aleena          NaN  
     2 Besalisk        NaN  
     3 Cerean           92  
     4 Chagrian        NaN  
     5 Clawdite        NaN  
     6 Droid            53.3
     7 Dug             NaN  
     8 Ewok              8  
     9 Geonosian       NaN  
    10 Gungan           52  
    # ℹ 28 more rows

#### 10. Найти самый распространённый цвет глаз персонажей вселенной Звёздных войн

``` r
starwars %>% count (eye_color) %>% filter (n == max (n))
```

    # A tibble: 1 × 2
      eye_color     n
      <chr>     <int>
    1 brown        21

#### 11. Подсчитать среднюю длинц имени в каждой расе вселенной Звёздных войн

``` r
starwars %>% group_by (species) %>% summarise (name_lenght = mean (nchar (name)))
```

    # A tibble: 38 × 2
       species   name_lenght
       <chr>           <dbl>
     1 Aleena          12   
     2 Besalisk        15   
     3 Cerean          12   
     4 Chagrian        10   
     5 Clawdite        10   
     6 Droid            4.83
     7 Dug              7   
     8 Ewok            21   
     9 Geonosian       17   
    10 Gungan          11.7 
    # ℹ 28 more rows

## Оценка результата

Установлен пакет dplyr. Получены ответы на вопросы задания. Проведена
работа с использованием функций пакета dplyr.

## Вывод

Изучены функции пакета dplyr. Получены практические навыки работы с
изученными функциями.
