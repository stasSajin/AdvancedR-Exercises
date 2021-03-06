### 1. List three ways in which an environment differs from a list.

-   Environments are hierarchically structured, meaning that they have parent and children anvironments. List don't have that structure.
-   The objects in an environment have unique names, which is not true for lists
-   In lists, you can remove an object by setting it to NULL, in an environment, setting an entry to NULL will create a new binding to NULL
-   To compare environments, you should use `identical()` rather than `==`.

### 2. If you don't supply an explicit environment, where do ls() and rm() look? Where does &lt;- make bindings?

It first looks in the environment you are currently in. After that it moves up the hierarchy if nothing is found in the lower level environments.

### 3. Using parent.env() and a loop (or a recursive function), verify that the ancestors of globalenv() include baseenv() and emptyenv(). Use the same basic idea to implement your own version of search().

``` r
e<-globalenv()
while(environmentName(e) !="R_EmptyEnv") {
    e<-parent.env(e)
    cat(str(e, give.attr=FALSE))
}
```

    ## <environment: package:pryr> 
    ## <environment: package:stats> 
    ## <environment: package:graphics> 
    ## <environment: package:grDevices> 
    ## <environment: package:utils> 
    ## <environment: package:datasets> 
    ## <environment: package:methods> 
    ## <environment: 0x0000000005ca9588> 
    ## <environment: base> 
    ## <environment: R_EmptyEnv>

### 4. Modify where() to find all environments that contain a binding for name.

``` r
whereModified <- function(name, env = globalenv()) {
    #need to parse the name of a function into character
  if (!is.character(name)) { name <- deparse(substitute(name)) }
  if (identical(env, emptyenv())) {
    # Base case
    stop("Can't find ", name, call. = FALSE)
    
  } else if (exists(name, envir = env, inherits = FALSE)) {
    # Success case
    environmentName(env)
    
  } else {
    # Recursive case
    whereModified(name, parent.env(env))
    
  }
}

whereModified(start)
```

    ## [1] "package:stats"

### 5. Write your own version of get() using a function written in the style of where().

``` r
#?get()

getModified <- function(name, env = parent.frame()) {
  if (!is.character(name)) { name <- deparse(substitute(name)) }
    if (identical(env, emptyenv())) {
        # base case
        warning(paste(name," not found"))
        NULL
    } else if (is.element(name,ls(env))) {
        # success case
        env[[name]]
    } else {
        # recursive case
        getModified(name, env = parent.env(env))
    }
}


getModified(plot)
```

    ## function (x, y, ...) 
    ## UseMethod("plot")
    ## <bytecode: 0x00000000072769d8>
    ## <environment: namespace:graphics>

### 6. Write a function called fget() that finds only function objects. It should have two arguments, name and env, and should obey the regular scoping rules for functions: if there's an object with a matching name that's not a function, look in the parent. For an added challenge, also add an inherits argument which controls whether the function recurses up the parents or only looks in one environment.

``` r
#could not solve this on my own; looking around, I found this: https://github.com/peterhurford/adv-r-book-solutions/blob/master/06_environments/02_recursion/exercise3.r
fget<- function(name, env, inherits = TRUE) {
  if (!is.character(name)) { name <- deparse(substitute(name)) }
  test <- function(env,name) is.element(name,ls(env)) && is.function(env[[`name`]])
  if (test(env,name)) return(env[[`name`]])
  if (isTRUE(inherits)) {
    while (!identical(env,emptyenv())) {
      env <- parent.env(env)
      if (test(env,name)) return(env[[`name`]])
    }
  }
  warning(paste(name," not found"))
  NULL
}

fget('start',globalenv())
```

    ## function (x, ...) 
    ## UseMethod("start")
    ## <bytecode: 0x0000000007166fa0>
    ## <environment: namespace:stats>

### 7. Write your own version of exists(inherits = FALSE) (Hint: use ls().) Write a recursive version that behaves like exists(inherits = TRUE).

