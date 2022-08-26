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
