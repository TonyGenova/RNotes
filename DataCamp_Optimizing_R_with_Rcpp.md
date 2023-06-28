### Optimizing R Code with Rcpp

#### Chapter 1
Example to benchmark code pieces
```R
# Load microbenchmark
library(microbenchmark)

# Define the function sum_loop
sum_loop <- function(x) {
  result <- 0
  for (i in 1:length(x)){
    result = result + x[i]
  }
  result
}

# Check for equality 
all.equal(sum_loop(x), sum(x))

# Compare the performance
microbenchmark(sum_loop = sum_loop(x), R_sum = sum(x))
```

Checking data types in Rcpp and R
```R
# Load Rcpp
library(Rcpp)

# Evaluate 2 + 2 in C++
x <- evalCpp("2 + 2")

# Evaluate 2 + 2 in R
y <- 2 + 2

# Storage modes of x and y
storage.mode(x)
storage.mode(y)
# Change the C++ expression so that it returns a double
z <- evalCpp("2.0 + 2.0")
```

Casting types explicitly in Rcpp
```R
# Load Rcpp
library(Rcpp)

# Evaluate 17 / 2 in C++
evalCpp("17 / 2")

# Cast 17 to a double and divide by 2
evalCpp("(double)17 / 2")

# Cast 56.3 to an int
evalCpp("(int)(56.3)")
```

Function to calculate Euclidian Distance
```R
# Define the function euclidean_distance()
cppFunction('
  double euclidean_distance(double x, double y) {
    return sqrt(x*x + y*y) ;
  }
')

# Calculate the euclidean distance
euclidean_distance(1.5, 2.5)
```

Debugging Method - print to screen, stop on error
```R
# Define the function add()
cppFunction('
  int add(int x, int y) {
    int res = x + y ;
    Rprintf("** %d + %d = %d\\n", x, y, res) ;
    return res ;
  }
')

# Call add() to print THE answer
add(40, 2)

cppFunction('
  // adds x and y, but only if they are positive
  int add_positive_numbers(int x, int y) {
      // if x is negative, stop
      if(x < 0) stop("x is negative") ;
    
      // if y is negative, stop
      if(y < 0) stop("y is negative") ;
     
      return x + y ;
  }
')

# Call the function with positive numbers
add_positive_numbers(2, 3)

# Call the function with a negative number
add_positive_numbers(-2, 3)

```
