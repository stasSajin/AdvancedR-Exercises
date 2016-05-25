### 1. Why are the following two invocations of lapply() equivalent?

In the second lapply, the first mean() argument get's interpreted as a value for trim. This is because x is already beeing supplied.

``` r
trims <- c(0, 0.1, 0.2, 0.5)
x <- rcauchy(100)

lapply(trims, function(trim) mean(x, trim = trim))
```

    ## [[1]]
    ## [1] -1.427089
    ## 
    ## [[2]]
    ## [1] 0.1658607
    ## 
    ## [[3]]
    ## [1] 0.07378269
    ## 
    ## [[4]]
    ## [1] -0.02796707

``` r
lapply(trims, mean, x = x)
```

    ## [[1]]
    ## [1] -1.427089
    ## 
    ## [[2]]
    ## [1] 0.1658607
    ## 
    ## [[3]]
    ## [1] 0.07378269
    ## 
    ## [[4]]
    ## [1] -0.02796707

### 2. The function below scales a vector so it falls in the range \[0, 1\]. How would you apply it to every column of a data frame? How would you apply it to every numeric column in a data frame?

``` r
scale01 <- function(x) {
  rng <- range(x, na.rm = TRUE)
  (x - rng[1]) / (rng[2] - rng[1])
}

#sapply automatically applies the functions over colums, so:
sapply(mtcars, scale01)
```

    ##             mpg cyl       disp         hp       drat         wt       qsec
    ##  [1,] 0.4510638 0.5 0.22175106 0.20494700 0.52534562 0.28304781 0.23333333
    ##  [2,] 0.4510638 0.5 0.22175106 0.20494700 0.52534562 0.34824853 0.30000000
    ##  [3,] 0.5276596 0.0 0.09204290 0.14487633 0.50230415 0.20634109 0.48928571
    ##  [4,] 0.4680851 0.5 0.46620105 0.20494700 0.14746544 0.43518282 0.58809524
    ##  [5,] 0.3531915 1.0 0.72062859 0.43462898 0.17972350 0.49271286 0.30000000
    ##  [6,] 0.3276596 0.5 0.38388626 0.18727915 0.00000000 0.49782664 0.68095238
    ##  [7,] 0.1659574 1.0 0.72062859 0.68197880 0.20737327 0.52595244 0.15952381
    ##  [8,] 0.5957447 0.0 0.18857570 0.03533569 0.42857143 0.42879059 0.65476190
    ##  [9,] 0.5276596 0.0 0.17385882 0.15194346 0.53456221 0.41856303 1.00000000
    ## [10,] 0.3744681 0.5 0.24070841 0.25088339 0.53456221 0.49271286 0.45238095
    ## [11,] 0.3148936 0.5 0.24070841 0.25088339 0.53456221 0.49271286 0.52380952
    ## [12,] 0.2553191 1.0 0.51060115 0.45229682 0.14285714 0.65379698 0.34523810
    ## [13,] 0.2936170 1.0 0.51060115 0.45229682 0.14285714 0.56686269 0.36904762
    ## [14,] 0.2042553 1.0 0.51060115 0.45229682 0.14285714 0.57964715 0.41666667
    ## [15,] 0.0000000 1.0 1.00000000 0.54063604 0.07834101 0.95551010 0.41428571
    ## [16,] 0.0000000 1.0 0.97006735 0.57597173 0.11059908 1.00000000 0.39523810
    ## [17,] 0.1829787 1.0 0.92017960 0.62897527 0.21658986 0.97980056 0.34761905
    ## [18,] 0.9361702 0.0 0.01895735 0.04946996 0.60829493 0.17565840 0.59166667
    ## [19,] 0.8510638 0.0 0.01147418 0.00000000 1.00000000 0.02608029 0.47857143
    ## [20,] 1.0000000 0.0 0.00000000 0.04593640 0.67281106 0.08233188 0.64285714
    ## [21,] 0.4723404 0.0 0.12222499 0.15901060 0.43317972 0.24341601 0.65595238
    ## [22,] 0.2170213 1.0 0.61586431 0.34628975 0.00000000 0.51316799 0.28214286
    ## [23,] 0.2042553 1.0 0.58094288 0.34628975 0.17972350 0.49143442 0.33333333
    ## [24,] 0.1234043 1.0 0.69568471 0.68197880 0.44700461 0.59498849 0.10833333
    ## [25,] 0.3744681 1.0 0.82040409 0.43462898 0.14746544 0.59626694 0.30357143
    ## [26,] 0.7191489 0.0 0.01970566 0.04946996 0.60829493 0.10790079 0.52380952
    ## [27,] 0.6638298 0.0 0.12272387 0.13780919 0.76958525 0.16031705 0.26190476
    ## [28,] 0.8510638 0.0 0.05986530 0.21554770 0.46543779 0.00000000 0.28571429
    ## [29,] 0.2297872 1.0 0.69817910 0.74911661 0.67281106 0.42367681 0.00000000
    ## [30,] 0.3957447 0.5 0.18433525 0.43462898 0.39631336 0.32140118 0.11904762
    ## [31,] 0.1957447 1.0 0.57345972 1.00000000 0.35944700 0.52595244 0.01190476
    ## [32,] 0.4680851 0.0 0.12446994 0.20141343 0.62211982 0.32395807 0.48809524
    ##       vs am gear      carb
    ##  [1,]  0  1  0.5 0.4285714
    ##  [2,]  0  1  0.5 0.4285714
    ##  [3,]  1  1  0.5 0.0000000
    ##  [4,]  1  0  0.0 0.0000000
    ##  [5,]  0  0  0.0 0.1428571
    ##  [6,]  1  0  0.0 0.0000000
    ##  [7,]  0  0  0.0 0.4285714
    ##  [8,]  1  0  0.5 0.1428571
    ##  [9,]  1  0  0.5 0.1428571
    ## [10,]  1  0  0.5 0.4285714
    ## [11,]  1  0  0.5 0.4285714
    ## [12,]  0  0  0.0 0.2857143
    ## [13,]  0  0  0.0 0.2857143
    ## [14,]  0  0  0.0 0.2857143
    ## [15,]  0  0  0.0 0.4285714
    ## [16,]  0  0  0.0 0.4285714
    ## [17,]  0  0  0.0 0.4285714
    ## [18,]  1  1  0.5 0.0000000
    ## [19,]  1  1  0.5 0.1428571
    ## [20,]  1  1  0.5 0.0000000
    ## [21,]  1  0  0.0 0.0000000
    ## [22,]  0  0  0.0 0.1428571
    ## [23,]  0  0  0.0 0.1428571
    ## [24,]  0  0  0.0 0.4285714
    ## [25,]  0  0  0.0 0.1428571
    ## [26,]  1  1  0.5 0.0000000
    ## [27,]  0  1  1.0 0.1428571
    ## [28,]  1  1  1.0 0.1428571
    ## [29,]  0  1  1.0 0.4285714
    ## [30,]  0  1  1.0 0.7142857
    ## [31,]  0  1  1.0 1.0000000
    ## [32,]  1  1  0.5 0.1428571

