## RNotes
#### Notes on R Programming Language

##### R Import of Multiple Files and Data Manipulation
```R
Input Files
(Reserves Example 1):
111111,5100001,10000
111111,5100002,5000
222222,5100003,10000
333333,5100004,10000
(QS file example):
111111,0.8
222222,0.5

# Load a CSV file that doesn't have headers
data <- read.csv("Reserves Example 1.csv", header=FALSE)
#name columns
names(data) <- c("Policy", "ClaimID","GrossRx")
#Load Splits
QSfile <- read.csv("QS file example.csv", header=FALSE)
#Name Columns
names(QSfile) <- c("Policy", "QS")

#add Split % to Rx file
library(plyr)
library(dplyr)
plyr1 <- join(data, QSfile, by = "Policy")
#add assumed Rx
plyr1 %>% mutate(Assumed=QS*GrossRx) %>% mutate(NotAssumed=(1-QS)*GrossRx)

Output:
Policy ClaimID GrossRx  QS Assumed NotAssumed
1 111111 5100001   10000 0.8    8000       2000
2 111111 5100002    5000 0.8    4000       1000
3 222222 5100003   10000 0.5    5000       5000
4 333333 5100004   10000  NA      NA         NA
```
##### Basic File Commands
Read or write csv  
```R
DataName <- read.csv("FileName.csv")
write.csv(DataName, file="FileName.csv")
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
