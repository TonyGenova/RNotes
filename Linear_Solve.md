#### Code to do a linear solve of an equation in R in steps  
#### Non-linear equations can be solved step-wise with this, each step will get closer to final solve

```r
library(dplyr)
library(rlang)
options (scipen=999)

#create data frame for loop output
StatsArray <- data.frame(Solve_Number = integer(),                 
                         Iteration_Number = integer(),
                         Iteration_Time = numeric(),
                         Result = numeric(),
                         Delta = numeric(),
                         Solve_Calc = numeric(),
                         Prior_Solve = numeric(),
                         Prior_Result = numeric(),
                         Solve_Check = numeric())

#initialize starting values 
Result = 0.0
New_Solve_Calc = 0.0
Current_Solve_Calc = 0.0
IterationNum = 0
TotalIterations = 0
Delta = 99
Prior_Solve_Calc = 40
Prior_Result = 100



## start cell calculation loop here??
Solve_Check = 0
while (Solve_Check == 0) {
  
  StartTime <- Sys.time()
  #these get updated every calc cycle
  if(IterationNum >= 1){
    Delta = Prior_Solve_Calc - New_Solve_Calc
    Prior_Result = Result
    Prior_Solve_Calc = New_Solve_Calc
  } else{
    Prior_Solve_Calc = 400
    Prior_Result = 100
    Delta = 99
  }
  
  
  #simple version of solving model
  # using Prior_Solve_Calc to solve a simple function
  #should solve to 25.5
  one_factor = Prior_Solve_Calc - 15
  two_factor = Prior_Solve_Calc - 36
  Result = one_factor + two_factor
  
  
  #calculation algorithim
  New_Solve_Calc = Prior_Solve_Calc - Delta * Result/(Prior_Result-Result)
  Current_Solve_Calc = New_Solve_Calc
  Solve_Check = if_else(abs(Result)<.01|(ceiling(abs(Current_Solve_Calc - Prior_Solve_Calc)*100)/100)<.01,Prior_Solve_Calc,0)
  
  IterationNum = IterationNum + 1
  TotalIterations = TotalIterations + 1
  
  # StatsArray[TotalIterations,1] = i # will need to update with loop
  StatsArray[TotalIterations,"Solve_Number"] = 1
  StatsArray[TotalIterations,"Iteration_Number"] = IterationNum
  StatsArray[TotalIterations,"Iteration_Time"] = Sys.time() - StartTime
  StatsArray[TotalIterations,"Result"] = Result
  StatsArray[TotalIterations,"Delta"] = Delta
  StatsArray[TotalIterations,"Prior_Solve"] = Prior_Solve_Calc
  StatsArray[TotalIterations,"Solve_Calc"] = New_Solve_Calc
  StatsArray[TotalIterations,"Prior_Result"] = Prior_Result
  StatsArray[TotalIterations,"Solve_Check"] = Solve_Check

}
```
