iteraetions and list columns
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

## Lists

``` r
l = list(
          vec_numeric = 5:8,
          vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
          mat = matrix(1:8, nrow = 2, ncol = 4),
          summary = summary(rnorm(100))
          )
```

## `for` loop

create a new list

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1), 
    b = rnorm(20, mean = 0, sd = 5),
    c = rnorm(20, mean = 10, sd = 0.2), 
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm[1]
```

    ## $a
    ##  [1] 2.923329 4.116438 2.843138 2.890153 3.942374 4.027218 2.829789 3.648631
    ##  [9] 4.068470 2.538249 6.172373 3.979622 4.289094 3.673798 2.990132 1.782695
    ## [17] 1.799776 4.015895 1.710258 3.898962

pause and get the old function.

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

I can apply this function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.41  1.06

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.380  6.22

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.185

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.33 0.838

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
output[[i]] = mean_and_sd(list_norm[[i]])
}
```

## Map

``` r
map(list_norm, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.41  1.06
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.380  6.22
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.185
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.33 0.838

map(list, function you want to do)

``` r
output = map(list_norm, median)
```

``` r
output = map_dbl(list_norm, median)
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns

``` r
listcol_df =
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 2.923329 4.116438 2.843138 2.890153 3.942374 4.027218 2.829789 3.648631
    ##  [9] 4.068470 2.538249 6.172373 3.979622 4.289094 3.673798 2.990132 1.782695
    ## [17] 1.799776 4.015895 1.710258 3.898962
    ## 
    ## $b
    ##  [1]  2.7482728 -4.1446649 -9.2789711 -2.9496081 -4.7225038  8.8565984
    ##  [7] -2.7243346 -5.6564845  6.2133325 -1.3785549 -3.8281978 13.2035172
    ## [13] -0.4485176 -6.3097742 -7.9469845 -6.9513135  8.0718120  3.3418361
    ## [19]  4.0112500  2.2872238
    ## 
    ## $c
    ##  [1] 10.125713 10.222389  9.851736  9.986005  9.968438 10.206562 10.004889
    ##  [8] 10.075820 10.186733  9.934460  9.927314 10.219644 10.040295  9.835210
    ## [15]  9.639080  9.969527 10.418954  9.764363 10.148787  9.867686
    ## 
    ## $d
    ##  [1] -3.233372 -3.221242 -1.760679 -3.508602 -3.619299 -3.400744 -4.377914
    ##  [8] -2.619016 -3.257021 -2.800023 -4.246960 -5.207595 -4.072413 -3.086861
    ## [15] -1.706444 -2.970990 -3.482340 -4.043684 -2.571086 -3.400521

try operations:

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.41  1.06

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.41  1.06
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.380  6.22
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.185
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.33 0.838

applying this function to each list in this listcolumn

add a list column?

``` r
listcol_df %>%
  mutate(
    summary = map(samp, mean_and_sd), 
    medians = map_dbl(samp, median)
  )
```

    ## # A tibble: 4 x 4
    ##   name  samp         summary          medians
    ##   <chr> <named list> <named list>       <dbl>
    ## 1 a     <dbl [20]>   <tibble [1 x 2]>    3.66
    ## 2 b     <dbl [20]>   <tibble [1 x 2]>   -2.05
    ## 3 c     <dbl [20]>   <tibble [1 x 2]>   10.0 
    ## 4 d     <dbl [20]>   <tibble [1 x 2]>   -3.33
