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

\`\`{r}

library(dplyr) library(jsonlite) library(tidyr) library(xml2)
library(rvest) \`\`

#### Импорт данных

\`\`{r} url_yand \<-
“https://storage.yandexcloud.net/iamcth-data/dataset.tar.gz”
download.file(url_yand, destfile = tf \<- tempfile(fileext = “.tar.gz”),
mode = “wb”) temp_dir \<- tempdir() untar(tf, exdir = temp_dir)
json_files \<- list.files(temp_dir, pattern=“\\.json$”, full.names =
TRUE, recursive = TRUE) data \<- stream_in(file(json_files))

`#### Преобразование данных`{r} data \<- data %\>% mutate(`@timestamp` =
as.POSIXct(`@timestamp`, format = “%Y-%m-%dT%H:%M:%OSZ”, tz = “UTC”))
%\>% rename(timestamp = `@timestamp`, metadata = `@metadata`) \`\`

#### Общая структура

\`\`{r} data %\>% glimpse()

\`\`

### Анализ

#### 1. Раскройте датафрейм избавившись от вложенных датафреймов. Для обнаружения таких можно использовать функцию dplyr::glimpse() , а для раскрытия вложенности – tidyr::unnest() . Обратите внимание, что при раскрытии теряются внешние названия колонок – это можно предотвратить если использовать параметр tidyr::unnest(…, names_sep = ) .

`{r} data$metadata %>% glimpse ()`
```` {r} data$event %>% glimpse () ``` ````{r} data$log %\>% glimpse ()

``{r} data*w**i**n**l**o**g*ecs %\>% glimpse () ``{r}
data*h**o**s**t*agent %\>% glimpse () ``{r} raskr_data \<- data %\>%
unnest (c (metadata, event, log, winlog, ecs, host, agent), names_sep =
“.”) raskr_data %\>% glimpse \`\`

#### 2. Минимизируйте количество колонок в датафрейме – уберите колоки с единственным значением параметра.

`{r} minimized_data <- raskr_data %>%  select (-metadata.beat, -metadata.type, -metadata.version, -metadata.topic, -event.kind, -winlog.api, -agent.ephemeral_id, -agent.hostname, -agent.id, -agent.version, -agent.type)  minimized_data %>% glimpse`

#### 3. Какое количество хостов представлено в данном датасете?

\`\`{r} uniq \<- raskr_data %\>% select (agent.hostname) %\>% unique
kolvo = count (uniq, “agent.hostname”) kolvo


    #### 4. Подготовьте датафрейм с расшифровкой Windows Event_ID, приведите типы данных к типу их значений.

    ``{r}
    EventID_page <- xml2::read_html ("https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor")
    data_EventID <- rvest::html_table (EventID_page) [[1]]
    data_EventID %>% glimpse
    ``

    ``{r}
    data_EventID <- data_EventID %>% mutate_at (vars (`Legacy Windows Event ID`,`Current Windows Event ID`), as.integer) 

    ``

    ``{r}
    data_EventID %>% glimpse ()

#### 5. Есть ли в логе события с высоким и средним уровнем значимости? Сколько их?

\``{r} group <- data_EventID %>% group_by(`Potential
Criticality`)   znach = count(group,`Potential
Criticality`) znach %>% filter(`Potential
Criticality`== "High" |`Potential Criticality\` == “Medium”)

\`\`

## Оценка результата

Установлен пакет dplyr. Получены ответы на вопросы задания. Проведена
работа с использованием функций пакета dplyr. Проведён анализ и
обработка импортированных данных.

## Вывод

Изучены функции пакета dplyr. Получены практические навыки работы с
изученными функциями.
