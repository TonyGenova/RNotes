### Chapter 1 - inner_join verb   
inner_join only keeps pbservation if it has exact match between first and second tables  
note: can pipe joins together for multiple tables!  
```R
#example, theme_id is in sets table, id is in themes table
#suffix adds to field names of shared named columns for readability
sets %>% inner_join(themes, by = c("theme_id" = "id"), suffix = c("_set", "_theme")) %>%
#can get a count of themes
  count(name_theme, sort = TRUE)

sets %>%
  # Add inventories using an inner join 
  inner_join(inventories, by = c("set_num")) %>%
  # Add inventory_parts using an inner join 
  inner_join(inventory_parts, by = c("id" = "inventory_id")) %>%
  #add join for colors
  inner_join(colors, by = c("color_id" = "id"), suffix = c("_set", "_color")) %>%
  #count by colors
  count(name_color, sort=TRUE)
```
### Chapter 2 - left and right joins   
left_join keeps all data in first table, will give NA if doesn't appear in second table  
```R
# Combine the star_destroyer and millennium_falcon tables
millennium_falcon %>%
  left_join(star_destroyer, by = c("part_num","color_id"), suffix = c("_falcon","_star_destroyer"))

# Aggregate Millennium Falcon for the total quantity in each part
millennium_falcon_colors <- millennium_falcon %>%
  group_by(color_id) %>%
  summarize(total_quantity = sum(quantity))
# Aggregate Star Destroyer for the total quantity in each part
star_destroyer_colors <- star_destroyer %>%
  group_by(color_id) %>%
  summarize(total_quantity = sum(quantity))
# Left join the Millennium Falcon colors to the Star Destroyer colors
millennium_falcon_colors %>%
  left_join(star_destroyer_colors, by = c("color_id"), suffix = c("_falcon", "_star_destroyer"))

inventory_version_1 <- inventories %>%
  filter(version == 1)
# Join versions to sets
sets %>%
  left_join(inventory_version_1, by = c("set_num")) %>%
  # Filter for where version is na
  filter(is.na(version))
```
right_join keeps all observations in second table, gives NAs where doesn't appear in first table  
replace_na can be used to replace NA values  
```R
parts %>%
  count(part_cat_id) %>%
  right_join(part_categories, by = c("part_cat_id" = "id")) %>%
  # Filter for NA
  filter(is.na(n))

parts %>%
  count(part_cat_id) %>%
  right_join(part_categories, by = c("part_cat_id" = "id")) %>%
  # Use replace_na to replace missing values in the n column
  replace_na(list(n= 0))
```
joining tables to themselves  
```R

themes %>% 
  # Inner join the themes table
  inner_join(themes, by = c("id" = "parent_id"), suffix = c("_parent", "_child")) %>%
  # Filter for the "Harry Potter" parent name 
  filter(name_parent == "Harry Potter")

# Join themes to itself again to find the grandchild relationships
themes %>% 
  inner_join(themes, by = c("id" = "parent_id"), suffix = c("_parent", "_child")) %>%
  inner_join(themes, by = c("id_child" = "parent_id"), suffix = c("_parent","_grandchild"))

themes %>% 
  # Left join the themes table to its own children
  left_join(themes, by=c("id" = "parent_id"), suffix = c("_parent", "_child")) %>%
  # Filter for themes that have no child themes
  filter(is.na(name_child))

```
### Chapter 3 - Full joins   
Full joins keeps all observations in both tables, whether or not they match  
```R

```