``` r
#applying it over numeric colums; I'll nest two numeric functions; I'll put a conditional in the anonymous function.
sapply(mtcars, function(x) { if (is.numeric(x)) scale01(x) else x })
```

    ##             mpg cyl       disp         hp       drat         wt       qsec
    ##  [1,] 0.4510638 0.5 0.22175106 0.20494700 0.52534562 0.28304781 0.23333333
    ##  [2,] 0.4510638 0.5 0.22175106 0.20494700 0.52534562 0.34824853 0.30000000
    ##  [3,] 0.5276596 0.0 0.09204290 0.14487633 0.50230415 0.20634109 0.48928571
    ##  [4,] 0.4680851 0.5 0.46620105 0.20494700 0.14746544 0.43518282 0.58809524
    ##  [5,] 0.3531915 1.0 0.72062859 0.43462898 0.17972350 0.49271286 0.30000000
    ##  [6,] 0.3276596 0.5 0.38388626 0.18727915 0.00000000 0.49782664 0.68095238
    ##  [7,] 0.1659574 1.0 0.72062859 0.68197880 0.20737327 0.52595244 0.15952381
    ##  [8,] 0.5957447 0.0 0.18857570 0.03533569 0.42857143 0.42879059 0.65476190
    ##  [9,] 0.5276596 0.0 0.17385882 0.15194346 0.53456221 0.41856303 1.00000000
    ## [10,] 0.3744681 0.5 0.24070841 0.25088339 0.53456221 0.49271286 0.45238095
    ## [11,] 0.3148936 0.5 0.24070841 0.25088339 0.53456221 0.49271286 0.52380952
    ## [12,] 0.2553191 1.0 0.51060115 0.45229682 0.14285714 0.65379698 0.34523810
    ## [13,] 0.2936170 1.0 0.51060115 0.45229682 0.14285714 0.56686269 0.36904762
    ## [14,] 0.2042553 1.0 0.51060115 0.45229682 0.14285714 0.57964715 0.41666667
    ## [15,] 0.0000000 1.0 1.00000000 0.54063604 0.07834101 0.95551010 0.41428571
    ## [16,] 0.0000000 1.0 0.97006735 0.57597173 0.11059908 1.00000000 0.39523810
    ## [17,] 0.1829787 1.0 0.92017960 0.62897527 0.21658986 0.97980056 0.34761905
    ## [18,] 0.9361702 0.0 0.01895735 0.04946996 0.60829493 0.17565840 0.59166667
    ## [19,] 0.8510638 0.0 0.01147418 0.00000000 1.00000000 0.02608029 0.47857143
    ## [20,] 1.0000000 0.0 0.00000000 0.04593640 0.67281106 0.08233188 0.64285714
    ## [21,] 0.4723404 0.0 0.12222499 0.15901060 0.43317972 0.24341601 0.65595238
    ## [22,] 0.2170213 1.0 0.61586431 0.34628975 0.00000000 0.51316799 0.28214286
    ## [23,] 0.2042553 1.0 0.58094288 0.34628975 0.17972350 0.49143442 0.33333333
    ## [24,] 0.1234043 1.0 0.69568471 0.68197880 0.44700461 0.59498849 0.10833333
    ## [25,] 0.3744681 1.0 0.82040409 0.43462898 0.14746544 0.59626694 0.30357143
    ## [26,] 0.7191489 0.0 0.01970566 0.04946996 0.60829493 0.10790079 0.52380952
    ## [27,] 0.6638298 0.0 0.12272387 0.13780919 0.76958525 0.16031705 0.26190476
    ## [28,] 0.8510638 0.0 0.05986530 0.21554770 0.46543779 0.00000000 0.28571429
    ## [29,] 0.2297872 1.0 0.69817910 0.74911661 0.67281106 0.42367681 0.00000000
    ## [30,] 0.3957447 0.5 0.18433525 0.43462898 0.39631336 0.32140118 0.11904762
    ## [31,] 0.1957447 1.0 0.57345972 1.00000000 0.35944700 0.52595244 0.01190476
    ## [32,] 0.4680851 0.0 0.12446994 0.20141343 0.62211982 0.32395807 0.48809524
    ##       vs am gear      carb
    ##  [1,]  0  1  0.5 0.4285714
    ##  [2,]  0  1  0.5 0.4285714
    ##  [3,]  1  1  0.5 0.0000000
    ##  [4,]  1  0  0.0 0.0000000
    ##  [5,]  0  0  0.0 0.1428571
    ##  [6,]  1  0  0.0 0.0000000
    ##  [7,]  0  0  0.0 0.4285714
    ##  [8,]  1  0  0.5 0.1428571
    ##  [9,]  1  0  0.5 0.1428571
    ## [10,]  1  0  0.5 0.4285714
    ## [11,]  1  0  0.5 0.4285714
    ## [12,]  0  0  0.0 0.2857143
    ## [13,]  0  0  0.0 0.2857143
    ## [14,]  0  0  0.0 0.2857143
    ## [15,]  0  0  0.0 0.4285714
    ## [16,]  0  0  0.0 0.4285714
    ## [17,]  0  0  0.0 0.4285714
    ## [18,]  1  1  0.5 0.0000000
    ## [19,]  1  1  0.5 0.1428571
    ## [20,]  1  1  0.5 0.0000000
    ## [21,]  1  0  0.0 0.0000000
    ## [22,]  0  0  0.0 0.1428571
    ## [23,]  0  0  0.0 0.1428571
    ## [24,]  0  0  0.0 0.4285714
    ## [25,]  0  0  0.0 0.1428571
    ## [26,]  1  1  0.5 0.0000000
    ## [27,]  0  1  1.0 0.1428571
    ## [28,]  1  1  1.0 0.1428571
    ## [29,]  0  1  1.0 0.4285714
    ## [30,]  0  1  1.0 0.7142857
    ## [31,]  0  1  1.0 1.0000000
    ## [32,]  1  1  0.5 0.1428571

