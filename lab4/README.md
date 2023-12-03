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
library(dplyr)
```

#### 1. Импортируйте данные DNS

``` r
df <- read.table("dns.log", header = FALSE, sep = "\t", quote = "", encoding="UTF-8")
```

#### 2. Добавьте пропущенные данные о структуре данных (назначении столбцов), 3. Преобразуйте данные в столбцах в нужный формат

``` r
colnames(df) <- read.csv("header_zeek.csv", header = FALSE, skip = 1)$V1 #новый header с названиями в соответствии с документацией zeek
```

#### 4. Просмотрите общую структуру данных с помощью функции glimpse()

``` r
df %>%
  glimpse()
```

    Rows: 427,935
    Columns: 23
    $ ts          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901006…
    $ uid         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz7Bs…
    $ id.orig_h   <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", "19…
    $ id.orig_p   <int> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 1…
    $ id.resp_h   <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255", "1…
    $ id_resp_p   <int> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ proto       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "u…
    $ trans_id    <int> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187, 62…
    $ query       <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\…
    $ qclass      <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1"…
    $ qclass_name <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C…
    $ qtype       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "32"…
    $ qtype_name  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB…
    $ rcode       <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rcode_name  <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-…
    $ AA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ TC          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ RD          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRU…
    $ RA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ Z           <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0…
    $ answers     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ TTLs        <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rejected    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…

``` r
head(df)
```

              ts                uid       id.orig_h id.orig_p       id.resp_h
    1 1331901006 CWGtK431H9XuaTN4fi 192.168.202.100     45658  192.168.27.203
    2 1331901015  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    3 1331901016  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    4 1331901017  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    5 1331901006  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
    6 1331901007  C36a282Jljz7BsbGH  192.168.202.76       137 192.168.202.255
      id_resp_p proto trans_id
    1       137   udp    33008
    2       137   udp    57402
    3       137   udp    57402
    4       137   udp    57402
    5       137   udp    57398
    6       137   udp    57398
                                                                        query
    1 *\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00
    2                                                                HPE8AA67
    3                                                                HPE8AA67
    4                                                                HPE8AA67
    5                                                                    WPAD
    6                                                                    WPAD
      qclass qclass_name qtype qtype_name rcode rcode_name    AA    TC    RD    RA
    1      1  C_INTERNET    33        SRV     0    NOERROR FALSE FALSE FALSE FALSE
    2      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE FALSE
    3      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE FALSE
    4      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE FALSE
    5      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE FALSE
    6      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE FALSE
      Z answers TTLs rejected
    1 1       -    -    FALSE
    2 1       -    -    FALSE
    3 1       -    -    FALSE
    4 1       -    -    FALSE
    5 1       -    -    FALSE
    6 1       -    -    FALSE

#### 4. Сколько участников информационного обмена в сети Доброй Организации?

``` r
unique_id <- unique(df$`id.orig_h`)
unique_proto <- unique(df$`id.resp_h`)
unique_people <- union(unique_id , unique_proto)
unique_people %>% length()
```

    [1] 1359

#### 5. Какое соотношение участников обмена внутри сети и участников обращений к внешним ресурсам?

``` r
hostlar <- c(unique_id, unique_proto)
internal_ip_pattern <- c("192.168.", "10.", "100.([6-9]|1[0-1][0-9]|12[0-7]).", "172.((1[6-9])|(2[0-9])|(3[0-1])).")
internal_ips <- hostlar[grep(paste(internal_ip_pattern, collapse = "|"), hostlar)]
internal_ips_cnt <- sum(hostlar %in% internal_ips)
external_ips_cnt <- length(hostlar) - internal_ips_cnt
stn_in_ex <- internal_ips_cnt / external_ips_cnt
stn_in_ex
```

    [1] 16.24419

#### 6. Найдите топ-10 участников сети, проявляющих наибольшую сетевую активность.

``` r
top_hostlar <- df %>%
  group_by(id.orig_h) %>%
  summarise(request_count = n()) %>%
  arrange(desc(request_count)) %>%
  top_n(10, request_count)
top_hostlar
```

    # A tibble: 10 × 2
       id.orig_h       request_count
       <chr>                   <int>
     1 10.10.117.210           75943
     2 192.168.202.93          26522
     3 192.168.202.103         18121
     4 192.168.202.76          16978
     5 192.168.202.97          16176
     6 192.168.202.141         14967
     7 10.10.117.209           14222
     8 192.168.202.110         13372
     9 192.168.203.63          12148
    10 192.168.202.106         10784

#### 7. Найдите топ-10 доменов, к которым обращаются пользователи сети и соответственное количество обращений

``` r
top_domainler <- df %>%
  group_by(query) %>%
  summarise(request_count = n()) %>%
  arrange(desc(request_count)) %>%
  top_n(10, request_count)
top_domainler
```

    # A tibble: 10 × 2
       query                                                           request_count
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00…         10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7248
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569

#### 8. Опеределите базовые статистические характеристики (функция summary() ) интервала времени между последовательным обращениями к топ-10 доменам.

``` r
t_d_filter <- df %>% filter(tolower(query) %in% top_domainler$query) %>% arrange(ts)
t_i <- diff(t_d_filter$ts)
summary(t_i)
```

        Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
        0.00     0.00     0.10     1.07     0.54 49677.59 

#### 9. Часто вредоносное программное обеспечение использует DNS канал в качестве каналауправления, периодически отправляя запросы на подконтрольный злоумышленникам DNS сервер. По периодическим запросам на один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP адреса в исследуемом датасете?

``` r
ip_domain_counts <- df %>%
  group_by(ip = `id.orig_h`, query) %>%
  summarise(request_count = n()) %>%
  filter(request_count > 1)
```

    `summarise()` has grouped output by 'ip'. You can override using the `.groups`
    argument.

``` r
unique_ips_with_periodic_requests <- unique(ip_domain_counts$ip)

unique_ips_with_periodic_requests %>% length()
```

    [1] 240

``` r
unique_ips_with_periodic_requests %>% head()
```

    [1] "10.10.10.10"     "10.10.117.209"   "10.10.117.210"   "128.244.37.196" 
    [5] "169.254.109.123" "169.254.228.26" 

### Обогащение данных

#### 10. Определите местоположение (страну, город) и организацию-провайдера для топ-10 доменов. Для этого можно использовать сторонние сервисы, например https://v4.ifconfig.co.

``` r
print(top_domainler$query)
```

     [1] "teredo.ipv6.microsoft.com"                                              
     [2] "tools.google.com"                                                       
     [3] "www.apple.com"                                                          
     [4] "time.apple.com"                                                         
     [5] "safebrowsing.clients.google.com"                                        
     [6] "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00"
     [7] "WPAD"                                                                   
     [8] "44.206.168.192.in-addr.arpa"                                            
     [9] "HPE8AA67"                                                               
    [10] "ISATAP"                                                                 

Местоположения были определены при помощи domaintools.site:

HOSTNAME: Tools.google.com IP: 209.85.233.102 CONTINENT: North America
COUNTRY: United States

HOSTNAME: Apple.com IP: 17.253.144.10 CONTINENT: North America COUNTRY:
United States

HOSTNAME: Time.apple.com IP: 17.253.38.253 CONTINENT: Europe COUNTRY:
Sweden

HOSTNAME: Safebrowsing.clients.google.com IP: 142.250.150.138 CONTINENT:
North America COUNTRY: United States

## Оценка результатов

Задания были успешно решены с помощью библиотеки dplyr, tidyverse,
readr.

## Вывод

Были изучены возможности библиотек tidyverse, readr, проанализированы
данные сетевого трафика.
