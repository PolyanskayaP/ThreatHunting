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

### 1: Basic Building Blocks

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

``` r
z * 2 + 1000
```

    [1] 1002.20 1018.00 1006.28

``` r
my_div
```

    [1] 3.478505 3.181981 2.146460

### 2: Workspace and Files

Determine which directory your R session is using as its current working
directory using getwd().

``` r
getwd()
```

    [1] "C:/!Полина/!7семестр/ИАТПУИБ/ThreatHunting/lab1"

List all the objects in your local workspace using ls().

``` r
ls()
```

    [1] "has_annotations" "my_div"          "my_sqrt"         "x"              
    [5] "y"               "z"              

``` r
x <- 9
```

``` r
ls()
```

    [1] "has_annotations" "my_div"          "my_sqrt"         "x"              
    [5] "y"               "z"              

List all the files in your working directory using list.files() or
dir().

``` r
dir()
```

    [1] "lab1.qmd"       "lab1.rmarkdown" "README.md"     

``` r
?list.files
```

Using the args() function on a function name is also a handy way to see
what arguments a function can take.

``` r
args(list.files) 
```

    function (path = ".", pattern = NULL, all.files = FALSE, full.names = FALSE, 
        recursive = FALSE, ignore.case = FALSE, include.dirs = FALSE, 
        no.. = FALSE) 
    NULL

Assign the value of the current working directory to a variable called
“old.dir”.

``` r
old.dir <- getwd()
```

Use dir.create() to create a directory in the current working directory
called “testdir”.

``` r
dir.create("testdir")
```

Use setwd(“testdir”) to set your working directory to “testdir”.

``` r
setwd("testdir")
```

Create a file in your working directory called “mytest.R” using the
file.create() function.

``` r
file.create("mytest.R")
```

    [1] TRUE

Let’s check this by listing all the files in the current directory.

``` r
list.files() 
```

    [1] "lab1.qmd"       "lab1.rmarkdown" "mytest.R"       "README.md"     
    [5] "testdir"       

Check to see if “mytest.R” exists in the working directory using the
file.exists() function.

``` r
file.exists("mytest.R")
```

    [1] TRUE

Access information about the file “mytest.R” by using file.info().

``` r
file.info("mytest.R")
```

             size isdir mode               mtime               ctime
    mytest.R    0 FALSE  666 2023-09-14 15:05:49 2023-09-14 15:05:49
                           atime exe
    mytest.R 2023-09-14 15:05:49  no

Change the name of the file “mytest.R” to “mytest2.R” by using
file.rename().

``` r
file.rename("mytest.R", "mytest2.R")
```

    [1] TRUE

Make a copy of “mytest2.R” called “mytest3.R” using file.copy().

``` r
file.copy("mytest2.R", "mytest3.R")
```

    [1] TRUE

Provide the relative path to the file “mytest3.R” by using file.path().

``` r
file.path("mytest3.R")
```

    [1] "mytest3.R"

You can use file.path to construct file and directory paths that are
independent of the operating system your R code is running on. Pass
‘folder1’ and ‘folder2’ as arguments to file.path to make a
platform-independent pathname.

``` r
file.path("folder1", "folder2")
```

    [1] "folder1/folder2"

Take a look at the documentation for dir.create by entering ?dir.create
. Notice the ‘recursive’ argument. In order to create nested
directories, ‘recursive’ must be set to TRUE.

``` r
?dir.create
```

Create a directory in the current working directory called “testdir2”
and a subdirectory for it called “testdir3”, all in one command by using
dir.create() and file.path().

``` r
dir.create(file.path('testdir2', 'testdir3'), recursive = TRUE)
```

Go back to your original working directory using setwd(). (Recall that
we created the variable old.dir with the full path for the orginal
working directory at the start of these questions.)

``` r
setwd(old.dir)
```

### 3: Sequences of Numbers