### 3. Use both for loops and lapply() to fit linear models to the mtcars using the formulas stored in this list:

``` r
formulas <- list(
  mpg ~ disp,
  mpg ~ I(1 / disp),
  mpg ~ disp + wt,
  mpg ~ I(1 / disp) + wt
)

#using the lapply
lapply(formulas, lm, data = mtcars)
```

    ## [[1]]
    ## 
    ## Call:
    ## FUN(formula = X[[i]], data = ..1)
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    29.59985     -0.04122  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## FUN(formula = X[[i]], data = ..1)
    ## 
    ## Coefficients:
    ## (Intercept)    I(1/disp)  
    ##       10.75      1557.67  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## FUN(formula = X[[i]], data = ..1)
    ## 
    ## Coefficients:
    ## (Intercept)         disp           wt  
    ##    34.96055     -0.01772     -3.35083  
    ## 
    ## 
    ## [[4]]
    ## 
    ## Call:
    ## FUN(formula = X[[i]], data = ..1)
    ## 
    ## Coefficients:
    ## (Intercept)    I(1/disp)           wt  
    ##      19.024     1142.560       -1.798

``` r
#using loops
output2 <- vector('list', length(formulas))
for (i in seq_along(formulas)) { 
    output2[[i]] <- lm(formulas[[i]], data = mtcars)
}
output2
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = formulas[[i]], data = mtcars)
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    29.59985     -0.04122  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = formulas[[i]], data = mtcars)
    ## 
    ## Coefficients:
    ## (Intercept)    I(1/disp)  
    ##       10.75      1557.67  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = formulas[[i]], data = mtcars)
    ## 
    ## Coefficients:
    ## (Intercept)         disp           wt  
    ##    34.96055     -0.01772     -3.35083  
    ## 
    ## 
    ## [[4]]
    ## 
    ## Call:
    ## lm(formula = formulas[[i]], data = mtcars)
    ## 
    ## Coefficients:
    ## (Intercept)    I(1/disp)           wt  
    ##      19.024     1142.560       -1.798

### 4. Fit the model mpg ~ disp to each of the bootstrap replicates of mtcars in the list below by using a for loop and lapply(). Can you do it without an anonymous function?

``` r
bootstraps <- lapply(1:10, function(i) {
  rows <- sample(1:nrow(mtcars), rep = TRUE)
  mtcars[rows, ]
})

