# lab3
Полянская Полина Алексеевна

Основы обработки данных с помощью R

## Цель

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  Ноутбук с ОС Windows 10
2.  RStudio
3.  Пакеты dplyr, nycflights13

## Задание

Проанализировать встроенные в пакет nycflights13 наборы данных с помощью
языка R и ответить на вопросы. До этого нужно: - установить пакет dplyr
командой install.packages(‘dplyr’) - загрузить библиотеку dplyr командой
library(dplyr) - установить пакет nycflights13 командой
install.packages(‘nycflights13’) - загрузить библиотеку nycflights13
командой library(nycflights13)

``` r
library(dplyr)
```

    Warning: пакет 'dplyr' был собран под R версии 4.2.3


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

``` r
library(nycflights13)
```

    Warning: пакет 'nycflights13' был собран под R версии 4.2.3

### 1. Сколько встроенных в пакет nycflights13 датафреймов?

Этот пакет предоставляет 5 датафреймов.

flights: все рейсы, вылетевшие из Нью-Йорка в 2013 году weather:
почасовые метеорологические данные для каждого аэропорта planes:
информация о конструкции каждого самолета airports: названия аэропортов
и местоположения airlines: перевод между двумя кодами и названиями
почтовых отправлений

### 2. Сколько строк в каждом датафрейме?

``` r
flights %>%
  nrow()
```

    [1] 336776

``` r
weather %>%
  nrow()
```

    [1] 26115

``` r
planes %>%
  nrow()
```

    [1] 3322

``` r
airports %>%
  nrow()
```

    [1] 1458

``` r
airlines %>% 
  nrow()
```

    [1] 16

### 3. Сколько столбцов в каждом датафрейме?

``` r
flights %>%
  ncol()
```

    [1] 19

``` r
weather %>%
  ncol()
```

    [1] 15

``` r
planes %>%
  ncol()
```

    [1] 9

``` r
airports %>%
  ncol()
```

    [1] 8

``` r
airlines %>% 
  ncol()
```

    [1] 2

### 4. Как просмотреть примерный вид датафрейма?

``` r
nycflights13::flights
```

    # A tibble: 336,776 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      542            540         2      923            850
     4  2013     1     1      544            545        -1     1004           1022
     5  2013     1     1      554            600        -6      812            837
     6  2013     1     1      554            558        -4      740            728
     7  2013     1     1      555            600        -5      913            854
     8  2013     1     1      557            600        -3      709            723
     9  2013     1     1      557            600        -3      838            846
    10  2013     1     1      558            600        -2      753            745
    # ℹ 336,766 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
glimpse(flights)
```

    Rows: 336,776
    Columns: 19
    $ year           <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2…
    $ month          <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
    $ day            <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
    $ dep_time       <int> 517, 533, 542, 544, 554, 554, 555, 557, 557, 558, 558, …
    $ sched_dep_time <int> 515, 529, 540, 545, 600, 558, 600, 600, 600, 600, 600, …
    $ dep_delay      <dbl> 2, 4, 2, -1, -6, -4, -5, -3, -3, -2, -2, -2, -2, -2, -1…
    $ arr_time       <int> 830, 850, 923, 1004, 812, 740, 913, 709, 838, 753, 849,…
    $ sched_arr_time <int> 819, 830, 850, 1022, 837, 728, 854, 723, 846, 745, 851,…
    $ arr_delay      <dbl> 11, 20, 33, -18, -25, 12, 19, -14, -8, 8, -2, -3, 7, -1…
    $ carrier        <chr> "UA", "UA", "AA", "B6", "DL", "UA", "B6", "EV", "B6", "…
    $ flight         <int> 1545, 1714, 1141, 725, 461, 1696, 507, 5708, 79, 301, 4…
    $ tailnum        <chr> "N14228", "N24211", "N619AA", "N804JB", "N668DN", "N394…
    $ origin         <chr> "EWR", "LGA", "JFK", "JFK", "LGA", "EWR", "EWR", "LGA",…
    $ dest           <chr> "IAH", "IAH", "MIA", "BQN", "ATL", "ORD", "FLL", "IAD",…
    $ air_time       <dbl> 227, 227, 160, 183, 116, 150, 158, 53, 140, 138, 149, 1…
    $ distance       <dbl> 1400, 1416, 1089, 1576, 762, 719, 1065, 229, 944, 733, …
    $ hour           <dbl> 5, 5, 5, 5, 6, 5, 6, 6, 6, 6, 6, 6, 6, 6, 6, 5, 6, 6, 6…
    $ minute         <dbl> 15, 29, 40, 45, 0, 58, 0, 0, 0, 0, 0, 0, 0, 0, 0, 59, 0…
    $ time_hour      <dttm> 2013-01-01 05:00:00, 2013-01-01 05:00:00, 2013-01-01 0…

### 5. Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?

``` r
table(flights$carrier) 
```


       9E    AA    AS    B6    DL    EV    F9    FL    HA    MQ    OO    UA    US 
    18460 32729   714 54635 48110 54173   685  3260   342 26397    32 58665 20536 
       VX    WN    YV 
     5162 12275   601 

### 6. Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

as.character(df) - перевод в строки

``` r
df_id_john <- airports %>%
  filter(name == "John F Kennedy Intl") %>%
  select(faa)
