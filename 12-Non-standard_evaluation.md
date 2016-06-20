Load the following libraries

``` r
pacman::p_load(plyr,pryr,microbenchmark)
```

Set 1 exercises
===============

1. One important feature of deparse() to be aware of when programming is that it can return multiple strings if the input is too long. For example, the following call produces a vector of length two:
=======================================================================================================================================================================================================

``` r
g <- function(x) deparse(substitute(x))
g(a + b + c + d + e + f + g + h + i + j + k + l + m + 
      n + o + p + q + r + s + t + u + v + w + x + y + z)
```

    ## [1] "a + b + c + d + e + f + g + h + i + j + k + l + m + n + o + p + "
    ## [2] "    q + r + s + t + u + v + w + x + y + z"

``` r
#?deparse
```

**Why does this happen? Carefully read the documentation for ?deparse. Can you write a wrapper around deparse() so that it always returns a single string?**

The deparse function has an argument called width.cutoff, which specifies the number of bytes in a line, which defaults to 60 and cannot exceed 500.

``` r
g <- function(x) paste(deparse(substitute(x)), collapse= '')

g(a + b + c + d + e + f + g + h + i + j + k + l + m + 
      n + o + p + q + r + s + t + u + v + w + x + y + z)
```

    ## [1] "a + b + c + d + e + f + g + h + i + j + k + l + m + n + o + p +     q + r + s + t + u + v + w + x + y + z"

2. Why does as.Date.default() use substitute() and deparse()? Why does pairwise.t.test() use them? Read the source code.
========================================================================================================================

``` r
as.Date.default
```

    ## function (x, ...) 
    ## {
    ##     if (inherits(x, "Date")) 
    ##         return(x)
    ##     if (is.logical(x) && all(is.na(x))) 
    ##         return(structure(as.numeric(x), class = "Date"))
    ##     stop(gettextf("do not know how to convert '%s' to class %s", 
    ##         deparse(substitute(x)), dQuote("Date")), domain = NA)
    ## }
    ## <bytecode: 0x0000000005f38698>
    ## <environment: namespace:base>

The deparse(substitute) allows one to printan informative error message with the input that was given to the function. For example

``` r
as.Date.default(5)

#will show error
#Error in as.Date.default(5) : 
#  do not know how to convert '5' to class "Date"
```

``` r
pairwise.t.test
```

    ## function (x, g, p.adjust.method = p.adjust.methods, pool.sd = !paired, 
    ##     paired = FALSE, alternative = c("two.sided", "less", "greater"), 
    ##     ...) 
    ## {
    ##     if (paired & pool.sd) 
    ##         stop("pooling of SD is incompatible with paired tests")
    ##     DNAME <- paste(deparse(substitute(x)), "and", deparse(substitute(g)))
    ##     g <- factor(g)
    ##     p.adjust.method <- match.arg(p.adjust.method)
    ##     alternative <- match.arg(alternative)
    ##     if (pool.sd) {
    ##         METHOD <- "t tests with pooled SD"
    ##         xbar <- tapply(x, g, mean, na.rm = TRUE)
    ##         s <- tapply(x, g, sd, na.rm = TRUE)
    ##         n <- tapply(!is.na(x), g, sum)
    ##         degf <- n - 1
    ##         total.degf <- sum(degf)
    ##         pooled.sd <- sqrt(sum(s^2 * degf)/total.degf)
    ##         compare.levels <- function(i, j) {
    ##             dif <- xbar[i] - xbar[j]
    ##             se.dif <- pooled.sd * sqrt(1/n[i] + 1/n[j])
    ##             t.val <- dif/se.dif
    ##             if (alternative == "two.sided") 
    ##                 2 * pt(-abs(t.val), total.degf)
    ##             else pt(t.val, total.degf, lower.tail = (alternative == 
    ##                 "less"))
    ##         }
    ##     }
    ##     else {
    ##         METHOD <- if (paired) 
    ##             "paired t tests"
    ##         else "t tests with non-pooled SD"
    ##         compare.levels <- function(i, j) {
    ##             xi <- x[as.integer(g) == i]
    ##             xj <- x[as.integer(g) == j]
    ##             t.test(xi, xj, paired = paired, alternative = alternative, 
    ##                 ...)$p.value
    ##         }
    ##     }
    ##     PVAL <- pairwise.table(compare.levels, levels(g), p.adjust.method)
    ##     ans <- list(method = METHOD, data.name = DNAME, p.value = PVAL, 
    ##         p.adjust.method = p.adjust.method)
    ##     class(ans) <- "pairwise.htest"
    ##     ans
    ## }
    ## <bytecode: 0x0000000006184800>
    ## <environment: namespace:stats>

