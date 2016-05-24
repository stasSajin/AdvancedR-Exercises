### 1. Compare the following two implementations of message2error(). What is the main advantage of withCallingHandlers() in this scenario? (Hint: look carefully at the traceback.)

``` r
message2error <- function(code) {
  withCallingHandlers(code, message = function(e) stop(e))
}

message2error(someCode)
# Error in withCallingHandlers(code, message = function(e) stop(e)) : 
#   object 'someCode' not found

# 2: withCallingHandlers(code, message = function(e) stop(e)) at #2
# 1: message2error(someCode)


message2error <- function(code) {
  tryCatch(code, message = function(e) stop(e))
}

message2error(someCode)

# Error in doTryCatch(return(expr), name, parentenv, handler) : 
#   object 'someCode' not found 

## this is the traceback output.
# 5 doTryCatch(return(expr), name, parentenv, handler) 
# 4 tryCatchOne(expr, names, parentenv, handlers[[1L]]) 
# 3 tryCatchList(expr, classes, parentenv, handlers) 
# 2 tryCatch(code, message = function(e) stop(e)) 
# 1 message2error(someCode) 

#the traceback for the second option seems to have more statements. I'm not sure why. 
```

### 2. The goal of the col\_means() function defined below is to compute the means of all numeric columns in a data frame.

``` r
col_means <- function(df) {
  numeric <- sapply(df, is.numeric)
  numeric_cols <- df[, numeric]

  data.frame(lapply(numeric_cols, mean))
}

# However, the function is not robust to unusual inputs. Look at the following results, decide which ones are incorrect, and modify col_means() to be more robust. (Hint: there are two function calls in col_means() that are particularly prone to problems.)

col_means(mtcars) #works fine
col_means(mtcars[, 0]) #throws an error because 
col_means(mtcars[0, ]) #shows NA
col_means(mtcars[, "mpg", drop = F]) #gives weird output; it seems that it gives the mean per row
col_means(1:10) #error shows incorrect number of dimentions.
col_means(as.matrix(mtcars)) # Error in df[, numeric] : (subscript) logical subscript too long
col_means(as.list(mtcars)) # Error in df[, numeric] : incorrect number of dimension

mtcars2 <- mtcars
mtcars2[-1] <- lapply(mtcars2[-1], as.character)
col_means(mtcars2) #this returns row means
```

Define a correct function. It seems like all the above issues could be fixed by asking the user to include a dataframe as input. Could not solve it alone initially, so used <https://github.com/peterhurford/adv-r-book-solutions/blob/master/07_debugging/exercise2.R>

``` r
col_means2 <- function(df) {
  if (!is.data.frame(df)) stop("Input must be a data frame")
  if (0 %in% dim(df)) {
    warning("Dataframe is empty, no means to calculate.")
    return(NULL)
  }
  numeric <- sapply(df, is.numeric)
  if (FALSE %in% numeric) warning("Dropped ", sum(!numeric), " non-numeric columns.")
  numeric_cols <- df[, numeric, drop = FALSE]
  data.frame(lapply(numeric_cols, mean))
}
```

### 3. The following function "lags" a vector, returning a version of x that is n values behind the original. Improve the function so that it (1) returns a useful error message if n is not a vector, and (2) has reasonable behaviour when n is 0 or longer than x.

``` r
lag <- function(x, n = 1L) {
  if (!is.numeric(n) | length(n) != 1) 
      stop('n must be a numeric vector with at least length 1.')
  xlen <- length(x)
  if (n > xlen) stop('n must be smaller than the number of elements in x')
  if (n == 0) 
      warning('n is 0, so the lagged vector and the unlagged vector will be identical.')
  c(rep(NA, n), x[seq_len(xlen - n)])
}
```
