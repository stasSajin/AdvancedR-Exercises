Quiz
----

### 1 Fix each of the following common data frame subsetting errors:

``` r
#mtcars[mtcars$cyl = 4, ]
mtcars[mtcars$cyl == 4, ]
```

    ##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    ## Toyota Corona  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2

``` r
#mtcars[-1:4, ]
mtcars[1:4, ] #don't mix negative and positive subsetting
```

    ##                 mpg cyl disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4      21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag  21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710     22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1

``` r
#mtcars[-1:4, ]
mtcars[mtcars$cyl <= 5,] #needs comma for logical subsetting
```

    ##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    ## Toyota Corona  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2

``` r
#mtcars[mtcars$cyl == 4 | 6, ]
mtcars[mtcars$cyl == 4 | mtcars$cyl == 6, ]
```

    ##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4      21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## Valiant        18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
    ## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## Merc 280       19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
    ## Merc 280C      17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
    ## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    ## Toyota Corona  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## Ferrari Dino   19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    ## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2

### 2 Why does x &lt;- 1:5; `x[NA]` yield five missing values? (Hint: why is it different from `x[NA_real_]`?)

the `x[]` returns the original vector. Since the index is NA, it returns NA for each of the values in the original vector

the `x[NA_real_]` returns a one length vector, since the NA here is a typeof numeric. Hence, the NA subsetting returns NA for only one value in the vector.

### 2.3 When should you use drop = FALSE?

It is used when you want to keep the structure of the output the same as the input. Hadley seems to strongly recommend that drop=FALSE be used for factor/array/dataframe subsetting.

### 3 If x is a matrix, what does x\[\] &lt;- 0 do? How is it different to x &lt;- 0?

``` r
#create a 3x3 matrix
x<-matrix(data=c(1:9),nrow = 3,ncol=3)
x
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9

``` r
#this should replace everything with 0
x[] <- 0 
x
```

    ##      [,1] [,2] [,3]
    ## [1,]    0    0    0
    ## [2,]    0    0    0
    ## [3,]    0    0    0

``` r
#this replaces the matrix stored in x with a 0.
x <- 0
x
```

    ## [1] 0

### 4 How can you use a named vector to relabel categorical variables?

I'm not sure what is being asked here.

Exercises
---------

### 1--Same as quiz question 1

### 2--Same as quiz question 2

### 3 What does `upper.tri()` return? How does subsetting a matrix with it work? Do we need any additional subsetting rules to describe its behaviour?

``` r
#upper.tri() returns  a matrix of logicals the same size as the given matrix in the lower or upper triangle. Using the upper.tri() for subsetting will return the values for the upper triangle. We can specify the rule for whether it should also subset the diagonal or not.
x <- outer(1:5, 1:5, FUN = "*")
x
```

    ##      [,1] [,2] [,3] [,4] [,5]
    ## [1,]    1    2    3    4    5
    ## [2,]    2    4    6    8   10
    ## [3,]    3    6    9   12   15
    ## [4,]    4    8   12   16   20
    ## [5,]    5   10   15   20   25

``` r
x[upper.tri(x)]
```

    ##  [1]  2  3  6  4  8 12  5 10 15 20

### 4 Why does mtcars\[1:20\] return an error? How does it differ from the similar mtcars\[1:20, \]?

It returns an error because the column is not specified. The second expression specifies that all colums should be returned.

### 5 Implement your own function that extracts the diagonal entries from a matrix (it should behave like diag(x) where x is a matrix).

``` r
x<-matrix(1:16,4)

diagonal<- function(matrix){
    x<-c() #create empty vector
    for (i in 1:nrow(matrix)) {
        x <- c(x, matrix[i,i])
    }
    return(x)
}

x
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16

``` r
diagonal(x)
```

    ## [1]  1  6 11 16

``` r
diag(x)
```

    ## [1]  1  6 11 16

### 6 What does `df[is.na(df)] <- 0` do? How does it work?

The function replaces the NAs in the dataframe with 0. First, `is.na(df)` identifies the indixes for the cells that have NA. It returns TRUE for every cell in the df that has NA and FALSE otherwise. The \[\], then simply subsets the TRUE values. the `<-0` replaces the subsetted values with 0.

### 7. Given a linear model, e.g., mod &lt;- lm(mpg ~ wt, data = mtcars), extract the residual degrees of freedom. Extract the R squared from the model summary (summary(mod))

