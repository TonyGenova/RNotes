## RNotes
#### Notes on R Programming Language

##### Basic Keyboard Commands
Ctrl + L : Clear Console  
Ctrl + Shift  + Enter : Execute script (Also Source button)  
Ctrl + Enter : Execute next piece of code  

##### Online Resources  
 - #Rstats hashtag on Twitter
 - rseek.org search engine
 - https://community.rstudio.com/  

##### Code snippets I want to explore further
read_tsv - tab delimited read in  
coord_flip in ggplot, switches axes for view  
arrange and arrange(desc) - sorts data  
Select(-columns) - the minus sign removes only the named columns  
Mutate(rank) - to add ranking, minus sign ranks in reserve order  
select(first:last) selects a range of columns if they are consecutive  
dataframe - groupby and count will allow counts of categories if there are countable items in your data  

##### Update a code based on a date lookup
 
```R

library(tidyverse)
library(lubridate)
library(reprex)
library(fuzzyjoin)

#test code for updating based on date range
#these are override codes with date ranges
df <- data.frame (item_override  = c("a", "b", "c", "d"),
                  Override_Code = c(6,7,6,6),
                  Start_Date = c(as.Date('1950-01-01'),as.Date('2015-01-01'),as.Date('2018-01-01'),as.Date('2020-01-01')),
                  End_Date = c(as.Date('2015-01-01'),as.Date('2099-01-01'),as.Date('2099-01-01'),as.Date('2099-01-01')))
df

#this the data to update if an override is found
data_to_update <- data.frame (item  = c("a","a", "b","b", "c","c", "d","d"),
                              Code = c(1,1,1,1,1,1,1,1),
                              Date = c(as.Date('2014-04-25'),as.Date('2017-07-27'),as.Date('2011-08-17'),as.Date('2021-11-25'),
                                       as.Date('2015-12-01'),as.Date('2020-03-01'),as.Date('2018-01-01'),as.Date('2020-01-01')))

new_data <- fuzzy_left_join(data_to_update, df,
                by = c(
                "item" = "item_override", 
                "Date" = "Start_Date", 
                "Date" = "End_Date"
                ),
              match_fun = list(`==`, `>=`, `<=`)
            ) %>% 
        mutate(Code = ifelse(is.na(Override_Code),Code,Override_Code)) %>% 
        select(item, Code, Date)
```  

##### Add a default date if date is NA
 
```R
library(tidyverse)
library(lubridate)
library(reprex)
library(dplyr)

df <- data.frame (item  = c("a", "b", "c", "d"),
                  date = c(as.Date('2010-01-01'),as.Date('2015-01-01'),NA,NA))
#first part adds date, second part formats date
DB2 <- df %>% mutate(date = ifelse(is.na(date), as.Date('2020-01-01'), date)) %>%
  mutate(date = as.Date(date, origin = "1970-01-01"))
DB2

```


##### Basic File Commands
Read or write csv  
```R
DataName <- read.csv("FileName.csv")
write.csv(DataName, file="FileName.csv")

#using tidyverse library, sspecifying file has no headers
data <- read_csv("file example 1.csv", col_names = FALSE)
#another method - base R?
data <- read.csv("filename", header = FALSE)
#name columns
names(data) <- c("name1", "name2","name3")

```

##### Subset
Taking pieces of data, or eliminating pieces of data  
```R
# a basic subset
NewSubset <- OriginalData %>% select(ColumnA, ColumnB, ColumnC, ColumnD)
# remove lines with 0 in ColumnB 
NewDataFrameName <- subset(DataFrameName, !(ColumnB == 0))
#also set a dataframe to view the removed lines if necessary
RemovedLinesDataFrame <- subset(DataFrameName, (ColumnB == 0))
#add "NewColumn" column to existing DataFrame
DataFrameName <- DataFrameName %>% mutate(NewColumn=substr(`ExistingColumn`,1,6))


```
##### Working with dates
```R
#Set a variable for Date, year, month
VarDate <- as.Date("2020-03-31")
VarDateYear <- as.numeric(format(VarDate,"%Y"))
VarDateMonth <- as.numeric(format(VarDate,"%m"))

#change date format (if string?)  
mutate (DATE = ymd(DATE))  
```