I this case, the deparse and subsititute are used to create a string DNAME with the parameters x and g

3. pairwise.t.test() assumes that deparse() always returns a length one character vector. Can you construct an input that violates this expectation? What happens?
==================================================================================================================================================================

``` r
testPair<-pairwise.t.test(rep(1,20),rep(2,20))
str(testPair)
```

    ## List of 4
    ##  $ method         : chr "t tests with pooled SD"
    ##  $ data.name      : chr "rep(1, 20) and rep(2, 20)"
    ##  $ p.value        : list()
    ##   ..- attr(*, "dim")= int [1:2] 0 0
    ##   ..- attr(*, "dimnames")=List of 2
    ##   .. ..$ : NULL
    ##   .. ..$ : NULL
    ##  $ p.adjust.method: chr "holm"
    ##  - attr(*, "class")= chr "pairwise.htest"

``` r
length(testPair$data.name)
```

    ## [1] 1

Ok, so the above didn't work, the dataname value shows only one length vector. Nontheless, from exercise 1 we know that deparse has a limit of 60bytes per line, so all we have to do is to make the x and y arguments have more characters in them

``` r
testPair<-pairwise.t.test(c(1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1),
        c(1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1))
str(testPair)
```

    ## List of 4
    ##  $ method         : chr "t tests with pooled SD"
    ##  $ data.name      : chr [1:2] "c(1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,  and c(1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1"| __truncated__ "    1, 1, 1) and     1, 1, 1)"
    ##  $ p.value        : list()
    ##   ..- attr(*, "dim")= int [1:2] 0 0
    ##   ..- attr(*, "dimnames")=List of 2
    ##   .. ..$ : NULL
    ##   .. ..$ : NULL
    ##  $ p.adjust.method: chr "holm"
    ##  - attr(*, "class")= chr "pairwise.htest"

``` r
length(testPair$data.name)
```

    ## [1] 2

It worked this time.

4. f(), defined above, just calls substitute(). Why can't we use it to define g()? In other words, what will the following code return? First make a prediction. Then run the code and think about the results.
===============================================================================================================================================================================================================

``` r
f <- function(x) substitute(x)
g <- function(x) deparse(f(x))

#should return x, since x is passed as input; subsitutte simply interprets the input as literal. it captures the argument's expression rather than its result
g(1:10)
```

    ## [1] "x"

``` r
#same
g(x)
```

    ## [1] "x"

``` r
#same
g(x + y ^ 2 / z + exp(a * sin(b)))
```

    ## [1] "x"

Set 2 Exercises
===============

1. Predict the results of the following lines of code:
======================================================

``` r
#should be 4
eval(quote(eval(quote(eval(quote(2 + 2))))))
```

    ## [1] 4

``` r
#4
eval(eval(quote(eval(quote(eval(quote(2 + 2)))))))
```

    ## [1] 4

``` r
#eval(quote(eval(quote(eval(quote(2 + 2)))))) ; quote takes the literal
quote(eval(quote(eval(quote(eval(quote(2 + 2)))))))
```

    ## eval(quote(eval(quote(eval(quote(2 + 2))))))

2. subset2() has a bug if you use it with a single column data frame. What should the following code return? How can you modify subset2() so it returns the correct type of object?
===================================================================================================================================================================================

``` r
subset2 <- function(x, condition) {
  condition_call <- substitute(condition)
  r <- eval(condition_call, x)
  x[r, , drop=FALSE]
}

sample_df2 <- data.frame(x = 1:10)
subset2(sample_df2, x > 8)
```

    ##     x
    ## 9   9
    ## 10 10

``` r
#> [1]  9 10
```

3. The real subset function (subset.data.frame()) removes missing values in the condition. Modify subset2() to do the same: drop the offending rows.
====================================================================================================================================================

Just add na.omit on the data.frame that is returned inside the function subset

``` r
subset3 <- function(x, condition) {
  condition_call <- substitute(condition)
  r <- eval(condition_call, x)
  na.omit(x[r, , drop=FALSE])
}

sample_df2 <- data.frame(x = c(1:10, NA))
subset3(sample_df2, x > 8)
```

    ##     x
    ## 9   9
    ## 10 10

4. What happens if you use quote() instead of substitute() inside of subset2()?
===============================================================================