#with lapply(); should show the output of 10 bootstrapped models.
outputBoot <- lapply(bootstraps, lm, formula = mpg ~ disp)
outputBoot
```

    ## [[1]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    30.21107     -0.04511  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    30.93404     -0.04671  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    27.45218     -0.03648  
    ## 
    ## 
    ## [[4]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    31.07889     -0.04519  
    ## 
    ## 
    ## [[5]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##     27.5998      -0.0339  
    ## 
    ## 
    ## [[6]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    31.52052     -0.05101  
    ## 
    ## 
    ## [[7]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    29.01262     -0.03909  
    ## 
    ## 
    ## [[8]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    27.73408     -0.03563  
    ## 
    ## 
    ## [[9]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    28.94791     -0.03961  
    ## 
    ## 
    ## [[10]]
    ## 
    ## Call:
    ## FUN(formula = ..1, data = X[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    30.77666     -0.04323

``` r
#this time using a for loop
output <- vector('list', length(bootstraps))
for (i in seq_along(bootstraps)) { 
    outputBoot[[i]] <- lm(mpg ~ disp, data = bootstraps[[i]])
}
outputBoot
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    30.21107     -0.04511  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    30.93404     -0.04671  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    27.45218     -0.03648  
    ## 
    ## 
    ## [[4]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    31.07889     -0.04519  
    ## 
    ## 
    ## [[5]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##     27.5998      -0.0339  
    ## 
    ## 
    ## [[6]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    31.52052     -0.05101  
    ## 
    ## 
    ## [[7]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    29.01262     -0.03909  
    ## 
    ## 
    ## [[8]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    27.73408     -0.03563  
    ## 
    ## 
    ## [[9]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    28.94791     -0.03961  
    ## 
    ## 
    ## [[10]]
    ## 
    ## Call:
    ## lm(formula = mpg ~ disp, data = bootstraps[[i]])
    ## 
    ## Coefficients:
    ## (Intercept)         disp  
    ##    30.77666     -0.04323

