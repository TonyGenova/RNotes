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
group_by and summarize
```r
#code examples
counties_selected %>%
  summarize(min_population = min(population), max_unemployment = max(unemployment), average_income = mean(income))
counties_selected %>%
  group_by(state) %>%
  summarize(total_area = sum(land_area),
            total_population = sum(population)) %>%
  # Add a density column
  mutate(density = total_population/total_area) %>%
  # Sort by density in descending order
  arrange(desc(density))
```
top_n - after a group_by, can take the extreme observations from group  
```r
#example usage
counties_selected %>%
  # Find the total population for each combination of state and metro
  group_by(state, metro) %>%
  summarize(total_pop = sum(population)) %>%
  # Extract the most populated row for each state
  top_n(1, total_pop) %>%
  # Count the states with more people in Metro or Nonmetro areas
  ungroup() %>%  #not really sure what ungroup is doing here??
  count(metro)
```
### Chapter 3 - Selecting and Transforming data  
```r
#can select columns by name or with a contains string or with a starts_with or ends_with string
#last_col() is also an option
#can us -column to remove a column
select(state, county,contains("work"), starts_with("income"))

#example from exercises
# Glimpse the counties table
glimpse(counties)
counties %>%
  # Select state, county, population, and industry-related columns
  #note colon allows to select a range of columns using the names of first and last
  select(state, county, population, professional:production) %>%
  # Arrange service in descending order 
  arrange(desc(service))
 
#can rename a column
rename(new_name = old_name)
#also can rename in a select
select(column1, column2, new_name = old_name)

#transmute combines select and mutate
counties %>% transmute(state, county, fraction_men = men/population)
```
### Chapter 4 - Case Study  
```r
#can have multiple filter conditions using %in%
babynames_multiple <- babynames %>%
  filter(name %in% c("Amy", "Christopher")
  
babynames %>%
# Find the most common name in each year
  group_by(year) %>%
    top_n(1,number)
    
selected_names <- babynames %>%
  # Filter for the names Steven, Thomas, and Matthew 
  filter(name %in% c("Steven", "Thomas", "Matthew"))
# Plot the names using a different color for each name
ggplot(selected_names, aes(x = year, y = number, color = name)) +
  geom_line()
  
#grouped mutate combines group and mutate
#can be used to add a total to multiple lines of original data
babynames %>%
  group_by(year) %>%
  mutate(year_total = sum(number)) %>%
  ungroup() %>%
  mutate(fraction = number/year_total)
  
# Calculate the fraction of people born each year with the same name
babynames %>%
  group_by(year) %>%
  mutate(year_total = sum(number)) %>%
  ungroup() %>%
  mutate(fraction = number / year_total) %>%
  # Find the year each name is most common
  group_by(name) %>%
  top_n(1, fraction)
  
babynames %>%
  # Add columns name_total and name_max for each name
  group_by(name) %>%
  mutate(name_total = sum(number),
         name_max = max(number)) %>%
  # Ungroup the table 
  ungroup() %>%
  # Add the fraction_max column containing the number by the name maximum 
  mutate(fraction_max = number/name_max)
```
lag(), when applied to a vector, iterates each item by 1  
  so item 1 becomes item 2, etc  
  can them subtract from original vector to get changes  
```r

```
