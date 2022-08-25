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
```r
#difftime() allows for granular control of time difference calculations
difftime(date1, date2, units="xxx") # units can be secs, mins, hours, days, weeks
```
periods v durations  
periods - generally conform to human understanding of intervals  
durations - an exact count up or down, prefaced by a "d" in r    
```r
# month sequencing exercise
# A sequence of 1 to 12 periods of 1 month
month_seq <- 1:12 * months(1)
# Add 1 to 12 months to jan_31
jan_31 + month_seq  #doesn't quite work because not all months have 31 days
# Replace + with %m+%
jan_31 %m+% month_seq #iterates months forward in time
# Replace + with %m-%
jan_31 %m-% month_seq #iterates months backward in time
```  
Intervals  - can be used to test within a period or to calculate interval between periods  
```r
#intervals creates using
datetime1 %--% datetime2 #or
interval(datetime1, datetime2)
#once interval variable established
int_start(interval)
int_end(interval)
int_length(interval)
#to check if date within interval
datevariable %within% intervalvariable
#returns FALSE or TRUE

#to comapre two intervals for overlap
int_overlaps(interval1, interval2)
#returns FALSE or TRUE

#interesting code from problem on intervals
# Print halleys, a list of halleys comet appearances
halleys
# New column for interval from start to end date
halleys <- halleys %>% 
  mutate(visible = start_date %--% end_date)
# The visitation of 1066
halleys_1066 <- halleys[14, ] 
# Monarchs in power on perihelion date
monarchs %>% 
  filter(halleys_1066$perihelion_date %within% reign) %>%
  select(name, from, to, dominion)
# Monarchs whose reign overlaps visible time
monarchs %>% 
  filter(int_overlaps(halleys_1066$visible, reign)) %>%
  select(name, from, to, dominion)

```
### Chapter 4 - Problems/additional comments   
Can use force_tz()  to change time zones    
Can use with_tz() to view time in another time zone  
as.hms() can be used to view just a time with no associated date (in hms package)  
parse_date_time() is very flexible for parsing dates, but can be slow because of the flexibility  
If have a defined date format, quicker options exist:  
fastPOSIXct(), fast_strptime()  
stamp() can be used to set a date format you want to apply to other dates for viewing  
```r
#exercise to compare pasring dates with a benchmark
library(microbenchmark)
library(fasttime)

# Examine structure of dates
str(dates)

# Use fastPOSIXct() to parse dates
fastPOSIXct(dates) %>% str()

# Compare speed of fastPOSIXct() to ymd_hms()
microbenchmark(
  ymd_hms = ymd_hms(dates),
  fasttime = fastPOSIXct(dates),
  times = 20)
```