### 5. For each model in the previous two exercises, extract R2 using the function below.

``` r
rsq <- function(mod) summary(mod)$r.squared

unlist(lapply(output2, rsq))
```

    ## [1] 0.7183433 0.8596865 0.7809306 0.8838038

``` r
unlist(lapply(outputBoot, rsq))
```

    ##  [1] 0.7461689 0.7662614 0.7104509 0.7248655 0.6982751 0.7226014 0.7982741
    ##  [8] 0.6487383 0.6716465 0.7079539

### 6. Use vapply() to:

1.  Compute the standard deviation of every column in a numeric data frame.

``` r
#vapply needs the specification of the output
vapply(mtcars, sd, numeric(1))
```

    ##         mpg         cyl        disp          hp        drat          wt 
    ##   6.0269481   1.7859216 123.9386938  68.5628685   0.5346787   0.9784574 
    ##        qsec          vs          am        gear        carb 
    ##   1.7869432   0.5040161   0.4989909   0.7378041   1.6152000

1.  Compute the standard deviation of every numeric column in a mixed data frame. (Hint: you'll need to use vapply() twice.)

``` r
vapply(mtcars[vapply(mtcars, is.numeric, logical(1))], sd, numeric(1))
```

    ##         mpg         cyl        disp          hp        drat          wt 
    ##   6.0269481   1.7859216 123.9386938  68.5628685   0.5346787   0.9784574 
    ##        qsec          vs          am        gear        carb 
    ##   1.7869432   0.5040161   0.4989909   0.7378041   1.6152000

### 7. Why is using sapply() to get the class() of each element in a data frame dangerous?

Some elements could have more than one class, and sapply() will silently return and empty list.

### 8. The following code simulates the performance of a t-test for non-normal data. Use sapply() and an anonymous function to extract the p-value from every trial.

``` r
trials <- replicate(
  100, 
  t.test(rpois(10, 10), rpois(7, 10)),
  simplify = FALSE
)

sapply(trials, function(x) x$p.value)
```

    ##   [1] 0.518674794 0.528286826 0.811002888 0.570326513 0.206405447
    ##   [6] 0.401119084 0.015070601 0.367835994 0.032463936 0.632592534
    ##  [11] 0.978225731 0.331095743 0.389974305 0.799554008 0.885675510
    ##  [16] 0.980122342 0.002478173 0.010551315 0.297452784 0.497516299
    ##  [21] 0.326417279 0.605449421 0.120294506 0.871975535 0.251134758
    ##  [26] 0.910475060 0.262584792 0.440035284 0.923235129 0.024260932
    ##  [31] 0.087024575 0.919482789 0.621426509 0.787818689 0.761334109
    ##  [36] 0.043282144 0.175124359 0.419119830 0.418383816 0.193054888
    ##  [41] 0.177167882 0.385676237 0.365925892 0.096001595 0.962555280
    ##  [46] 0.118600998 0.306496990 0.986929621 0.366926030 0.307895080
    ##  [51] 0.003734972 0.044436296 0.691551919 0.295940139 0.170689597
    ##  [56] 0.068916953 0.266755500 0.911452309 0.404921855 0.819455145
    ##  [61] 0.400071145 0.656014117 0.481865761 0.858993469 0.784124535
    ##  [66] 0.457519555 0.498844330 0.833767302 0.989283292 0.747577303
    ##  [71] 0.191822511 0.258117439 0.786242636 0.788513018 0.520602855
    ##  [76] 0.212414729 0.341428918 0.395453965 0.274492953 0.044755976
    ##  [81] 0.307827380 0.808263522 0.099898431 0.582495470 0.013302706
    ##  [86] 0.487690269 0.478325261 0.539710556 0.829396219 0.651395334
    ##  [91] 0.652410020 0.881199622 0.145900890 0.607307084 0.796853214
    ##  [96] 0.069699413 0.577873671 0.534975606 0.333822253 0.247983586

``` r
#Extra challenge: get rid of the anonymous function by using [[ directly.
sapply(trials, `[[`, "p.value")
```

    ##   [1] 0.518674794 0.528286826 0.811002888 0.570326513 0.206405447
    ##   [6] 0.401119084 0.015070601 0.367835994 0.032463936 0.632592534
    ##  [11] 0.978225731 0.331095743 0.389974305 0.799554008 0.885675510
    ##  [16] 0.980122342 0.002478173 0.010551315 0.297452784 0.497516299
    ##  [21] 0.326417279 0.605449421 0.120294506 0.871975535 0.251134758
    ##  [26] 0.910475060 0.262584792 0.440035284 0.923235129 0.024260932
    ##  [31] 0.087024575 0.919482789 0.621426509 0.787818689 0.761334109
    ##  [36] 0.043282144 0.175124359 0.419119830 0.418383816 0.193054888
    ##  [41] 0.177167882 0.385676237 0.365925892 0.096001595 0.962555280
    ##  [46] 0.118600998 0.306496990 0.986929621 0.366926030 0.307895080
    ##  [51] 0.003734972 0.044436296 0.691551919 0.295940139 0.170689597
    ##  [56] 0.068916953 0.266755500 0.911452309 0.404921855 0.819455145
    ##  [61] 0.400071145 0.656014117 0.481865761 0.858993469 0.784124535
    ##  [66] 0.457519555 0.498844330 0.833767302 0.989283292 0.747577303
    ##  [71] 0.191822511 0.258117439 0.786242636 0.788513018 0.520602855
    ##  [76] 0.212414729 0.341428918 0.395453965 0.274492953 0.044755976
    ##  [81] 0.307827380 0.808263522 0.099898431 0.582495470 0.013302706
    ##  [86] 0.487690269 0.478325261 0.539710556 0.829396219 0.651395334
    ##  [91] 0.652410020 0.881199622 0.145900890 0.607307084 0.796853214
    ##  [96] 0.069699413 0.577873671 0.534975606 0.333822253 0.247983586

### 9. What does replicate() do? What sort of for loop does it eliminate? Why do its arguments differ from lapply() and friends?

``` r
#?replicate()

##from documentation: "replicate is a wrapper for the common use of sapply for repeated evaluation of an expression (which will usually involve random number generation)."

#replicate is different from lapply because it isn't applying an expression to a number of inputs, but simply runs the expression a number of times. it eliminates the for loops for repeatedly evaluating an expression.
```

### 10. Implement a version of lapply() that supplies FUN with both the name and the value of each component.

``` r
#I'll do it later, not sure what is meant by component.
```

### 11. Implement a combination of Map() and vapply() to create an lapply() variant that iterates in parallel over all of its inputs and stores its outputs in a vector (or a matrix). What arguments should the function take?

``` r
#same issue
```

### 12. Implement mcsapply(), a multicore version of sapply(). Can you implement mcvapply(), a parallel version of vapply()? Why or why not?

``` r
#took answer from here: https://github.com/peterhurford/adv-r-book-solutions/blob/master/09_functionals/02_friends_of_lapply/exericse7.r

mcsapply <- function(X, FUN, ..., simplify = TRUE, USE.NAMES = TRUE) {
  FUN <- match.fun(FUN)
  answer <- parallel::mclapply(X = X, FUN = FUN, ...)
  if (USE.NAMES && is.character(X) && is.null(names(answer)))
      names(answer) <- X
  if (!identical(simplify, FALSE) && length(answer))
      simplify2array(answer, higher = (simplify == "array"))
  else answer
}


microbenchmark::microbenchmark(
  mcsapply(seq(10), function(x) { Sys.sleep(0.1); x + 1 }, mc.cores = 1),
  mcsapply(seq(10), function(x) { Sys.sleep(0.1); x + 1 }, mc.cores = 1)
, times = 1)
```

    ## Warning in `levels<-`(`*tmp*`, value = if (nl == nL) as.character(labels)
    ## else paste0(labels, : duplicated levels in factors are deprecated

    ## Unit: seconds
    ##                                                                           expr
    ##  mcsapply(seq(10), function(x) {     Sys.sleep(0.1)     x + 1 }, mc.cores = 1)
    ##       min       lq    mean  median       uq      max neval
    ##  1.000826 1.000826 1.00785 1.00785 1.014873 1.014873     2

### 13. How does apply() arrange the output? Read the documentation and perform some experiments.

``` r
a<-apply(mtcars, 2, mean)
a
```

    ##        mpg        cyl       disp         hp       drat         wt 
    ##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
    ##       qsec         vs         am       gear       carb 
    ##  17.848750   0.437500   0.406250   3.687500   2.812500

``` r
#altough the output might look like a data.frame, it is not
is.data.frame(a)
```

    ## [1] FALSE

``` r
#instead, the output is a vector that is arranged by colums
is.vector(a)
```

    ## [1] TRUE

### 14. There's no equivalent to split() + vapply(). Should there be? When would it be useful? Implement one yourself.

``` r
#It would not be useful. Although one could use this when dealing with lists of lists, where the large list is split and then vapply() is applied, one could simply use 2 vapplys to do the same thing. 
```

### 15. Implement a pure R version of split(). (Hint: use unique() and subsetting.) Can you do it without a for loop?

``` r
library(magrittr)
split2 <- function(x, f, drop = FALSE, ...) {
  f %>% as.factor %>% unique %>%
    lapply(., function(sf) x[f == sf, ]) %>%
    { names(.) <- levels(f); . }
}

split2(mtcars, mtcars$cyl)
```

    ## [[1]]
    ##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4      21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## Valiant        18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
    ## Merc 280       19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
    ## Merc 280C      17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
    ## Ferrari Dino   19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    ## 
    ## [[2]]
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
    ## 
    ## [[3]]
    ##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
    ## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
    ## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
    ## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
    ## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
    ## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
    ## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
    ## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
    ## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
    ## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
    ## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
    ## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
    ## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    ## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8

### 16. What other types of input and output are missing? Brainstorm before you look up some answers in the plyr paper.

lapply and friends don't handle dataframe output (plyr does). it can handle list-to-list and array-to array

### 17. Why isn't is.na() a predicate function? What base R function is closest to being a predicate version of is.na()?

A predication function would return a single logical value (has length 1), while is.na() returns a vectorized output (which can have length higher than 1)

anyNA() is a prediacte function since it returns TRUE or FALSE regardless of how many NAs are present.

### 18. Use Filter() and vapply() to create a function that applies a summary statistic to every numeric column in a data frame.

``` r
summary2 <- function(df, f) {
   vapply(Filter(is.numeric, df), f, numeric(1))
}

summary2(mtcars, mean)
```

    ##        mpg        cyl       disp         hp       drat         wt 
    ##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
    ##       qsec         vs         am       gear       carb 
    ##  17.848750   0.437500   0.406250   3.687500   2.812500

### 19. What's the relationship between which() and Position()? What's the relationship between where() and Filter()?

Position() gives the first instance of TRUE; which() gives all instances.

where() returns a logical vector, while Filter() returns the subset of the data based upon a logical vector.

### 20. Implement Any(), a function that takes a list and a predicate function, and returns TRUE if the predicate function returns TRUE for any of the inputs. Implement All() similarly.

``` r
Any <- function(f, x) {
  any(vapply(x, f, logical(1)))
}

All <- function(f, x) {
  all(vapply(x, f, logical(1)))
}

#another option
All <- function(x) {
   Reduce(`&&`, x, TRUE)
}
Any <- function(x) {
   Reduce(`||`, x, FALSE)
}
```

### 21.Implement the span() function from Haskell: given a list x and a predicate function f, span returns the location of the longest sequential run of elements where the predicate is true. (Hint: you might find rle() helpful.)

``` r
#not sure
```

### 22. Implement `arg_max()`. It should take a function and a vector of inputs, and return the elements of the input where the function returns the highest value. For example, `arg_max(-10:5, function(x) x ^ 2)` should return `-10. arg_max(-5:5, function(x) x ^ 2)` should return `c(-5, 5)`. Also implement the matching `arg_min()` function.

``` r
arg_max <- function(x, FUN) {
   FUN.x <- vapply(x, FUN, numeric(1))
   x[which(FUN.x == max(FUN.x))]
}
```

### 23. Challenge: read about the [fixed point algorithm](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-12.html#%_sec_1.3). Complete the exercises using R.

beyond me right now

### 24. Implement smaller and larger functions that, given two inputs, return either the smaller or the larger value. Implement na.rm = TRUE: what should the identity be? (Hint: smaller(x, smaller(NA, NA, na.rm = TRUE), na.rm = TRUE) must be x, so smaller(NA, NA, na.rm = TRUE) must be bigger than any other value of x.) Use smaller and larger to implement equivalents of min(), max(), pmin(), pmax(), and new functions `row_min()` and `row_max()`.

### 25. Create a table that has and, or, add, multiply, smaller, and larger in the columns and binary operator, reducing variant, vectorised variant, and array variants in the rows.

1.  Fill in the cells with the names of base R functions that perform each of the roles.

2.  Compare the names and arguments of the existing R functions. How consistent are they? How could you improve them?

3.  Complete the matrix by implementing any missing functions.

### 26. How does paste() fit into this structure? What is the scalar binary function that underlies paste()? What are the sep and collapse arguments to paste() equivalent to? Are there any paste variants that don't have existing R implementations?
