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