``` r
subset3 <- function(x, condition) {
  condition_call <- quote(condition)
  r <- eval(condition_call, x)
  na.omit(x[r, , drop=FALSE])
}

subset3(sample_df2, x > 8)
```

Weird, I thought that the function will evaluate to the same thing as above. Instead, it shows a data.farme of length 0.

5. The second argument in subset() allows you to select variables. It treats variable names as if they were positions. This allows you to do things like subset(mtcars, , -cyl) to drop the cylinder variable, or subset(mtcars, , disp:drat) to select all the variables between disp and drat. How does this work? I've made this easier to understand by extracting it out into its own function.
====================================================================================================================================================================================================================================================================================================================================================================================================

``` r
select <- function(df, vars) {
  vars <- substitute(vars)
  var_pos <- setNames(as.list(seq_along(df)), names(df))
  pos <- eval(vars, var_pos)
  df[, pos, drop = FALSE]
}
#select(mtcars, -cyl)
```

var\_pos creates a list with the variable name and the column index in the df for each variable. The eval find the index values for the specified variables in the vars argument. The indices are stored in pos and then passed on to the df\[\].

6. What does evalq() do? Use it to reduce the amount of typing for the examples above that use both eval() and quote().
=======================================================================================================================

evalq() is the same as eval(quote())

``` r
evalq(evalq(evalq(2 + 2)))
```

    ## [1] 4

``` r
#4
eval(evalq(evalq(evalq(2 + 2))))
```

    ## [1] 4

``` r
#eval(quote(eval(quote(eval(quote(2 + 2)))))) ; quote takes the literal
quote(evalq(evalq(evalq(2 + 2))))
```

    ## evalq(evalq(evalq(2 + 2)))

Set 3 Exercises
===============

1. plyr::arrange() works similarly to subset(), but instead of selecting rows, it reorders them. How does it work? What does substitute(order(...)) do? Create a function that does only that and experiment with it.
=====================================================================================================================================================================================================================

``` r
arrange
```

    ## function (df, ...) 
    ## {
    ##     stopifnot(is.data.frame(df))
    ##     ord <- eval(substitute(order(...)), df, parent.frame())
    ##     if (length(ord) != nrow(df)) {
    ##         stop("Length of ordering vectors don't match data frame size", 
    ##             call. = FALSE)
    ##     }
    ##     unrowname(df[ord, , drop = FALSE])
    ## }
    ## <environment: namespace:plyr>

``` r
ord <- function(df, ...) {
  eval(substitute(order(...)), df, parent.frame())
}

ord(mtcars, cyl, disp)
```

    ##  [1] 20 19 18 26 28  3 21 27 32  9  8 30  1  2 10 11  6  4 12 13 14 31 23
    ## [24] 22 24 29  5  7 25 17 16 15

``` r
#note that the ouput will be a randking for each row in the dataset based on the order of cyl and disp; for examplem rank 1 corresponds to the 15 index (ie, row) in the dataset. Note that 15th row has 8 cyl and highest disp, hence rank 1.
```

2. What does transform() do? Read the documentation. How does it work? Read the source code for transform.data.frame(). What does substitute(list(...)) do?
===========================================================================================================================================================

``` r
#?transform
```

it allows us to make modifications to the dataframe. For instance, you can see in the documentation examples that it allows us to change the values of the columns and create new colums.

3, plyr::mutate() is similar to transform() but it applies the transformations sequentially so that transformation can refer to columns that were just created:
===============================================================================================================================================================

``` r
df <- data.frame(x = 1:5)
#transform(df, x2 = x * x, x3 = x2 * x) this will throw an error
#Error in eval(expr, envir, enclos) : object 'x2' not found

plyr::mutate(df, x2 = x * x, x3 = x2 * x)
```

    ##   x x2  x3
    ## 1 1  1   1
    ## 2 2  4   8
    ## 3 3  9  27
    ## 4 4 16  64
    ## 5 5 25 125

How does mutate work? What's the key difference between mutate() and transform()?

Transform does not allow you to reffer back to previously specified arguments. For instance, notice that it throws and error for x2 not found. That's because it has not stored the variable x2 in the dataframe when it has evaluated it. On the other hand, the mutate function proceeds more sequantially. It evaluates the new transformaiton, stores it in the df, and then proceeds to the next transformation. Mutate thus allows one to reffer back to arguments that have been already evaluated.

