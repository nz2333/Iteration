Iteration writing functions
================
nz2333
11/19/2021

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.4     v dplyr   1.0.7
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   2.0.1     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_colour_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

## Do something simple

take a smple and turn into z scores

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  1.1140162 -1.2512019  0.4900649 -0.8402227 -0.5512311 -1.3369661
    ##  [7]  1.8157803  1.1048140  0.7995999  0.8839713 -1.5339016  0.5136595
    ## [13]  0.2196976 -1.7455866  0.3770321 -0.8845060 -0.9113495 -0.7495147
    ## [19]  0.9395801  0.6337736  0.7837103  1.2718429 -0.5012896  0.1257167
    ## [25] -0.3495108 -1.4146347  1.5557814  0.5237776 -0.5210918 -0.5618113

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input mst be numeric")
  }
  
  if(length(x) < 3) {
    stop("Input at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
}

z_scores(x_vec)
```

    ##  [1]  1.1140162 -1.2512019  0.4900649 -0.8402227 -0.5512311 -1.3369661
    ##  [7]  1.8157803  1.1048140  0.7995999  0.8839713 -1.5339016  0.5136595
    ## [13]  0.2196976 -1.7455866  0.3770321 -0.8845060 -0.9113495 -0.7495147
    ## [19]  0.9395801  0.6337736  0.7837103  1.2718429 -0.5012896  0.1257167
    ## [25] -0.3495108 -1.4146347  1.5557814  0.5237776 -0.5210918 -0.5618113

Try my function on some other things:

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input at least three numbers

``` r
z_scores("my name")
```

    ## Error in z_scores("my name"): Input mst be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input mst be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input mst be numeric

## Multiple outputs

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input mst be numeric")
  }
  
  if(length(x) < 3) {
    stop("Input at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x, 
    sd = sd_x
  )

}
```

check if the function works

``` r
x_vec = rnorm(1000)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0192 0.989

## multiple inputs

``` r
sim_data = 
  tibble(
    x = rnorm(100, mean = 4, sd = 3)
  )

sim_data %>%
  summarize(
    mean = mean(x), 
    sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.90  2.89

``` r
sim_mean_sd = function(sample_size, mu, sigma) {
  
  sim_data = 
  tibble(
    x = rnorm(n = sample_size, mean = mu, sd = sigma)
  )

sim_data %>%
  summarize(
    mean = mean(x), 
    sd = sd(x)
  )

}

sim_mean_sd(100, 6, 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.26  3.24

## Mean scoping example

``` r
f = function(x) {
  
  z = x + y
  
  z
  
}

x = 1
y = 2
 f(x =y)
```

    ## [1] 4

## Functions as arguments

``` r
my_summary = function(x, sum_func) {
  sum_func(x)
}

x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 3.015351

``` r
median(x_vec)
```

    ## [1] 3.049251

``` r
my_summary(x_vec, mean)
```

    ## [1] 3.015351
