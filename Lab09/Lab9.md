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
    ## [1,]    5    0    5    4    4    3    5    2    4     5
    ## [2,]    3    5    2    3    1    4    2    4    4     4
    ## [3,]    2    2    2    5    6    2    4    4    7     7
    ## [4,]    3    4    6    5    3    6    2    2    5     8
    ## [5,]    3    3    4    4    3    2    4    8    6     1

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
    ##       expr      min       lq     mean   median       uq       max neval
    ##     fun1() 11.59306 12.65574 7.477653 12.94143 13.08435 0.2241079   100
    ##  fun1alt()  1.00000  1.00000 1.000000  1.00000  1.00000 1.0000000   100

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
    ##        expr     min       lq     mean  median       uq      max neval
    ##     fun2(x) 7.59321 7.392162 6.643006 7.41783 7.640504 1.950108   100
    ##  fun2alt(x) 1.00000 1.000000 1.000000 1.00000 1.000000 1.000000   100