4. What does with() do? How does it work? Read the source code for with.default(). What does within() do? How does it work? Read the source code for within.data.frame(). Why is the code so much more complex than with()?
===========================================================================================================================================================================================================================

from help: `with` evaluates an R expression within an environment created from data

`with.default` is just a wrapper for the standard `eval(substitute())` **I'll answer other parts of the question later.**

Set 4 Exercises
===============

1. The following R functions all use NSE. For each, describe how it uses NSE, and read the documentation to determine its escape hatch.
=======================================================================================================================================

rm() library() and require() substitute() data() data.frame()

2. Base functions match.fun(), page(), and ls() all try to automatically determine whether you want standard or non-standard evaluation. Each uses a different approach. Figure out the essence of each approach then compare and contrast.
===========================================================================================================================================================================================================================================

match.fun tests to see if the FUN argument is a function or character, then decides whether to use standard or non-standard evaluation.

page is similar to match.fun

3. Add an escape hatch to plyr::mutate() by splitting it into two functions. One function should capture the unevaluated inputs. The other should take a data frame and list of expressions and perform the computation.
========================================================================================================================================================================================================================

4. What's the escape hatch for ggplot2::aes()? What about plyr::()? What do they have in common? What are the advantages and disadvantages of their differences?
================================================================================================================================================================

5. The version of subset2\_q() I presented is a simplification of real code. Why is the following version better?
=================================================================================================================

``` r
subset2_q <- function(x, cond, env = parent.frame()) {
  r <- eval(cond, x, env)
  x[r, ]
}
```

My guess is that it allows you to specify the environment.

Set 5 exercises
===============

1. Use subs() to convert the LHS to the RHS for each of the following pairs:
============================================================================

-   a + b + c -&gt; a \* b \* c
-   f(g(a, b), c) -&gt; (a + b) \* c
-   f(a &lt; b, c, d) -&gt; if (a &lt; b) c else d

``` r
subs(a + b + c, list("+" = quote(`*`)))
```

    ## a * b * c

``` r
subs(f(g(a, b), c), list(g = quote(`+`), f = quote(`*`)))
```

    ## (a + b) * c

``` r
subs(f(a < b, c, d), list(f = quote(`if`)))
```

    ## if (a < b) c else d

2. For each of the following pairs of expressions, describe why you can't use subs() to convert one to the other.
=================================================================================================================

-   a + b + c -&gt; a + b \* c

You can't specify which indices should be converted to what. For instance, you can only use subs to convert all '+' to '\*'

-   f(a, b) -&gt; f(a, b, c)

You can't add new arguments with subs

-   f(a, b, c) -&gt; f(a, b)

You can't remove arguments with subs

3. How does pryr::named\_dots() work? Read the source.
======================================================

``` r
named_dots
```

    ## function (...) 
    ## {
    ##     args <- dots(...)
    ##     nms <- names(args) %||% rep("", length(args))
    ##     missing <- nms == ""
    ##     if (all(!missing)) 
    ##         return(args)
    ##     deparse2 <- function(x) paste(deparse(x, 500L), collapse = "")
    ##     defaults <- vapply(args[missing], deparse2, character(1), 
    ##         USE.NAMES = FALSE)
    ##     names(args)[missing] <- defaults
    ##     args
    ## }
    ## <environment: namespace:pryr>

It first passes all the arguments through dots. Let's see what dots does:

``` r
dots
```

    ## function (...) 
    ## {
    ##     eval(substitute(alist(...)))
    ## }
    ## <environment: namespace:pryr>

Ok, so the dots will just provide a list of all the arguments and their values.

``` r
#Example:
dots(x=5,y=6)
```

    ## $x
    ## [1] 5
    ## 
    ## $y
    ## [1] 6

Then from the list we extract all the names for each argument. If all the args are "", it simply returns the args. Otherwise, it names the args with their values, and returns the re-named list of args.

Set 6 Exercises
===============

1. What does the following function do? What's the escape hatch? Do you think that this is an appropriate use of NSE?
=====================================================================================================================

``` r
nl <- function(...) {
  dots <- named_dots(...)
  lapply(dots, eval, parent.frame())
}
```

2. Instead of relying on promises, you can use formulas created with ~ to explicitly capture an expression and its environment. What are the advantages and disadvantages of making quoting explicit? How does it impact referential transparency?
==================================================================================================================================================================================================================================================

3. Read the standard non-standard evaluation rules found at <http://developer.r-project.org/nonstandard-eval.pdf>.
==================================================================================================================
