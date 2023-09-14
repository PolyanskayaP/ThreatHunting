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

    [1] "lab1.qmd"       "lab1.rmarkdown" "mytest2.R"      "mytest3.R"     
    [5] "README.md"     

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

    [1] "lab1.qmd"       "lab1.rmarkdown" "mytest.R"       "mytest2.R"     
    [5] "mytest3.R"      "README.md"      "testdir"       

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
    mytest.R    0 FALSE  666 2023-09-14 15:40:22 2023-09-14 15:40:22
                           atime exe
    mytest.R 2023-09-14 15:40:22  no

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

    [1] FALSE

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

The simplest way to create a sequence of numbers in R is by using the
`:` operator. Type 1:20 to see how it works.

``` r
1:20
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

``` r
pi:10
```

    [1] 3.141593 4.141593 5.141593 6.141593 7.141593 8.141593 9.141593

``` r
15:1
```

     [1] 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1

``` r
help(`:`)
```

``` r
?`:`
```

``` r
seq(1, 20)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

``` r
seq(0, 10, by=0.5)
```

     [1]  0.0  0.5  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0  6.5  7.0
    [16]  7.5  8.0  8.5  9.0  9.5 10.0

``` r
my_seq <- seq(5, 10, length=30)
```

``` r
length(my_seq)
```

    [1] 30

``` r
1:length(my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

``` r
seq(along.with = my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

``` r
seq_along(my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

``` r
rep(0, times = 40)
```

     [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
    [39] 0 0

``` r
rep(c(0, 1, 2), times = 10)
```

     [1] 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2

``` r
rep(c(0, 1, 2), each = 10)
```

     [1] 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2

### 4: Vectors

``` r
num_vect <- c(0.5, 55, -10, 6)
```

Use tf \<- num_vect \< 1 to assign the result of num_vect \< 1 to a
variable called tf.

``` r
tf <- num_vect < 1
```

What do you think tf will look like?

1: a single logical value 2: a vector of 4 logical values

2

``` r
tf
```

    [1]  TRUE FALSE  TRUE FALSE

``` r
num_vect >= 6
```

    [1] FALSE  TRUE FALSE  TRUE

(3 \> 5) & (4 == 4)

1: TRUE 2: FALSE

Выбор:2

(TRUE == TRUE) | (TRUE == FALSE)

1: FALSE 2: TRUE

Выбор:2

((111 \>= 111) | !(TRUE)) & ((4 + 1) == 5)

1: FALSE 2: TRUE

Выбор:2

``` r
my_char <- c("My", "name", "is")
```

``` r
my_char
```

    [1] "My"   "name" "is"  

``` r
paste(my_char, collapse = " ")
```

    [1] "My name is"

``` r
my_name <- c(my_char, "Polina")
```

``` r
my_name
```

    [1] "My"     "name"   "is"     "Polina"

Now, use the paste() function once more to join the words in my_name
together into a single character string. Don’t forget to say collapse =
” “!

``` r
paste(my_name, collapse = " ")
```

    [1] "My name is Polina"

``` r
paste("Hello", "world!", sep = " ")
```

    [1] "Hello world!"

``` r
paste(1:3, c("X", "Y", "Z"), sep = "")
```

    [1] "1X" "2Y" "3Z"

``` r
paste(LETTERS, 1:4, sep = "-")
```

     [1] "A-1" "B-2" "C-3" "D-4" "E-1" "F-2" "G-3" "H-4" "I-1" "J-2" "K-3" "L-4"
    [13] "M-1" "N-2" "O-3" "P-4" "Q-1" "R-2" "S-3" "T-4" "U-1" "V-2" "W-3" "X-4"
    [25] "Y-1" "Z-2"

## Вывод

Я ознакомилась с основным синтаксисом языка R. Было пройдено 4 урока из
пакета swirl.
