### 1. Given a function, like "mean", match.fun() lets you find a function. Given a function, can you find its name? Why doesn't that make sense in R?

The short answer is no. In R functions are objects in their own right and are not automatically bound to a name. COnsider the case of anonymous functions that have no name.

### 2. Use lapply() and an anonymous function to find the coefficient of variation (the standard deviation divided by the mean) for all columns in the mtcars dataset.

``` r
lapply(mtcars, function(x) sd(x)/mean(x))
```

    ## $mpg
    ## [1] 0.2999881
    ## 
    ## $cyl
    ## [1] 0.2886338
    ## 
    ## $disp
    ## [1] 0.5371779
    ## 
    ## $hp
    ## [1] 0.4674077
    ## 
    ## $drat
    ## [1] 0.1486638
    ## 
    ## $wt
    ## [1] 0.3041285
    ## 
    ## $qsec
    ## [1] 0.1001159
    ## 
    ## $vs
    ## [1] 1.152037
    ## 
    ## $am
    ## [1] 1.228285
    ## 
    ## $gear
    ## [1] 0.2000825
    ## 
    ## $carb
    ## [1] 0.5742933

### 3. Use integrate() and an anonymous function to find the area under the curve for the following functions. Use Wolfram Alpha to check your answers.

1.  y = x ^ 2 - x, x in \[0, 10\]
2.  y = sin(x) + cos(x), x in \[-pi, pi\]
3.  y = exp(x) / x, x in \[10, 20\]

``` r
#for the first case
integrate(function(x) x ^ 2 - x, 0, 10)
```

    ## 283.3333 with absolute error < 3.1e-12

``` r
#for the second case
integrate(function(x) sin(x) + cos(x), -pi, pi)
```

    ## 2.615901e-16 with absolute error < 6.3e-14

``` r
#for the third case
integrate(function(x) exp(x) / x, 10, 20)
```

    ## 25613160 with absolute error < 2.8e-07

### 4. A good rule of thumb is that an anonymous function should fit on one line and shouldn't need to use {}. Review your code. Where could you have used an anonymous function instead of a named function? Where should you have used a named function instead of an anonymous function?

It sould be beneficial to create a named function for standard error. Other than that, we don't really have a need to use anonymous functions for integration because we could just replace the x values with the lower and upper bound we are integrating on.

### 5. Why are functions created by other functions called closures?

Closures enclose the environment of a parent function and access all of its variables.

### 6. What does the following statistical function do? What would be a better name for it? (The existing name is a bit of a hint.)

The formula `(x^lambda-1)/lambda` is for Box-Cox transformation. This function will return a function that that either returns the log or computes the box-cox transform.

``` r
bc <- function(lambda) {
  if (lambda == 0) {
    function(x) log(x)
  } else {
    function(x) (x ^ lambda - 1) / lambda
  }
}
```

### 7. What does approxfun() do? What does it return?

According to the documentation: "Returns a list of points which linearly interpolate given data points, or a function performing the linear (or constant) interpolation.""

### 8. What does ecdf() do? What does it return?

Documentation: "Computes an empirical cumulative distribution function, with several methods for plotting, printing and computing with such an "ecdf" object."

### 9. Create a function that creates functions that compute the ith central moment of a numeric vector. You can test it by running the following code:

``` r
#here is the function; You can find more about the formula here: http://www.r-tutor.com/elementary-statistics/numerical-measures/moment
moment<-function(momentOrder) {
    function (x) (sum((x-mean(x))^momentOrder)/length(x))
}

m1 <- moment(1)
m2 <- moment(2)

unenclose(m1)
```

    ## function (x) 
    ## (sum((x - mean(x))^1)/length(x))

``` r
unenclose(m2)
```

    ## function (x) 
    ## (sum((x - mean(x))^2)/length(x))

``` r
x <- runif(100)
stopifnot(all.equal(m1(x), 0))
stopifnot(all.equal(m2(x), var(x) * 99 / 100))
#all tests have passed
```

### 10. Create a function pick() that takes an index, i, as an argument and returns a function with an argument x that subsets x with i.

``` r
#here is the solution:
pick <- function(i) { function(x) x[[i]] }

lapply(mtcars, pick(5))
```

    ## $mpg
    ## [1] 18.7
    ## 
    ## $cyl
    ## [1] 8
    ## 
    ## $disp
    ## [1] 360
    ## 
    ## $hp
    ## [1] 175
    ## 
    ## $drat
    ## [1] 3.15
    ## 
    ## $wt
    ## [1] 3.44
    ## 
    ## $qsec
    ## [1] 17.02
    ## 
    ## $vs
    ## [1] 0
    ## 
    ## $am
    ## [1] 0
    ## 
    ## $gear
    ## [1] 3
    ## 
    ## $carb
    ## [1] 2

