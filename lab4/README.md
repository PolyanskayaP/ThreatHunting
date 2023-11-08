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
3.  Пакеты dplyr, tidyverse, readr

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
head(log, 10)
```

    # A tibble: 10 × 1
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

``` r
df <- read.csv("dns.log", header = FALSE,sep = "\t",encoding = "UTF-8")
```

``` r
head(df, 10)
```

               V1                 V2              V3    V4              V5  V6  V7
    1  1331901006 CWGtK431H9XuaTN4fi 192.168.202.100 45658  192.168.27.203 137 udp
    2  1331901015  C36a282Jljz7BsbGH  192.168.202.76   137 192.168.202.255 137 udp
    3  1331901016  C36a282Jljz7BsbGH  192.168.202.76   137 192.168.202.255 137 udp
    4  1331901017  C36a282Jljz7BsbGH  192.168.202.76   137 192.168.202.255 137 udp
    5  1331901006  C36a282Jljz7BsbGH  192.168.202.76   137 192.168.202.255 137 udp
    6  1331901007  C36a282Jljz7BsbGH  192.168.202.76   137 192.168.202.255 137 udp
    7  1331901007  C36a282Jljz7BsbGH  192.168.202.76   137 192.168.202.255 137 udp
    8  1331901006 ClEZCt3GLkJdtGGmAa  192.168.202.89   137 192.168.202.255 137 udp
    9  1331901007 ClEZCt3GLkJdtGGmAa  192.168.202.89   137 192.168.202.255 137 udp
    10 1331901008 ClEZCt3GLkJdtGGmAa  192.168.202.89   137 192.168.202.255 137 udp
          V8
    1  33008
    2  57402
    3  57402
    4  57402
    5  57398
    6  57398
    7  57398
    8  62187
    9  62187
    10 62187
                                                                            V9 V10
    1  *\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00   1
    2                                                                 HPE8AA67   1
    3                                                                 HPE8AA67   1
    4                                                                 HPE8AA67   1
    5                                                                     WPAD   1
    6                                                                     WPAD   1
    7                                                                     WPAD   1
    8                                                                   EWREP1   1
    9                                                                   EWREP1   1
    10                                                                  EWREP1   1
              V11 V12 V13 V14     V15   V16   V17   V18   V19 V20 V21 V22   V23
    1  C_INTERNET  33 SRV   0 NOERROR FALSE FALSE FALSE FALSE   1   -   - FALSE
    2  C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE
    3  C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE
    4  C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE
    5  C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE
    6  C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE
    7  C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE
    8  C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE
    9  C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE
    10 C_INTERNET  32  NB   -       - FALSE FALSE  TRUE FALSE   1   -   - FALSE

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

``` r
field<-head_df[,1]
names(df)<-field

df %>% 
  glimpse()
```

    Rows: 427,935
    Columns: 23
    $ `ts `          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901…
    $ `uid `         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz…
    $ `id `          <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", …
    $ dns_proto      <int> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
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

``` r
head(df, 10)
```

              ts                uid              id  dns_proto          proto 
    1  1331901006 CWGtK431H9XuaTN4fi 192.168.202.100     45658  192.168.27.203
    2  1331901015  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    3  1331901016  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    4  1331901017  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    5  1331901006  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    6  1331901007  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    7  1331901007  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    8  1331901006 ClEZCt3GLkJdtGGmAa  192.168.202.89       137 192.168.202.255
    9  1331901007 ClEZCt3GLkJdtGGmAa  192.168.202.89       137 192.168.202.255
    10 1331901008 ClEZCt3GLkJdtGGmAa  192.168.202.89       137 192.168.202.255
       trans_id  query  qclass 
    1        137    udp   33008
    2        137    udp   57402
    3        137    udp   57402
    4        137    udp   57402
    5        137    udp   57398
    6        137    udp   57398
    7        137    udp   57398
    8        137    udp   62187
    9        137    udp   62187
    10       137    udp   62187
                                                                  qclass_name 
    1  *\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00
    2                                                                 HPE8AA67
    3                                                                 HPE8AA67
    4                                                                 HPE8AA67
    5                                                                     WPAD
    6                                                                     WPAD
    7                                                                     WPAD
    8                                                                   EWREP1
    9                                                                   EWREP1
    10                                                                  EWREP1
       qtype  qtype_name  rcode  rcode_name  QR      AA  TC RD    RA     Z 
    1       1  C_INTERNET     33         SRV   0 NOERROR  FALSE FALSE FALSE
    2       1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
    3       1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
    4       1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
    5       1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
    6       1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
    7       1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
    8       1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
    9       1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
    10      1  C_INTERNET     32          NB   -       -  FALSE FALSE  TRUE
       answers  TTLs  rejected  NA    NA
    1     FALSE     1         -  - FALSE
    2     FALSE     1         -  - FALSE
    3     FALSE     1         -  - FALSE
    4     FALSE     1         -  - FALSE
    5     FALSE     1         -  - FALSE
    6     FALSE     1         -  - FALSE
    7     FALSE     1         -  - FALSE
    8     FALSE     1         -  - FALSE
    9     FALSE     1         -  - FALSE
    10    FALSE     1         -  - FALSE

#### 4. Просмотрите общую структуру данных с помощью функции glimpse()

``` r
glimpse(head_df)
```

    Rows: 21
    Columns: 3
    $ Field       <chr> "ts ", "uid ", "id ", "dns_proto", "proto ", "trans_id ", …
    $ Type        <chr> "time ", "string", "recor ", "d ", "proto", "count", "stri…
    $ Description <chr> "Timestamp of the DNS request ", "Unique id of the connect…

``` r
glimpse(df)
```

    Rows: 427,935
    Columns: 23
    $ `ts `          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901…
    $ `uid `         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz…
    $ `id `          <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", …
    $ dns_proto      <int> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
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

### Анализ

#### 4. Сколько участников информационного обмена в сети Доброй Организации?

``` r
unique_id <- unique(df$id)
unique_proto <- unique(df$proto)
unique_people <- union(unique_id , unique_proto)
unique_people %>% length()
```

    [1] 1359

#### 5. Какое соотношение участников обмена внутри сети и участников обращений к внешним ресурсам?

#### 6. Найдите топ-10 участников сети, проявляющих наибольшую сетевую активность.

#### 7. Найдите топ-10 доменов, к которым обращаются пользователи сети и соответственное количество обращений

#### 8. Опеределите базовые статистические характеристики (функция summary() ) интервала времени между последовательным обращениями к топ-10 доменам.

#### 9. Часто вредоносное программное обеспечение использует DNS канал в качестве каналауправления, периодически отправляя запросы на подконтрольный злоумышленникам DNS сервер. По периодическим запросам на один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP адреса в исследуемом датасете?

### Обогащение данных

#### 10. Определите местоположение (страну, город) и организацию-провайдера для топ-10 доменов. Для этого можно использовать сторонние сервисы, например https://v4.ifconfig.co.

## Оценка результатов

Задания были успешно решены с помощью библиотеки dplyr, tidyverse,
readr.

## Вывод

Были изучены возможности библиотек tidyverse, readr.
