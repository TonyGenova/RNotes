### Relational Operators  
'> and < can check for alphabetical order as well as numeric  
    can also be applied to vectors


### Logical Operators  
And &, Or | and Not !  
Not operator example: !is.numeric(5), to check for numerics  
Logicals can be applied to vectors for comparison  
& will compare each element of vector, && checks only first element of vector  
| will compare each element of vector, || checks only first element of vector  

### Loops (Chapter 2)  
While loop  
Can put a break statement into a while loop to end on a condition  
```R
ctr <- 1
while (ctr<=7){
    if(ctr %% 5 == 0) {
        break
    }
    print(paste("ctr is set to", ctr))
    ctr <- ctr + 1
}
```
  
For loop 
Can also use break in for loops
```R
for(var in seq){
    if(condition){
        break
    }
    expr
}
```
Can use next statement to skip based on a condition
```R
for(var in seq){
    if(condition){
        next
    }
    expr
}
```
Can get length of vector to loop, and use the index at the same time
```R
for(i in 1:length(vector)){
    print(vector[i])
}
```
Slightly different approach when working with lists
```R
# The nyc list is already specified
nyc <- list(pop = 8405837, 
            boroughs = c("Manhattan", "Bronx", "Brooklyn", "Queens", "Staten Island"), 
            capital = FALSE)

# Loop version 1
for (p in nyc) {
  print(p)
}

# Loop version 2
for (i in 1:length(nyc)) {
  #note double square bracket
  print(nyc[[i]])
}
``` 
Loop through a matrix  
```R
for (i in 1:nrow(ttt)) {
  for (j in 1:ncol(ttt)) {
    print(paste("On row ",i," and column ",j," the board contains ",ttt[i,j]))
  }
}
```
Loop through a character string, count until a condition  
```R
# Pre-defined variables
rquote <- "r's internals are irrefutably intriguing"
chars <- strsplit(rquote, split = "")[[1]]

# Initialize rcount
rcount <- 0

# Finish the for loop
for (char in chars) {
  if (char == "r"){
      rcount <- rcount + 1
  }
  if (char == "u"){
      break
  }
  
}

# Print out rcount
print(rcount)
```

### Functions (Chapter 3)  
```R
#basic structure
#can have default values in arguments
named_function <- function (args) {
    do work
   }
```
Can use return statement to end function and return a value based on a condition  

### Apply Functions (Chapter 4)  
Lapply - apply a function to a whole list or vector, always returns a list  
unlist(lapply(list, function) - returns a vector  
lapply allows for arguments in functions  
```R
#basic usage: 
lapply(list, function)  
#to return a vector instead of a list
unlist(lapply(list, function)
#specify argument by name after function
lapply(list, function, argument=x) 
```
Anonymous functions - can define functions inside the lapply call, ie don't have to name function first
```R
#instead of
select_first <- function(x) {
  x[1]
}
#can just use
names <- lapply(split_low, function(x) {x[1]})

```
sapply - "simplify apply"  - simplifies the resulting list to an array  
sapply seems to give more usable, workable output - tries to give an array back  
vapply - can specify output format  

### Useful Functions (Chapter 5)  
Data Utilities
R features a bunch of functions to juggle around with data structures::   

seq(): Generate sequences, by specifying the from, to, and by arguments.  
rep(): Replicate elements of vectors and lists.  
sort(): Sort a vector in ascending order. Works on numerics, but also on character strings and logicals.  
rev(): Reverse the elements in a data structures for which reversal is defined.  
str(): Display the structure of any R object.  
append(): Merge vectors or lists.  
is.\*(): Check for the class of an R object.  
as.\*(): Convert an R object from one class to another.  
unlist(): Flatten (possibly embedded) lists to produce a vector.  

Regular Expressions  
Can be used to check for patterns, to replace and extract if necessary  
Can search single strings or lists of multiple strings  
grepl(pattern = "x", x = <string>)  - returns TRUE if found, FALSE if not  
grep(pattern = "x", x = <string>)  - returns numerical index of where pattern is found   
        x: searches for any, ^x: searches only at beginning, x$: searches only at end  
 (g)sub(pattern = "x", replacement = "y", x = <string>)  
     replaces first found incident of search  
    gsub replaces all found incidents  
    can use "|" to search for more than one pattern to replace in same command  
Subset a vector using a TRUE/FALSE generated from grep:  
    hits <- grep(pattern = "@.*\\.edu$", x=emails)   
     emails[hits]  
  
Times & Dates  
to get current date: Sys.Date()  
to get current time and date: Sys.time()  
to create date object: as.Date("1971-05-14", format = "%Y-%d-%m")  
R date/time packages: lubridate, zoo, xts  
%Y: 4-digit year (1982)  
%y: 2-digit year (82)  
%m: 2-digit month (01)  
%d: 2-digit day of the month (13)  
%A: weekday (Wednesday)  
%a: abbreviated weekday (Wed)  
%B: month (January)  
%b: abbreviated month (Jan)  
```R
#The following R commands will all create the same Date object for the 13th day in January of 1982:
as.Date("1982-01-13")
as.Date("Jan-13-82", format = "%b-%d-%y")
as.Date("13 January, 1982", format = "%d %B, %Y")
```  
Instructive work from exercise
```R
# Definition of character strings representing dates
str1 <- "May 23, '96"
str2 <- "2012-03-15"
str3 <- "30/January/2006"
# Convert the strings to dates: date1, date2, date3
date1 <- as.Date(str1, format = "%b %d, '%y")
date2 <- as.Date(str2)
date3 <- as.Date(str3, format = "%d/%B/%Y")
# Convert dates to formatted strings
format(date1, "%A")
format(date2, "%d")
format(date3, "%b %Y")
```    

