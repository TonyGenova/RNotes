### Chapter 1 - Transforming Data with dplyr   
Select() - selects certain variables (columns) from dataset  
glimpse() gives a view into data  
filter() allows to pick certain data by conditions  
arrange(variable) and arrange(desc(variable)) sorts data by a variable in the dataset  
```r
glimpse(dataset)
dataset %>% select(column 1, column 2, etc) %>%
  filter(variable > xxxx) %>% arrange(desc(VariableY))
```
mutate() creates a new column  
```r
counties_selected %>%
  # Add a new column public_workers with the number of people employed in public work
  mutate(public_workers = population*public_work/100)
```
### Chapter 2 - Aggregating Data
count() will aggregte data based on a condition, can add sort = TRUE to automatically sort  
  wt = XVariable will give a total based on XVariable, instead of a basic count
```r
#usage example, this will total population by state in counties data 
counties %>% count(state, wt = population, sort = TRUE)
```
