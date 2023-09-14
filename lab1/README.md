# lab1
Полянская Полина Алексеевна

Прохождение курса по R в RStudio через пакет swirl (R Programming: The
basics of programming in R)

## Цель

1.  Познакомится с синтаксисом языка программирования R

2.  Оформить отчет

## Исходные данные

1.  Ноутбук с ОС Windows 10
2.  RStudio
3.  Пакет swirl

## Решение задачи

Нужно ввести swirl::swirl()

``` r
5 + 7
```

    [1] 12

``` r
x <- 5 + 7
x
```

    [1] 12

``` r
y <- x - 3
y
```

    [1] 9

``` r
z <- c(1.1, 9, 3.14)
```

``` r
?c
```

    запускаю httpd сервер помощи... готово

``` r
z
```

    [1] 1.10 9.00 3.14

``` r
c(z, 555, z)
```

    [1]   1.10   9.00   3.14 555.00   1.10   9.00   3.14

``` r
z * 2 + 100
```

    [1] 102.20 118.00 106.28

``` r
my_sqrt <- sqrt(z - 1)
```

Before we view the contents of the my_sqrt variable, what do you think
it contains?

1: a vector of length 0 (i.e. an empty vector) : a single number (i.e a
vector of length 1) : a vector of length 3

3

``` r
my_sqrt
```

    [1] 0.3162278 2.8284271 1.4628739

``` r
my_div <- z / my_sqrt
```

Which statement do you think is true?

1: The first element of my_div is equal to the first element of z
divided by the first element of my_sqrt, and so on… 2: my_div is
undefined 3: my_div is a single number (i.e a vector of length 1)

Выбор:1

``` r
my_div
```

    [1] 3.478505 3.181981 2.146460

``` r
c(1, 2, 3, 4)
```

    [1] 1 2 3 4

``` r
c(1, 2, 3, 4) + c(0, 10)
```

    [1]  1 12  3 14

``` r
c(1, 2, 3, 4) + c(0, 10, 100)
```

    Warning in c(1, 2, 3, 4) + c(0, 10, 100): длина большего объекта не является
    произведением длины меньшего объекта

    [1]   1  12 103   4
