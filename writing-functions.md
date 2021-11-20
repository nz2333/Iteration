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

    ##  [1]  0.380278898 -1.220077845 -0.984308964 -0.993415465 -0.022107586
    ##  [6] -0.537458798 -0.577684196  0.709292104  2.519089238  0.706232163
    ## [11]  0.536116515 -0.485045321 -0.454775079 -1.312404773  1.265719693
    ## [16]  0.024103104  1.008289707  1.996736447 -0.356399921 -0.927770211
    ## [21] -0.157339211  0.468070863  0.734506893 -0.290874764  0.192670836
    ## [26] -0.496674225  1.291661889 -1.527358089  0.008041192 -1.497115092

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

    ##  [1]  0.380278898 -1.220077845 -0.984308964 -0.993415465 -0.022107586
    ##  [6] -0.537458798 -0.577684196  0.709292104  2.519089238  0.706232163
    ## [11]  0.536116515 -0.485045321 -0.454775079 -1.312404773  1.265719693
    ## [16]  0.024103104  1.008289707  1.996736447 -0.356399921 -0.927770211
    ## [21] -0.157339211  0.468070863  0.734506893 -0.290874764  0.192670836
    ## [26] -0.496674225  1.291661889 -1.527358089  0.008041192 -1.497115092

Try my function on some other things:

``` r
z_scores(3)
z_scores("my name")
z_scores(mtcars)
z_scores(c(TRUE, TRUE, FALSE, TRUE)
```

    ## Error: <text>:5:0: unexpected end of input
    ## 3: z_scores(mtcars)
    ## 4: z_scores(c(TRUE, TRUE, FALSE, TRUE)
    ##   ^