``` r
mod <- lm(mpg ~ wt, data = mtcars)

#extract df.residual
mod$df.residual
```

    ## [1] 30

``` r
#extract r-squared value
summary(mod)$r.squared
```

    ## [1] 0.7528328

### 8. How would you randomly permute the columns of a data frame? (This is an important technique in random forests.) Can you simultaneously permute the rows and columns in one step?

``` r
#let's create a dataframe first
df <- data.frame(matrix(1:100,10))
df
```

    ##    X1 X2 X3 X4 X5 X6 X7 X8 X9 X10
    ## 1   1 11 21 31 41 51 61 71 81  91
    ## 2   2 12 22 32 42 52 62 72 82  92
    ## 3   3 13 23 33 43 53 63 73 83  93
    ## 4   4 14 24 34 44 54 64 74 84  94
    ## 5   5 15 25 35 45 55 65 75 85  95
    ## 6   6 16 26 36 46 56 66 76 86  96
    ## 7   7 17 27 37 47 57 67 77 87  97
    ## 8   8 18 28 38 48 58 68 78 88  98
    ## 9   9 19 29 39 49 59 69 79 89  99
    ## 10 10 20 30 40 50 60 70 80 90 100

``` r
#let's randomply shuffle the rows
df[sample(nrow(df)),]
```

    ##    X1 X2 X3 X4 X5 X6 X7 X8 X9 X10
    ## 10 10 20 30 40 50 60 70 80 90 100
    ## 3   3 13 23 33 43 53 63 73 83  93
    ## 4   4 14 24 34 44 54 64 74 84  94
    ## 9   9 19 29 39 49 59 69 79 89  99
    ## 5   5 15 25 35 45 55 65 75 85  95
    ## 1   1 11 21 31 41 51 61 71 81  91
    ## 6   6 16 26 36 46 56 66 76 86  96
    ## 7   7 17 27 37 47 57 67 77 87  97
    ## 8   8 18 28 38 48 58 68 78 88  98
    ## 2   2 12 22 32 42 52 62 72 82  92

``` r
#now both rows and columns
df[sample(nrow(df)),sample(ncol(df))]
```

    ##    X6 X3 X5 X2 X7 X8 X4 X9 X10 X1
    ## 7  57 27 47 17 67 77 37 87  97  7
    ## 1  51 21 41 11 61 71 31 81  91  1
    ## 3  53 23 43 13 63 73 33 83  93  3
    ## 2  52 22 42 12 62 72 32 82  92  2
    ## 8  58 28 48 18 68 78 38 88  98  8
    ## 10 60 30 50 20 70 80 40 90 100 10
    ## 5  55 25 45 15 65 75 35 85  95  5
    ## 6  56 26 46 16 66 76 36 86  96  6
    ## 9  59 29 49 19 69 79 39 89  99  9
    ## 4  54 24 44 14 64 74 34 84  94  4

### 9. How would you select a random sample of m rows from a data frame? What if the sample had to be contiguous (i.e., with an initial row, a final row, and every row in between)?

``` r
#select 6 rows
m<-6
df[sample(nrow(df),m),]
```

    ##   X1 X2 X3 X4 X5 X6 X7 X8 X9 X10
    ## 3  3 13 23 33 43 53 63 73 83  93
    ## 6  6 16 26 36 46 56 66 76 86  96
    ## 4  4 14 24 34 44 54 64 74 84  94
    ## 5  5 15 25 35 45 55 65 75 85  95
    ## 1  1 11 21 31 41 51 61 71 81  91
    ## 9  9 19 29 39 49 59 69 79 89  99

``` r
#sample the first position of a row
firstPosition<-sample(nrow(df)-m+1,1)
lastPosition<-firstPosition-1+m
df[firstPosition:lastPosition,]
```

    ##   X1 X2 X3 X4 X5 X6 X7 X8 X9 X10
    ## 1  1 11 21 31 41 51 61 71 81  91
    ## 2  2 12 22 32 42 52 62 72 82  92
    ## 3  3 13 23 33 43 53 63 73 83  93
    ## 4  4 14 24 34 44 54 64 74 84  94
    ## 5  5 15 25 35 45 55 65 75 85  95
    ## 6  6 16 26 36 46 56 66 76 86  96

### 10. How could you put the columns in a data frame in alphabetical order?

I would use something along the lines: `names(df) <- order(names(df))`