##### Working with DataFrames
```R
#change a text column to numeric
DataFrame$ColumnName = as.numeric(as.character(DataFrame$ColumnName))

#create a vector as a counter, and another that keeps the first row as a "0" month, so count starts on row 2
#initilize calculations_de dataframe
projection_month <- 0:1451
#month 1 is same as month 0 for attained age, active cell it the row we are looking at
attained_age_months <- c((active_cell$AGE_1*12),(active_cell$AGE_1*12):((active_cell$AGE_1*12)+1450))
combined_df <- data.frame(projection_month, attained_age_months)
#calculate age in years
combined_df <- combined_df %>% mutate(attained_age_years = floor(attained_age_months/12))

cumprod() and rev(cumprod()) do some nice cumulative calculations

#take a specific row from a dataframe
new_df <- df[1,]
#can reference a certain row for a calculation (like a first row to calculate a difference)
dataframe_name[1,"variable_name"]
#so to calculate a running difference
variable_name - dataframe_name[1,"variable_name"]
#can calculate running cumulative numbers
cumprod(variable_name)
cumsum(variable_name)
#can put an if_else in a dataframe column calculation
mutate(new_column = if_else(other_column==1,0,other_column*.75))
#Basic Join
Basic_Join <- left_join(LeftSideData, RightSideData, by = "ColumnName" )
#join with a filter applied
Filter_Join <- left_join(LeftSideData, RightSideData, by = "ColumnName" ) %>% filter(ColumnB == 1,ColumnC == 1)
#Some code that helped me with an error once on text columns
Basic_Join$ColumnName = iconv(Basic_Join$ColumnName, to="UTF-8")
#convert NA values to 0
DataFrame$ColumnName[is.na(DataFrame$ColumnName)] <- 0
#updating dates to remove time stamp
DataFrame <- mutate(DataFrame,DateColumn =as.Date(OrigDateColumn,"%m/%d/%Y"))
#combine two dataframes with same structure
TwoDataFrames <- bind_rows(FirstDF,SecondDF)
#write to CSV
write.csv(DataFrame,"DataFrame.csv", row.names = FALSE)
#resort to get data in a specific order
DataFrame <- DataFrame[order(DataFrame$Col1,DataFrame$Col2,DataFrame$Col3),]

#group by and sums for data (pivot table like)
SumsForData <- BasicData %>% 
  group_by(Col1, Col2) %>% 
  summarise_at(vars(NumberData1, NumberData2), funs(sum))

#Code to calc month diff, used from
#https://stackoverflow.com/questions/1995933/number-of-months-between-two-dates
monnb <- function(d) { lt <- as.POSIXlt(as.Date(d, origin="1900-01-01")); lt$year*12 + lt$mon } 
# compute a month difference as a difference between two monnb's
mondf <- function(d1, d2) { monnb(d2) - monnb(d1) }
#examples
#mondf(as.Date("2008-01-01"), Sys.Date())
#mondf("2020-02-28", "2020-03-31")
#Add months since premium payment to DataFrame, VarDate is a global variable
DataFrame <- mutate(DataFrame, MosDifference = mondf(DataFrame$ColDate,VarDate))
     
#Create a month counter with a Flag based on conditions
DataTable <- mutate(DataTable, MonthsPlusPad = BasicCounter+1.5)
DataTable <- mutate(DataTable, MonthCounter = pmax(0,MonthsPlusPad-MosDifference))
#If a month is past the EP, 0, fully within the EP use 1, else a fraction
DataTable = DataTable %>% mutate(
  CheckMonth = case_when(
    MonthCounter == 0 ~ 0,
    MonthCounter > .99999999 ~ 1,
    TRUE ~ MonthCounter
  )
)

```  

##### Data.Table 
Used to do a more complicated join on three variables, with < and > conditions
```R
#convert a data frame to a data table
setDT(DataFrameToDT) 
#used this to convert some dates, but I believe this is lubridate
DataTable$NeededDate <- mdy(DataTableDT$NeededDate)
#code for a complex left join with < and > matching - note one date is being compared twice to check for being between a range
testmerge2 <- RightJoinDT[LeftJoinDT, on = .(RightSideCol=LeftSideCol, RightSideDate <= LeftSideDate, RightSideDate2 > LeftSideDate,
            .(ColumnA,ColumB,etc)]
#rename a column
testmerge2 <- testmerge2 %>% rename(NewName = OldName)




```

