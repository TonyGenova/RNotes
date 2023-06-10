### Basic implementation of Rcpp ideas

```r
library(Rcpp)

testdf <- data.frame (colA  = c(1, 2, 3, 4),
                  colB = c(6,7,6,6),
                  colC = c(0,0,0,0))




#Rcpp changes the values of the data frame by reference
#meaning that when this code is run, the original dataframe is modified
cppFunction('void xfunction2 (DataFrame df) {
            
            NumericVector v1 = df["colA"];
            NumericVector v2 = df["colB"];
            NumericVector v3 = df["colC"];
            
            for(int i = 0; i < v3.size(); i++){
              v3[i] = v1[i] + v2[i];
            }
            }')

#same idea in R code for speed test
bench_f1 <- function (testdf) {
  for(i in 1:nrow(testdf)){
  testdf[i,'colC'] = testdf[i,'colA'] + testdf[i,'colB']
    }
  testdf
}

library(microbenchmark)
microbenchmark(bench_f1(testdf), xfunction2(testdf))


testdf2 <- data.frame (colA2  = c(.05, .1, .5, .75),
                       colB2 = c(1,2,3,4),
                       colC2 = c(0,0,0,0))

cppFunction('void xfunction3 (DataFrame df, DataFrame df2) {
            
            NumericVector v1 = df["colA"];
            NumericVector v2 = df["colB"];
            NumericVector v3 = df["colC"];
            
            NumericVector v1b = df2["colA2"];
            double multiplier = v1b[1];
            
            
            for(int i = 0; i < v3.size(); i++){
              v3[i] = v1[i] + v2[i] * multiplier;
            }
            }')

cppFunction('void xfunction4 (DataFrame df) {
            
            NumericVector v1 = df["colA"];
            NumericVector v2 = df["colB"];
            NumericVector v3 = df["colC"];
            
            for(int i = 0; i < v3.size(); i++){
              v2[i] = v1[i] + v2[i-1];
            }
            }')
```
