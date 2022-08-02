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
