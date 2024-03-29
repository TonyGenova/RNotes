### Chapter 1  
Tidyverse: Collection of tools in R for organizing and visualizing data 
Filter, arrange, and mutate can be combined with piping results to the next command
``` r
#filter to get subset of observations
data %>% filter(year == 2002) #example to filter for just one year
#arrange sorts observations based on variable
data %>% arrange(columnX) # arannges data based on columnX
data %>% arrange(desc(columnX)) # arannges data based on columnX, descending
#mutate can add a new variable or change an existing one
data %>% mutate(x = x*100)
data %>% mutate(y = x*50)
```
### Chapter 2 ggplot  
```r
#basic ggplot call
ggplot(data, aes(x = [xaxisvariable], y = [yaxisvariable])) +
  geom_point()
 ```
can use a logarithmic scale - each distance is a multiplication of value - may be better visualization of data  
```r
#add scale_x_log10() to use logarithmic scale
ggplot(data, aes(x = [xaxisvariable], y = [yaxisvariable])) +
  geom_point() + scale_x_log10()
```
can add a color based on a categorical variable and size based on a numerical variable    
```r
#add scale_x_log10() to use logarithmic scale
ggplot(data, aes(x = [xaxisvariable], y = [yaxisvariable], color = [zvariable], size = [zzvariable])) +
  geom_point() + scale_x_log10()
```
Faceting allows you to divide plots into smaller subplots  
```r
#add facet_wrap() to break into smaller subplot/categories  
ggplot(data, aes(x = [xaxisvariable], y = [yaxisvariable], color = [zvariable], size = [zzvariable])) +
  geom_point() + scale_x_log10() + facet_wrap(~ variable)
```
### Chapter 3 Grouping and Summarizing  
can use mean, sum, median, min, max in summarizing
```r
#example summarize for a single output
dataset %>% summarize(meandatavariable = mean(datavariable))
#can combine with filter
dataset %>% filter(year == 2020) %>% summarize(meandatavariable = mean(datavariable), totalYvariable = sum(Yvariable))
```
group_by() will allow summarizing via groups  
```r
dataset %>%
  group_by(variableZ) %>%
    summarize(meanVariableX = mean(variableX),
      totalvariableY = sum(variableY))
# can group_by(variableZ, VariableA), etc if have more than one group by - use same parenthetical
```
can combine, filter, group_by, and summarize 
can also save summary to a variable and then send to ggplot for plotting
```r
by_year <- gapminder %>% group_by(year) %>% 
  summarize(totalPop = sum(pop), meanLifeExp = mean(lifeExp))
ggplot(by_year, aes(x = year, y = totalPop)) + geom_point() + expand_limits(y = 0) 
#expand limits specifies where y will start
by_year_continent <- gapminder %>% group_by(year, continent) %>% 
  summarize(totalPop = sum(pop), meanLifeExp = mean(lifeExp))
ggplot(by_year_continent, aes(x = year, y = totalPop, color = continent)) + geom_point() + expand_limits(y = 0) 
```
### Chapter 4 Types of Visualizations  
Examining Line Plots, Bar Plots, Histograms, Boxplots  
```r
#to make line plot, specify geom_line()
ggplot(by_year_continent, aes(x = year, y = totalPop, color = continent)) + geom_line() + expand_limits(y = 0) 

#bar plot can be used to compare categories - geom_col() - x is categorical variable, y is measurement
# Summarize the median gdpPercap by continent in 1952
by_continent <- gapminder %>% filter(year == 1952) %>%
    group_by(continent) %>% summarize(medianGdpPercap = median(gdpPercap))
# Create a bar plot showing medianGdp by continent
ggplot(by_continent, aes(x = continent, y = medianGdpPercap)) + 
  geom_col()

#Histogram - geom_histogram() - only has x axis
#can set binwidth: geom_historgram(binwidth = n)
#can also use scale_x_log10() for presentation
ggplot(gapminder_1952, aes(x = pop_by_mil)) + geom_histogram(bins = 50)
ggplot(gapminder_1952, aes(x = pop)) + geom_histogram()+ scale_x_log10()

#Box plots - geom_boxplot(x = category, y = values compared)
ggplot(gapminder_1952, aes(x = continent, y = gdpPercap)) + geom_boxplot() + scale_y_log10()
ggplot(gapminder_1952, aes(x = continent, y = gdpPercap)) +
  geom_boxplot() + scale_y_log10() + ggtitle("Comparing GDP per capita across continents")

```
