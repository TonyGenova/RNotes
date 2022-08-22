### Chapter 1  
``` r
#standard format for specifying dates in R
as.Date("2022-08-22")
```
The anytime package can make sense of various date formats  
``` r
# Load the anytime package
library(anytime)

# Various ways of writing Sep 10 2009
sep_10_2009 <- c("September 10 2009", "2009-09-10", "10 Sep 2009", "09-10-2009")

# Use anytime() to parse sep_10_2009
anytime(sep_10_2009)
```
