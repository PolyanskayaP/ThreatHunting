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
head_df <- read.csv('C:/ThreatHunting/lab4/header.csv')
```

``` r
head_df
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

``` r
log <- read_log("C:/ThreatHunting/lab4/dns.log")
```


    ── Column specification ────────────────────────────────────────────────────────
    cols(
      X1 = col_character()
    )

    Warning: 382 parsing failures.
     row col  expected    actual                            file
    2384  -- 1 columns 4 columns 'C:/ThreatHunting/lab4/dns.log'
    4209  -- 1 columns 3 columns 'C:/ThreatHunting/lab4/dns.log'
    4211  -- 1 columns 3 columns 'C:/ThreatHunting/lab4/dns.log'
    4212  -- 1 columns 3 columns 'C:/ThreatHunting/lab4/dns.log'
    5232  -- 1 columns 3 columns 'C:/ThreatHunting/lab4/dns.log'
    .... ... ......... ......... ...............................
    See problems(...) for more details.

``` r
log
```

    # A tibble: 427,935 × 1
       X1                                                                           
       <chr>                                                                        
     1 "1331901005.510000\tCWGtK431H9XuaTN4fi\t192.168.202.100\t45658\t192.168.27.2…
     2 "1331901015.070000\tC36a282Jljz7BsbGH\t192.168.202.76\t137\t192.168.202.255\…
     3 "1331901015.820000\tC36a282Jljz7BsbGH\t192.168.202.76\t137\t192.168.202.255\…
     4 "1331901016.570000\tC36a282Jljz7BsbGH\t192.168.202.76\t137\t192.168.202.255\…
     5 "1331901005.860000\tC36a282Jljz7BsbGH\t192.168.202.76\t137\t192.168.202.255\…
     6 "1331901006.610000\tC36a282Jljz7BsbGH\t192.168.202.76\t137\t192.168.202.255\…
     7 "1331901007.360000\tC36a282Jljz7BsbGH\t192.168.202.76\t137\t192.168.202.255\…
     8 "1331901006.370000\tClEZCt3GLkJdtGGmAa\t192.168.202.89\t137\t192.168.202.255…
     9 "1331901007.120000\tClEZCt3GLkJdtGGmAa\t192.168.202.89\t137\t192.168.202.255…
    10 "1331901007.870000\tClEZCt3GLkJdtGGmAa\t192.168.202.89\t137\t192.168.202.255…
    # ℹ 427,925 more rows

НЕПРАВИЛЬНОООО

``` r
df <- read_tsv("C:/ThreatHunting/lab4/dns.log", show_col_types = FALSE)
```

    New names:
    • `1` -> `1...10`
    • `F` -> `F...16`
    • `F` -> `F...17`
    • `F` -> `F...18`
    • `F` -> `F...19`
    • `1` -> `1...20`
    • `-` -> `-...21`
    • `-` -> `-...22`
    • `F` -> `F...23`

``` r
df
```

    # A tibble: 427,934 × 23
       `1331901005.510000` CWGtK431H9XuaTN4fi `192.168.202.100` `45658`
                     <dbl> <chr>              <chr>               <dbl>
     1         1331901015. C36a282Jljz7BsbGH  192.168.202.76        137
     2         1331901016. C36a282Jljz7BsbGH  192.168.202.76        137
     3         1331901017. C36a282Jljz7BsbGH  192.168.202.76        137
     4         1331901006. C36a282Jljz7BsbGH  192.168.202.76        137
     5         1331901007. C36a282Jljz7BsbGH  192.168.202.76        137
     6         1331901007. C36a282Jljz7BsbGH  192.168.202.76        137
     7         1331901006. ClEZCt3GLkJdtGGmAa 192.168.202.89        137
     8         1331901007. ClEZCt3GLkJdtGGmAa 192.168.202.89        137
     9         1331901008. ClEZCt3GLkJdtGGmAa 192.168.202.89        137
    10         1331901007. CpD4i41jyaYqmTBMH3 192.168.202.89        137
    # ℹ 427,924 more rows
    # ℹ 19 more variables: `192.168.27.203` <chr>, `137` <dbl>, udp <chr>,
    #   `33008` <dbl>,
    #   `*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00` <chr>,
    #   `1...10` <chr>, C_INTERNET <chr>, `33` <chr>, SRV <chr>, `0` <chr>,
    #   NOERROR <chr>, F...16 <lgl>, F...17 <lgl>, F...18 <lgl>, F...19 <lgl>,
    #   `1...20` <dbl>, `-...21` <chr>, `-...22` <chr>, F...23 <lgl>

#### 2. Добавьте пропущенные данные о структуре данных (назначении столбцов)

``` r
head_df[4, "Field"] <- "dns_proto"
```

``` r
head_df
```

              Field       Type
    1           ts       time 
    2          uid      string
    3           id      recor 
    4     dns_proto         d 
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

#### 3. Преобразуйте данные в столбцах в нужный формат

#### 4. Просмотрите общую структуру данных с помощью функции glimpse()

``` r
glimpse(head_df)
```

    Rows: 21
    Columns: 3
    $ Field       <chr> "ts ", "uid ", "id ", "dns_proto", "proto ", "trans_id ", …
    $ Type        <chr> "time ", "string", "recor ", "d ", "proto", "count", "stri…
    $ Description <chr> "Timestamp of the DNS request ", "Unique id of the connect…

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
