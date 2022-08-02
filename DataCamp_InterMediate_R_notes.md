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