Factor  
```R
#from Data Camp exercise
#note factor rank assignment carries forward even when assigned to variable
# Create factor_speed_vector
speed_vector <- c("medium", "slow", "slow", "medium", "fast")
factor_speed_vector <- factor(speed_vector, ordered = TRUE, levels = c("slow", "medium", "fast"))

# Factor value for second data analyst
da2 <- factor_speed_vector[2]

# Factor value for fifth data analyst
da5 <- factor_speed_vector[5]

# Is data analyst 2 faster than data analyst 5?
da2>da5
```


Read from Internet URL  
```R
Data <- url("http://www.webpage.com", "r")
x <- readlines(Data)
#to print to screen
x
```

##### Data Types and Structures
Basic Data Types  
* Interger
* Numeric
* Character
* Logical (Boolean)
* Complex

Data Structures  
* Vector - A series of variables of the same type
* List - A series of variables than can have different types
* Matrix - A 2 dimensional array with a single data type
* Data Frame - An 2 dimensional array that can hold multiple data types
* Array - Multi dimensional, single data type

R uses Implicit Coercion - Data entered into a vector with multiple data types will default to data type that can handle all data without an error

Useful Data review commands
```R
str(VariableName) #provides information on the structure of a vector/data frame/etc
summary(VariableName) #provides summary information on the data contained within a vector/data frame/etc
mean(VariableName)
sd(VariableName)
```
##### Other Topics to explore
 - R Markdown

##### TAPPLY
copied from http://www.r-bloggers.com/r-function-of-the-day-tapply/, with silght modifications for clarity  
>The tapply function is useful when we need to break up a vector into groups defined by some classifying factor, compute a function on the subsets, and return the results in a convenient form. You can even specify multiple factors as the grouping variable, for example treatment and sex, or team and handedness.

```R
# Generic structure
# tapply(Summary Variable, Group Variable, Function)

# Medical Example - take mean age grouped by treatment type
tapply(medical.example$age, medical.example$treatment, mean)
Treatment   Control
 62.26883  60.30371
# Baseball Example - take max average grouped by team
tapply(baseball.example$batting.average, baseball.example$team, max)
Team A    Team B    Team C    Team D    Team E
0.3784396 0.3012680 0.3488655 0.2962828 0.3858841  

#note sort() can be applied to tapply to sort the results
sort(tapply(baseball.example$batting.average, baseball.example$team, max))
```

##### Misc Tricks
is.na(DF$Variable) can be applied to a Logical to get a count of NAs, or !is.na to get a count of not NAs.  Taking the mean of this will get a proportion, with TRUE = 1 and FALSE = 0.  
Use all.equal(x,y) instead of == for better decimal precision at hundreths decimal point

#####Regression  
To perform a regression calculation:  
```R
# Can have 1 or more independent variables
model = lm(DependentVariable ~ IndVar1 + IndVar2, data=DataFrameName)
#To view results of regression
summary(model)
#Residuals are stored in model$residuals.  Can calculate SSE with
SSE = sum(model$residuals^2)
```
Notes on regression  
* T Value is (estimate of coeff/standard error).  A larger absolute T Value indicates a more significant variable.
* Pr >/t/ compares to the probability the true value of the coeff is actually zero.  Smaller values indicate less probability.  R uses the stars next to the variables to indicate significant varaibles.
* Multicollinerarity is when to independent variables are highly correlated.  This can skew regression results - review independent variables for correlation, remove highly correlated variables. 
* Remove variables from the regression one at a time, as removing one variable may cause another variable to become more significant.  Use qualitative logic as well as regression results when deciding what variables to remove. 

To calculate correlation
```R
cor(var1, var2) #correlation between two variables
cor(DataFrameName) #correlation between an entire data frame
```
To predict using the model  
```R
#predict new data using results from previous regression model
prediction = predict(model, newdata=DataFrame)
#to view prediction results
prediction
#to calculate SSE and R^2 of prediction
SSE = sum((ActualResults - prediction)^2)
SST = sum((ActualResults - mean(trainingData$DependentVariable))
RSq = 1-(SSE/SST)
```

##### Graphic Views of Data  
http://personality-project.org/r/r.graphics.html  
http://personality-project.org/r/r.short.html#graphics  
