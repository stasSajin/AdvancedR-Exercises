### 1. Read the source code for t() and t.test() and confirm that t.test() is an S3 generic and not an S3 method. What happens if you create an object with class test and call t() with it?

``` r
#let's confirm that the two functions are S3 generic
ftype(t)
```

    ## [1] "s3"      "generic"

``` r
ftype(t.test)
```

    ## [1] "s3"      "generic"

``` r
#create a vector t object
myTestObject2 <- structure(1:5, class = "test")
t(myTestObject2)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  myTestObject2
    ## t = 4.2426, df = 4, p-value = 0.01324
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  1.036757 4.963243
    ## sample estimates:
    ## mean of x 
    ##         3

``` r
#it executes a t.test on the observations within the vector.
```

### 2. What classes have a method for the Math group generic in base R? Read the source code. How do the methods work?

``` r
#?methods
#these are the classes???
methods("Math")
```

    ## [1] Math,nonStructure-method Math,structure-method   
    ## [3] Math.data.frame          Math.Date               
    ## [5] Math.difftime            Math.factor             
    ## [7] Math.POSIXt             
    ## see '?methods' for accessing help and source code

``` r
#the Math isn't really a function but represents a group of generic functions (e.g., +,-, sin, cos) that are supplied to other functions. Allows for the creation of more complex objects. 
```

### 3. R has two classes for representing date time data, POSIXct and POSIXlt, which both inherit from POSIXt. Which generics have different behaviours for the two classes? Which generics share the same behaviour?

``` r
#let's find the different generics for each class.
#these POSIXct functions are not present in POSIXlt
library(qdap)
```

    ## Loading required package: qdapDictionaries

    ## Loading required package: qdapRegex

    ## Loading required package: qdapTools

    ## Loading required package: RColorBrewer

    ## 
    ## Attaching package: 'qdap'

    ## The following object is masked from 'package:base':
    ## 
    ##     Filter

``` r
XctFunctions<-beg2char(methods(class="POSIXct"),".",1)
XltFunctions<-beg2char(methods(class="POSIXlt"),".",1)

#Functions found in POSIXct and not in POSIXlt
setdiff(XctFunctions, XltFunctions)
```

    ## [1] "[["                                   
    ## [2] "coerce,XMLAbstractNode,POSIXct-method"
    ## [3] "split"

``` r
#Functions found in POSIXlt and not in POSIXct
setdiff(XltFunctions, XctFunctions)
```

    ## [1] "anyNA"      "duplicated" "is"         "length"     "names"     
    ## [6] "names<-"    "sort"       "unique"

``` r
#functions found in both
intersect(XctFunctions, XltFunctions)
```

    ##  [1] "["                           "[<-"                        
    ##  [3] "as"                          "c"                          
    ##  [5] "coerce,oldClass,S3-method"   "format"                     
    ##  [7] "initialize,oldClass-method"  "mean"                       
    ##  [9] "print"                       "rep"                        
    ## [11] "show,oldClass-method"        "slotsFromS3,oldClass-method"
    ## [13] "summary"                     "Summary"                    
    ## [15] "weighted"                    "xtfrm"

### 4. Which base generic has the greatest number of defined methods?

``` r
#get list of functions in the base environment
objs <- mget(ls("package:base"), inherits = TRUE)
baseFunctions <- Filter(is.function, objs)

is.generic <- function(x) {
    is.element("generic",ftype(x))
}

generics<-Filter(is.generic,baseFunctions)

numMethods <- sapply(names(generics),function(x) length(methods(x)))
which.max(numMethods)
```

    ## integer(0)

### 5. UseMethod() calls methods in a special way. Predict what the following code will return, then run it and read the help for UseMethod() to figure out what's going on. Write down the rules in the simplest form possible.

``` r
#this should come up to 2, because the y is executed inside the function g and not in the global environment
y <- 1
g <- function(x) {
  y <- 2
  UseMethod("g")
}
g.numeric <- function(x) y
g(10)
```

    ## [1] 2

``` r
#this should show "char a" because the base h invokes the character method. If you execute something like `(h1)`, it will pass it to the numeric method and the ouput should be `num 1`.
h <- function(x) {
  x <- 10
  UseMethod("h")
}
h.character <- function(x) paste("char", x)
h.numeric <- function(x) paste("num", x)

h("a")
```

    ## [1] "char a"

### 6. Internal generics don't dispatch on the implicit class of base types. Carefully read ?"internal generic" to determine why the length of f and g is different in the example below. What function helps distinguish between the behaviour of f and g?

``` r
f <- function() 1
g <- function() 2
class(g) <- "function"

class(f)
```

    ## [1] "function"

``` r
class(g)
```

    ## [1] "function"

``` r
length.function <- function(x) "function"
length(f)
```

    ## [1] 1

``` r
length(g)
```

    ## [1] "function"

``` r
#not sure what the answer is to this one.
```

##### I'll have to work on some of these problems a bit later as they are a bit beyond me.

### 7. Which S4 generic has the most methods defined for it? Which S4 class has the most methods associated with it?

### 8. What happens if you define a new S4 class that doesn't "contain" an existing class? (Hint: read about virtual classes in ?Classes.)

### 9. What happens if you pass an S4 object to an S3 generic? What happens if you pass an S3 object to an S4 generic? (Hint: read ?setOldClass for the second case.)

### 10. Use a field function to prevent the account balance from being directly manipulated. (Hint: create a "hidden" .balance field, and read the help for the fields argument in setRefClass().)

### 11. I claimed that there aren't any RC classes in base R, but that was a bit of a simplification. Use getClasses() and find which classes extend() from envRefClass. What are the classes used for? (Hint: recall how to look up the documentation for a class.)

``` r
allClasses <- getClasses()
ext <- names(which(sapply(allClasses, function(x) extends(x,'envRefClass'))))
ext
#[1] "envRefClass"      "localRefClass"    "refGeneratorSlot"
#these classes implement basic reference style semantics for R objects.
```
