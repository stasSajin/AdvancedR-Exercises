### 1. What function allows you to tell if an object is a function? What function allows you to tell if a function is a primitive function?

You can use `is.function()`; for primitive functions, use `is.primitive()`.

### 2. This code makes a list of all functions in the base package.

Use it to answer the following questions:

1.  Which base function has the most arguments?
2.  How many base functions have no arguments? What's special about those functions?
3.  How could you adapt the code to find all primitive functions?

``` r
objs <- mget(ls("package:base"), inherits = TRUE)
funs <- Filter(is.function, objs)

#a. find the function with most arguments
which.max(lapply(funs, function(x) length(formals(x))))
```

    ## scan 
    ##  932

``` r
#the answer is scan; 

#b. The function with no arguments are primitives; There are 225 such functions
length(funs[lapply(funs, function(x) length(formals(x)))==0])
```

    ## [1] 225

``` r
#c. to select only the primitive functions, use:
primitive <- Filter(is.primitive, objs) #should give you 183 functions
```

### 3. What are the three important components of a function?

    * the body(), which is the code inside the function
    * formals(), which is the list of arguments which controls how you can call the function.
    * the environment(), which is a map of the location for the function's variables.

### 4. When does printing a function not show what environment it was created in?

The function environment is not displayed when the function was created in the global environment.

### 5. What does the following code return? Why? What does each of the three c's mean?

``` r
c <- 10 #create a variable that stores the value 10
c(c = c)
```

    ##  c 
    ## 10

The first c() returns a vector with all the elements inside it; the left-hand c is a local variable within the function; the right hand c represents a local variable outside the function. It has 10 stored in it.

### 6. What are the four principles that govern how R looks for values?

-   Name masking: if a name is not defined inside a function, it will look up the name one level up in the environment, up until the global environment
-   functions vs. variables: same principles apply as above, but if you specify function names, R will know to ignore objects that re not functions during its search.
-   a fresh start: eahc invocation of a function is completely independent from a previous invocation of the function.
-   dynamic lookup: allows for variables to be evaluated when needed.

### 7. What does the following function return? Make a prediction before running the code yourself.

Prediction: 202; the evaluation is done based on lexical scoping rules.

``` r
f <- function(x) {
  f <- function(x) {
    f <- function(x) {
      x ^ 2
    }
    f(x) + 1
  }
  f(x) * 2
}
f(10)
```

    ## [1] 202

### 8. Clarify the following list of odd function calls:

``` r
#samples 20 values with replacement from a vector of numbers 1:10 and NA.
x <- sample(replace = TRUE, 20, x = c(1:10, NA))
#gives you 20 numbers from a uniform distribution between 0-1; The order of the arguments is a bit weird. 
y <- runif(min = 0, max = 1, 20)
#find the correlation between x and y; the x and y variable are called from outside the function; u represent the method for computing covariances, which in this case represents the pairwise.complete.obs. m represents the method for the correlation coefficient to be computed, which in this case represents kendall. 
cor(m = "k", y = y, u = "p", x = x);
```

    ## [1] 0.0814427

### 9. What does this function return? Why? Which principle does it illustrate?

``` r
#the function should return 3; it illustraes dynamyc scoping???
f1 <- function(x = {y <- 1; 2}, y = 0) {
  x + y
}
f1()
```

    ## [1] 3

### 10. What does this function return? Why? Which principle does it illustrate?

``` r
#evaluates to 100; shows lazy evaluation; does not evaluate the function until it has enough information to allow it to assign a value to x.  The unevaluated x is defined as a promise.
f2 <- function(x = z) {
  z <- 100
  x
}
f2()
```

    ## [1] 100

### 11. Create a list of all the replacement functions found in the base package. Which ones are primitive functions?

``` r
#using the same functions as in previous chapter
objs <- mget(ls("package:base"), inherits = TRUE)
funs <- Filter(is.primitive, objs)

#use regular expressions to find <-. 
names(funs[grepl("<-", names(funs))])
```

    ##  [1] "$<-"            "@<-"            "[[<-"           "[<-"           
    ##  [5] "<-"             "<<-"            "attr<-"         "attributes<-"  
    ##  [9] "class<-"        "dim<-"          "dimnames<-"     "environment<-" 
    ## [13] "length<-"       "levels<-"       "names<-"        "oldClass<-"    
    ## [17] "storage.mode<-"

### 12. What are valid names for user-created infix functions?

Whatever you put between % symbols

### 13. Create an infix xor() operator.

``` r
`%xor%` <- function (a, b) !(a & b) & (a | b)

`%xor%` (T,T)
```

    ## [1] FALSE

``` r
`%xor%` (T,F)
```

    ## [1] TRUE

``` r
`%xor%` (F,F)
```

    ## [1] FALSE

### 14. Create infix versions of the set functions intersect(), union(), and setdiff().

``` r
#for the intersect
`%intersect2%` <- function(a,b) {
    a[a%in%b]
}
c(1,2,3,4) %intersect2% c(1,2,5,6)
```

    ## [1] 1 2

``` r
#for the union
`%union2%` <- function(a,b) {
    unique(c(a,b))
}
c(1,2,3,4) %union2% c(1,2,5,6)
```

    ## [1] 1 2 3 4 5 6

``` r
#for the setdiff
`%setdiff2%` <- function(a,b) {
    a[!(a%in%b)]
}
c(1,2,3,4) %setdiff2% c(1,2,5,6)
```

    ## [1] 3 4

### 15. Create a replacement function that modifies a random location in a vector.

``` r
`random2<-` <- function(x,value) {
    loc<-sample(length(v),1)
    x[loc] <- value
    x
}
```

### 16. How does the chdir parameter of source() compare to in\_dir()? Why might you prefer one approach to the other?

The chdir indicates that the R working directory is temporarily changes to the directory containing the file you want to run. The in\_dir saves the previous working directory whenever you set up a new working directory.

### 17. What function undoes the action of library()? How do you save and restore the values of options() and par()?

detach() undes the library() action. You can save the values by typing `options(option=value)`, where options represents the value you want to set and save. Same thing for par: `par(option=value)` The answer to restoring default options can be found here: <http://stackoverflow.com/questions/15946953/how-to-set-r-to-default-options>

### 18. Write a function that opens a graphics device, runs the supplied code, and closes the graphics device (always, regardless of whether or not the plotting code worked).

``` r
plot.png <- function(path, code) {
  png(path)
  on.exit(dev.off())
  force(code)
}
```

### 19. We can use on.exit() to implement a simple version of capture.output().

``` r
capture.output2 <- function(code) {
  temp <- tempfile()
  on.exit(file.remove(temp), add = TRUE)

  sink(temp)
  on.exit(sink(), add = TRUE)

  force(code)
  readLines(temp)
}
capture.output2(cat("a", "b", "c", sep = "\n"))
```

    ## Warning in file.remove(temp): cannot remove file 'C:\Users\Stas\AppData
    ## \Local\Temp\RtmpWiAhp9\file1b606e5e4b8c', reason 'Permission denied'

    ## [1] "a" "b" "c"

``` r
#> [1] "a" "b" "c"
```

Compare capture.output() to capture.output2(). How do the functions differ? What features have I removed to make the key ideas easier to see? How have I rewritten the key ideas to be easier to understand?

I get a warning message: `Warning message:  In file.remove(temp) :   cannot remove file 'C:\Users\Stas\AppData\Local\Temp\RtmpcvlDEb\filee9c39157594', reason 'Permission denied'`

Not sure how to dolve this issue.
