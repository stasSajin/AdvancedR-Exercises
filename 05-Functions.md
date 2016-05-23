### 1. What function allows you to tell if an object is a function? What function allows you to tell if a function is a primitive function?

You can use `is.function()`; for primitive functions, use `is.primitive()`.

### 2. This code makes a list of all functions in the base package.

`objs <- mget(ls("package:base"), inherits = TRUE)` `funs <- Filter(is.function, objs)`

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

> c &lt;- 10 c(c = c)