flights %>%
  filter(month == 5 & dest == as.character(df_id_john)) %>%
  nrow()
```

    [1] 0

### 7. Какой самый северный аэропорт?

``` r
north_airport <- airports %>% filter(lat == max(lat))
north_airport
```

    # A tibble: 1 × 8
      faa   name                      lat   lon   alt    tz dst   tzone
      <chr> <chr>                   <dbl> <dbl> <dbl> <dbl> <chr> <chr>
    1 EEN   Dillant Hopkins Airport  72.3  42.9   149    -5 A     <NA> 

### 8. Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?

``` r
high_mount <- airports %>% filter(alt == max(alt))
high_mount
```

    # A tibble: 1 × 8
      faa   name        lat   lon   alt    tz dst   tzone         
      <chr> <chr>     <dbl> <dbl> <dbl> <dbl> <chr> <chr>         
    1 TEX   Telluride  38.0 -108.  9078    -7 A     America/Denver

### 9. Какие бортовые номера у самых старых самолетов?

``` r
planes %>%arrange(year) %>% head(10) %>% select(tailnum)
```

    # A tibble: 10 × 1
       tailnum
       <chr>  
     1 N381AA 
     2 N201AA 
     3 N567AA 
     4 N378AA 
     5 N575AA 
     6 N14629 
     7 N615AA 
     8 N425AA 
     9 N383AA 
    10 N364AA 

### 10. Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия).

``` r
df_id_john <- airports %>% filter(name == "John F Kennedy Intl") %>% select(faa)
weather %>% filter(origin == as.character(df_id_john) & month == 9) %>%summarize(mean_temp = mean(temp, na.rm = TRUE))
```

    # A tibble: 1 × 1
      mean_temp
          <dbl>
    1      66.9

### 11. Самолеты какой авиакомпании совершили больше всего вылетов в июне?

n() нужно для подсчета наблюдений по группам

``` r
flights %>%
  filter(month == 6) %>%
  group_by(carrier) %>%
  summarize(total_flights = n()) %>% #подсчет наблюдений по группам
  arrange(desc(total_flights)) %>%
  head(1)
```

    # A tibble: 1 × 2
      carrier total_flights
      <chr>           <int>
    1 UA               4975

### 12. Самолеты какой авиакомпании задерживались чаще других в 2013 году?

``` r
flights %>%
  filter(year == 2013) %>%
  group_by(carrier) %>%
  summarize(zdrk = sum(arr_delay > 0, na.rm = TRUE)) %>%
  arrange(desc(zdrk)) %>%
  head(1)
```

    # A tibble: 1 × 2
      carrier  zdrk
      <chr>   <int>
    1 EV      24484

## Оценка результатов

Задания из пакета nycflights13 были успешно решены с помощью библиотеки
dplyr.

## Вывод

Были изучены методы анализа данных при помощи пакета dplyr.
