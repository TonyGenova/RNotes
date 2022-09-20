### Chapter 1 - tidy data   
tidy format - columns hold variables, rows hold observations, cells hold values  
```R
#separate function can split out data when two pieces are in one cell
population_df %>% separate(country, into = c("country", "continent"), sep = ",")
netflix_df %>% 
  # Split the duration column into value and unit columns
  separate(duration, into = c("value","unit"), sep = " ", convert = TRUE)
  
```
