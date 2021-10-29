Lab9HPC
================
Sylvia Baeyens
10/29/2021

## Problem 2

``` r
# generate a n x k dataset with mean lambda
fun1 <- function(n = 100, k = 4, lambda = 4) {
  x <- NULL
  for (i in 1:n)
    x <- rbind(x, rpois(k, lambda))
  return(x)
}
fun1(5,10)
```

    ##      [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
    ## [1,]    2    7    9    5    3    9    3    5    5     4
    ## [2,]    5    1    6    5   10    4    0    1    2     8
    ## [3,]    4    2    7    4    1    3    5    7    4     3
    ## [4,]    3    5    3    4    4    4    8    5    2     2
    ## [5,]    2    5    2    4    8    4    4    6    4     7

``` r
fun1alt <- function(n = 100, k = 4, lambda = 4) {
  matrix(rpois(n*k, lambda), nrow=n, ncol= k)
}
#install.packages("microbenchmark")
library(microbenchmark)
microbenchmark::microbenchmark(
  fun1(),
  fun1alt(), unit="relative"
)
```

    ## Warning in microbenchmark::microbenchmark(fun1(), fun1alt(), unit = "relative"):
    ## less accurate nanosecond times to avoid potential integer overflows

    ## Unit: relative
    ##       expr      min       lq     mean   median       uq      max neval
    ##     fun1() 13.11218 12.45735 7.206244 12.35515 12.38762 0.258983   100
    ##  fun1alt()  1.00000  1.00000 1.000000  1.00000  1.00000 1.000000   100

``` r
#find the column max
# Data Generating Process (10 x 10,000 matrix)
set.seed(1234)
x <- matrix(rnorm(1e4), nrow=10)

# Find each column's max value
fun2 <- function(x) {
  apply(x, 2, max)
}

fun2alt <- function(x) {
  inc = max.col(t(x))
  x[cbind(inc, 1:ncol(x))]
}

all(fun2(x) == fun2alt((x)))
```

    ## [1] TRUE

``` r
# Benchmarking
microbenchmark::microbenchmark(
  fun2(x),
  fun2alt(x), unit="relative"
)
```

    ## Unit: relative
    ##        expr      min       lq     mean  median       uq     max neval
    ##     fun2(x) 7.165345 7.188462 6.572052 7.42574 7.879063 1.74888   100
    ##  fun2alt(x) 1.000000 1.000000 1.000000 1.00000 1.000000 1.00000   100

## Problem 3: Parallelize Everything

``` r
library(parallel)
my_boot <- function(dat, stat, R, ncpus = 1L) {
  
  # Getting the random indices
  n <- nrow(dat)
  idx <- matrix(sample.int(n, n*R, TRUE), nrow=n, ncol=R)
 
  # Making the cluster using `ncpus`
  # STEP 1: GOES HERE- creating a cluster
  cl <- makePSOCKcluster(ncpus)    
  # STEP 2: GOES HERE- preparing a cluster
  clusterExport(cl, c("stat", "dat", "idx"),envir = environment())
  
    # STEP 3: THIS FUNCTION NEEDS TO BE REPLACES WITH parLapply
  ans <- parLapply(cl=cl, seq_len(R), function(i) {
    stat(dat[idx[,i], , drop=FALSE])
  })

  # Coercing the list into a matrix
  ans <- do.call(rbind, ans)
  
  # STEP 4: GOES HERE
  stopCluster(cl)
  ans
  
}
```

``` r
# Bootstrap of an OLS
my_stat <- function(d) coef(lm(y ~ x, data=d))

# DATA SIM
set.seed(1)
n <- 500; R <- 1e3

x <- cbind(rnorm(n)); y <- x*5 + rnorm(n)

# Checking if we get something similar as lm
ans0 <- confint(lm(y~x))
ans1 <- my_boot(dat = data.frame(x, y), my_stat, R = R, ncpus = 2L)

# You should get something like this
t(apply(ans1, 2, quantile, c(.025,.975)))
```

    ##                   2.5%      97.5%
    ## (Intercept) -0.1430703 0.05292241
    ## x            4.8685251 5.04843669

Is it faster?

``` r
system.time(my_boot(dat = data.frame(x, y), my_stat, R = 4000, ncpus = 1L))
```

    ##    user  system elapsed 
    ##   0.035   0.006   1.305

``` r
system.time(my_boot(dat = data.frame(x, y), my_stat, R = 4000, ncpus = 2L))
```

    ##    user  system elapsed 
    ##   0.049   0.009   0.771
