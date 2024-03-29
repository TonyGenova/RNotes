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
Inner joins keeps observations where there is a match on both sides  
```R
# Start with inventory_parts_joined table
inventory_parts_joined %>%
  # Combine with the sets table 
  inner_join(sets, by = c("set_num") ) %>%
  # Combine with the themes table 
  inner_join(themes, by = c("theme_id" = "id"), suffix = c("_set", "_theme"))
  
# Count the part number and color id, weight by quantity
batman %>%
   count(part_num, color_id, wt = quantity)
star_wars %>%
  count(part_num, color_id, wt = quantity)
  
batman_parts %>%
  # Combine the star_wars_parts table 
  full_join(star_wars_parts, by=c("part_num", "color_id"), suffix = c("_batman", "_star_wars")) %>%
  # Replace NAs with 0s in the n_batman and n_star_wars columns 
 replace_na(list(n_batman = 0, n_star_wars = 0))
 
 parts_joined %>%
  # Sort the number of star wars pieces in descending order 
  arrange(desc(n_star_wars)) %>%
  # Join the colors table to the parts_joined table
  inner_join(colors, by=c("color_id" = "id")) %>%
  # Join the parts table to the previous join 
  inner_join(parts, by = "part_num", suffix = c("_color", "_part"))
```
#### semi_join and anti_join  
Filtering joins keep observations from the first table, but does not add variables  
semi_join: what observations in table 1 are in table 2?  
anti_join: what observations in table 1 are not in table 2?
```R
batmobile %>% semi_join(batwing, by = c("color_id", "part_num"))
themes %>% semi_join(sets, by = c("id", "theme_id"))
batmobile %>% anti_join(batwing, by = c("color_id", "part_num"))

# Use inventory_parts to find colors included in at least one set
colors %>%
  semi_join(inventory_parts, by = c("id" = "color_id"))

# determine which sets don't have a version number 1
# Use filter() to extract version 1 
version_1_inventories <- inventories %>%
  filter(version == 1)
# Use anti_join() to find which set is missing a version 1
sets %>%
  anti_join(version_1_inventories, by = c("set_num"))
```
##### visualizing set differences 
Working with lego set to determine differences in quantities and fractions of colors  
```R
batman_colors <- inventory_parts_themes %>%
  # Filter the inventory_parts_themes table for the Batman theme
  filter(name_theme == "Batman") %>%
  group_by(color_id) %>%
  summarize(total = sum(quantity)) %>%
  # Add a fraction column of the total divided by the sum of the total 
  mutate(fraction = total/sum(total))

# Filter and aggregate the Star Wars set data; add a fraction column
star_wars_colors <- inventory_parts_themes %>%
  # Filter the inventory_parts_themes table for the Batman theme
  filter(name_theme == "Star Wars") %>%
  group_by(color_id) %>%
  summarize(total = sum(quantity)) %>%
  # Add a fraction column of the total divided by the sum of the total 
  mutate(fraction = total/sum(total))

# combine batman and star wars data
batman_colors %>%
  # Join the Batman and Star Wars colors
  full_join(star_wars_colors, by = "color_id", suffix = c("_batman", "_star_wars")) %>%
  # Replace NAs in the total_batman and total_star_wars columns
  replace_na(list(total_batman = 0, total_star_wars = 0)) %>%
  inner_join(colors, by = c("color_id" = "id"))

# combine and also calc differences and filter for total
batman_colors %>%
  full_join(star_wars_colors, by = "color_id", suffix = c("_batman", "_star_wars")) %>%
  replace_na(list(total_batman = 0, total_star_wars = 0)) %>%
  inner_join(colors, by = c("color_id" = "id")) %>%
  # Create the difference and total columns
    mutate(difference = fraction_batman - fraction_star_wars,
           total = total_batman + total_star_wars) %>%
    # Filter for totals greater than 200
    filter(total >= 200)

# Create a bar plot using colors_joined and the name and difference columns
ggplot(colors_joined, aes(x = name, y = difference, fill = name)) +
  geom_col() +
  coord_flip() +
  scale_fill_manual(values = color_palette, guide = "none") +
  labs(y = "Difference: Batman - Star Wars")
```
### Chapter 4 - Case Study
```R
# Join in the questions and tags table, replace the NAs in tag_name
questions %>%
  left_join(question_tags, by = c("id" = "question_id")) %>%
  left_join(tags, by = c("tag_id" = "id")) %>%
  replace_na(list(tag_name = "only-r"))

```


