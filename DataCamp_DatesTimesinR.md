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
### Chapter 2 - Lubridate  
Lubridate assumes UTC, not local time  
``` r
#Lubridate is a bit more flexible with parsing
ymd("2013-02-27")
ymd("2013.02.27")
ymd("2013 Feb 27th")
#also can use dmy(), myd(), etc

#can build a date from components in variables
make_date(year, month, day)

#can round dates as well
#all of these take a unit argument to specify unit to round to (hours, minute, day, week etc.
round_date() # round to nearest
ceiling_date() # round up
floor_date() # round down
#examples
r_3_4_1 <- ymd_hms("2016-05-03 07:13:28 UTC")
# Round down to day
floor_date(r_3_4_1, unit = "day")
# Round to nearest 5 minutes
round_date(r_3_4_1, unit = "5 minutes")
# Round up to week 
ceiling_date(r_3_4_1, unit = "week")
# Subtract r_3_4_1 rounded down to day
r_3_4_1 - floor_date(r_3_4_1, unit = "day")
```

### Chapter 3 - Taking Differences of dates and times  
