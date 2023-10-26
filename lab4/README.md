# lab4
Полянская Полина Алексеевна

Исследование метаданных DNS трафика

## Цель

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

1.  Ноутбук с ОС Windows 10
2.  RStudio
3.  Пакеты dplyr, nycflights13

## Задание

Используя программный пакет dplyr, освоить анализ DNS логов с помощью
языка программирования R.

### Подготовка данных

#### 1. Импортируйте данные DNS

Был установлен пакет tidyverse, в котором содержится пакет readr
install.packages(“tidyverse”)

``` r
library(tidyverse)
```

    Warning: пакет 'tidyverse' был собран под R версии 4.2.3

    Warning: пакет 'ggplot2' был собран под R версии 4.2.3

    Warning: пакет 'tibble' был собран под R версии 4.2.3

    Warning: пакет 'tidyr' был собран под R версии 4.2.3

    Warning: пакет 'readr' был собран под R версии 4.2.3

    Warning: пакет 'purrr' был собран под R версии 4.2.3

    Warning: пакет 'dplyr' был собран под R версии 4.2.3

    Warning: пакет 'forcats' был собран под R версии 4.2.3

    Warning: пакет 'lubridate' был собран под R версии 4.2.3

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readr)
```

``` r
df <- read.csv('C:/!Полина/!7семестр/ИАТПУИБ/ThreatHunting/lab4/header.csv')
```

``` r
df
```

              Field       Type
    1           ts       time 
    2          uid      string
    3           id      recor 
    4                       d 
    5        proto       proto
    6     trans_id       count
    7        query      string
    8       qclass       count
    9  qclass_name      string
    10       qtype       count
    11  qtype_name      string
    12       rcode       count
    13  rcode_name      string
    14          QR       bool 
    15          AA       bool 
    16       TC RD  bool bool 
    17          RA       bool 
    18           Z       count
    19     answers      vector
    20        TTLs      vector
    21    rejected       bool 
                                                                                           Description
    1                                                                    Timestamp of the DNS request 
    2                                                                     Unique id of the connection 
    3                                                ID record with orig/resp host/port. See conn.log 
    4                                                                                                 
    5                                                        Protocol of DNS transaction – TCP or UDP 
    6                                       16 bit identifier assigned by DNS client; responses match 
    7                                                                Domain name subject of the query 
    8                                                                Value specifying the query class 
    9                                           Descriptive name of the query class (e.g. C_INTERNET) 
    10                                                                Value specifying the query type 
    11                                                     Name of the query type (e.g. A, AAAA, PTR) 
    12                                                        Response code value in the DNS response 
    13                                 Descriptive name of the response code (e.g. NOERROR, NXDOMAIN) 
    14                                        Was this a query or a response? T = response, F = query 
    15                                    Authoritative Answer. T = server is authoritative for query 
    16 Truncation. T = message was truncated Recursion Desired. T = request recursive lookup of query 
    17                                     Recursion Available. T = server supports recursive queries 
    18                                      Reserved field, should be zero in all queries & responses 
    19                                           List of resource descriptions in answer to the query 
    20                                                               Caching intervals of the answers 
    21                                               Whether the DNS query was rejected by the server 

#### 2. Добавьте пропущенные данные о структуре данных (назначении столбцов)

#### 3. Преобразуйте данные в столбцах в нужный формат

#### 4. Просмотрите общую структуру данных с помощью функции glimpse()

### Анализ

#### 4. Сколько участников информационного обмена в сети Доброй Организации?

#### 5. Какое соотношение участников обмена внутри сети и участников обращений к внешним ресурсам?

#### 6. Найдите топ-10 участников сети, проявляющих наибольшую сетевую активность.

#### 7. Найдите топ-10 доменов, к которым обращаются пользователи сети и соответственное количество обращений

#### 8. Опеределите базовые статистические характеристики (функция summary() ) интервала времени между последовательным обращениями к топ-10 доменам.

#### 9. Часто вредоносное программное обеспечение использует DNS канал в качестве каналауправления, периодически отправляя запросы на подконтрольный злоумышленникам DNS сервер. По периодическим запросам на один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP адреса в исследуемом датасете?

### Обогащение данных

#### 10. Определите местоположение (страну, город) и организацию-провайдера для топ-10 доменов. Для этого можно использовать сторонние сервисы, например https://v4.ifconfig.co.

## Оценка результатов

Задания были успешно решены с помощью библиотеки dplyr и ….

## Вывод

Были изучены….