``` r
# should do the same as this
lapply(mtcars, function(x) x[[5]])
```

    ## $mpg
    ## [1] 18.7
    ## 
    ## $cyl
    ## [1] 8
    ## 
    ## $disp
    ## [1] 360
    ## 
    ## $hp
    ## [1] 175
    ## 
    ## $drat
    ## [1] 3.15
    ## 
    ## $wt
    ## [1] 3.44
    ## 
    ## $qsec
    ## [1] 17.02
    ## 
    ## $vs
    ## [1] 0
    ## 
    ## $am
    ## [1] 0
    ## 
    ## $gear
    ## [1] 3
    ## 
    ## $carb
    ## [1] 2

### 11. Implement a summary function that works like base::summary(), but uses a list of functions. Modify the function so it returns a closure, making it possible to use it as a function factory.

``` r
#most of this code has already been showcased in the book.

make_summary <- function(funs) {
  function(x) {
    lapply(funs, function(sf) sf(x))
  }
}

summaryFuns <- c(mean, median, sd, mad, IQR)
summary2 <- make_summary(summaryFuns)
summary2(1:10)
```

    ## [[1]]
    ## [1] 5.5
    ## 
    ## [[2]]
    ## [1] 5.5
    ## 
    ## [[3]]
    ## [1] 3.02765
    ## 
    ## [[4]]
    ## [1] 3.7065
    ## 
    ## [[5]]
    ## [1] 4.5

``` r
#it does not show the same, so here is a changed version
summaryFuns <- list('min' = min, 'median' = median, 'mean' = mean, 'max' = max)
summary2 <- make_summary(summaryFuns)
summary2(1:10)
```

    ## $min
    ## [1] 1
    ## 
    ## $median
    ## [1] 5.5
    ## 
    ## $mean
    ## [1] 5.5
    ## 
    ## $max
    ## [1] 10

### 12. Which of the following commands is equivalent to with(x, f(z))?

1.  x\(f(x\)z).
2.  f(x$z).
3.  x$f(z).
4.  f(z).
5.  It depends.

Answer: x\(f(x\)z);

### 13. Instead of creating individual functions (e.g., midpoint(), trapezoid(), simpson(), etc.), we could store them in a list. If we did that, how would that change the code? Can you create the list of functions from a list of coefficients for the Newton-Cotes formulae?

Putting all the functions in a list would make the function more mutable. Here is an example of how we can create the list of functions from a list of coefficients for the Newton-Cotes formulae

``` r
#run newton-cotes function

newton_cotes <- function(coef, open = FALSE) {
  n <- length(coef) + open
  function(f, a, b) {
    pos <- function(i) a + i * (b - a) / n
    points <- pos(seq.int(0, length(coef) - 1))

    (b - a) / sum(coef) * sum(f(points) * coef)
  }
}

#run the composite area function
composite <- function(f, a, b, n = 10, rule) {
  points <- seq(a, b, length = n + 1)

  area <- 0
  for (i in seq_len(n)) {
    area <- area + rule(f, points[i], points[i + 1])
  }

  area
}

#list of rules and coef for each;
rules <- list(
    midpoint=list(coef = c(0, 1), open=TRUE),
    trapezoid=list(coef=c(1,1)), 
    simpson=list(coef=c(1, 4, 1)),
    boole=list(coef=c(7, 32, 12, 32, 7)),
    milne=list(coef=c(2, -1, 2), open = TRUE))

#create functions for each rule by inputing the arguments in the newton_cotes function. The ouput should be a list of functions for each rule.
functionRules<-lapply(rules, function(args) do.call('newton_cotes',args))

#save results
results<-sapply(functionRules, function(r) composite(sin, 0, pi, n = 10, rule = r))
results
```

    ##  midpoint trapezoid   simpson     boole     milne 
    ##  2.005496  1.995886  2.001834  2.001979  1.993829

### 14. The trade-off between integration rules is that more complex rules are slower to compute, but need fewer pieces. For sin() in the range \[0, ??\], determine the number of pieces needed so that each rule will be equally accurate. Illustrate your results with a graph. How do they change for different functions? sin(1 / x^2) is particularly challenging.

I'll finish this at a later time.
