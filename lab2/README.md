# lab2
Полянская Полина Алексеевна

Основы обработки данных с помощью R

## Цель

1.  Развить практические навыки использования языка программирования R
    для обработки данных

2.  Закрепить знания базовых типов данных языка

3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  Ноутбук с ОС Windows 10
2.  RStudio

## Задание

Для загрузки библиотеки использовалась команда library(dplyr)

``` r
library(dplyr)
```

    Warning: пакет 'dplyr' был собран под R версии 4.2.3


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

Проанализировать встроенный в пакет dplyr набор данных starwars с
помощью языка R и ответить на вопросы:

### 1. Сколько строк в датафрейме?

``` r
starwars %>% nrow()
```

    [1] 87

### 2. Сколько столбцов в датафрейме?

``` r
starwars %>% ncol()
```

    [1] 14

### 3. Как просмотреть примерный вид датафрейма?

``` r
starwars %>% glimpse()
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
    $ films      <list> <"The Empire Strikes Back", "Revenge of the Sith", "Return…
    $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imp…
    $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1",…

### 4. Сколько уникальных рас персонажей (species) представлено в данных?

``` r
starwars %>% distinct(species) %>% n_distinct()
```

    [1] 38

### 5. Найти самого высокого персонажа.

``` r
starwars %>% filter(height == max(height, na.rm = TRUE))
```

    # A tibble: 1 × 14
      name      height  mass hair_color skin_color eye_color birth_year sex   gender
      <chr>      <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
    1 Yarael P…    264    NA none       white      yellow            NA male  mascu…
    # ℹ 5 more variables: homeworld <chr>, species <chr>, films <list>,
    #   vehicles <list>, starships <list>

### 6. Найти всех персонажей ниже 170

``` r
starwars %>% filter(height < 170)
```

    # A tibble: 23 × 14
       name     height  mass hair_color skin_color eye_color birth_year sex   gender
       <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
     1 C-3PO       167    75 <NA>       gold       yellow           112 none  mascu…
     2 R2-D2        96    32 <NA>       white, bl… red               33 none  mascu…
     3 Leia Or…    150    49 brown      light      brown             19 fema… femin…
     4 Beru Wh…    165    75 brown      light      blue              47 fema… femin…
     5 R5-D4        97    32 <NA>       white, red red               NA none  mascu…
     6 Yoda         66    17 white      green      brown            896 male  mascu…
     7 Mon Mot…    150    NA auburn     fair       blue              48 fema… femin…
     8 Wicket …     88    20 brown      brown      brown              8 male  mascu…
     9 Nien Nu…    160    68 none       grey       black             NA male  mascu…
    10 Watto       137    NA black      blue, grey yellow            NA male  mascu…
    # ℹ 13 more rows
    # ℹ 5 more variables: homeworld <chr>, species <chr>, films <list>,
    #   vehicles <list>, starships <list>

### 7. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать по формуле 𝐼 = 𝑚 / ℎ^2 , где 𝑚 – масса (weight), а ℎ – рост (height).

### 8. Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по отношению массы (mass) к росту

(height) персонажей.

### 9. Найти средний возраст персонажей каждой расы вселенной Звездных войн.

``` r
starwars %>% group_by(species) %>% summarize(sr_vozr = mean(birth_year, na.rm = TRUE))
```

    # A tibble: 38 × 2
       species   sr_vozr
       <chr>       <dbl>
     1 Aleena      NaN  
     2 Besalisk    NaN  
     3 Cerean       92  
     4 Chagrian    NaN  
     5 Clawdite    NaN  
     6 Droid        53.3
     7 Dug         NaN  
     8 Ewok          8  
     9 Geonosian   NaN  
    10 Gungan       52  
    # ℹ 28 more rows

### 10. Найти самый распространенный цвет глаз персонажей вселенной Звездных войн.

``` r
starwars %>% group_by(eye_color) %>% summarize(count = n()) %>% arrange(desc(count)) %>% head(1)
```

    # A tibble: 1 × 2
      eye_color count
      <chr>     <int>
    1 brown        21

### 11. Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн.

``` r
starwars %>% group_by(species) %>% summarize(sred_dlin_name = mean(nchar(name), na.rm = TRUE)) 
```

    # A tibble: 38 × 2
       species   sred_dlin_name
       <chr>              <dbl>
     1 Aleena             13   
     2 Besalisk           15   
     3 Cerean             12   
     4 Chagrian           10   
     5 Clawdite           10   
     6 Droid               4.83
     7 Dug                 7   
     8 Ewok               21   
     9 Geonosian          17   
    10 Gungan             11.7 
    # ℹ 28 more rows

## Оценка результатов

Задания были успешно решены с помощью библиотеки dplyr и языка
программирования R.

## Вывод

Я ознакомилась с библиотекой dplyr. Были изучены методы анализа данных
при помощи этой библиотеки.
