#### Notes on R Programming Language

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