``` r
existsModified<- function(name, env = parent.frame(), inherits = FALSE) {
  if (!is.character(name)) { name <- deparse(substitute(name)) }
  if (!inherits) return(name %in% ls(env))
  if (identical(env, emptyenv())) {
    # base case
    FALSE
  } else if (name %in% ls(env)) {
    # success case
    TRUE
  } else {
    # recursive case
    existsModified(name, env = parent.env(env), inherits = TRUE)
  }
}

#function start does nto exist in the current environment.
existsModified('start')
```

    ## [1] FALSE

``` r
#the fget() function from previous exercise exists though.
existsModified('fget')
```

    ## [1] TRUE

### 8. List the four environments associated with a function. What does each one do? Why is the distinction between enclosing and binding environments particularly important?

These are from the book:

-   Enclosing: The enclosing environment is the environment where the function was created. Every function has one and only one enclosing environment. The enclosing environment is important for lexical scoping.
-   Binding: Binding a function to a name with &lt;- defines a binding environment.
-   Execution: Calling a function creates an ephemeral execution environment that stores variables created during execution.
-   Calling: Every execution environment is associated with a calling environment, which tells you where the function was called.

If you assign a function to a different environemnt other than the enclosing environment, the enclosing environment determines how the function finds values; the binding environments determine how we find the function. This is from the book: "The distinction between the binding environment and the enclosing environment is important for package namespaces. Package namespaces keep packages independent. For example, if package A uses the base mean() function, what happens if package B creates its own mean() function? Namespaces ensure that package A continues to use the base mean() function, and that package A is not affected by package B (unless explicitly asked for)."

You might find this issue when using the dplyr and plyr packages at the same time, where the function summarise in each package was slightly different.

### 9. Draw a diagram that shows the enclosing environments of this function:

``` r
f1 <- function(x1) {
  f2 <- function(x2) {
    f3 <- function(x3) {
      x1 + x2 + x3
    }
    f3(3)
  }
  f2(2)
}
f1(1)
```

    ## [1] 6

``` r
#not done yet
```

### 10. Expand your previous diagram to show function bindings.

Need to do.

### 11. Expand it again to show the execution and calling environments.

Need to do.

### 12. Write an enhanced version of str() that provides more information about functions. Show where the function was found and what environment it was defined in.

``` r
strModified<- function(name) {
    name <- deparse(substitute(name))
    list(
        functionname = name,
        callEnviron = parent.frame(),
        enclosEnviron = get(name, parent.env(environment()))
        )
}

strModified(mean)
```

    ## $functionname
    ## [1] "mean"
    ## 
    ## $callEnviron
    ## <environment: R_GlobalEnv>
    ## 
    ## $enclosEnviron
    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x000000000b262ce0>
    ## <environment: namespace:base>

### 13. What does this function do? How does it differ from &lt;&lt;- and why might you prefer it?

This function looks up the specified name functions and then binds a new value to them. If it does not find the function, it does not automatically create a function in the global environment like the function assign() does. Instead, it simply stops execution. This can be useful if you want to modify a variable that already exists.

``` r
rebind <- function(name, value, env = parent.frame()) {
  if (identical(env, emptyenv())) {
    stop("Can't find ", name, call. = FALSE)
  } else if (exists(name, envir = env, inherits = FALSE)) {
    assign(name, value, envir = env)
  } else {
    rebind(name, value, parent.env(env))
  }
}
rebind("a", 10)
#> Error: Can't find a
a <- 5
rebind("a", 10)
a
#> [1] 10
```

### 14. Create a version of assign() that will only bind new names, never re-bind old names. Some programming languages only do this, and are known as single assignment languages.

``` r
assignModified<- function(name, value, env = parent.frame()) {
    if (exists(name, envir = env, inherits = FALSE)) {
    stop("Name already exists ", call. = FALSE)
  } else {
    assign(name, value, env)
  }
}
```

### 15. Write an assignment function that can do active, delayed, and locked bindings. What might you call it? What arguments should it take? Can you guess which sort of assignment it should do based on the input?

I'll work on this one a bit later
