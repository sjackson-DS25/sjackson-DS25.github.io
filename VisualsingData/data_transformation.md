---
layout: minimal
title: "Data transformation in R"
---


## Task

Complete exercises from Data Transformation in R (chapter 3) from Wickham et al (2017).  
<br><br>

### References
Wickham, H. (2017) *R for data science: import, tidy, transform, visualise, and model data*. Sebastopol, CA: O’Reilly Media.

<br><br>


##  Figure 1.  Graph Generated from exercise, investigating how flight delays vary over the course of the day.
Flights were grouped by hour of exoected departure and mean delay summarised before ploting using geom_line

![Density plot - Penguin body weight](https://raw.githubusercontent.com/sjackson-DS25/sjackson-DS25.github.io/master/VisualsingData/unit5_ggplot.png)

#exercises

```r


flights |> select(dep_time, sched_dep_time, dep_delay)
#dep_delay will be dep_time - sched_dep_time

# Brainstorm as many ways as possible to select dep_time, dep_delay, arr_time, and arr_delay from flights.
flights |> select(dep_time, dep_delay, arr_time, arr_delay )
flights |> select(dep_time, dep_delay:arr_time, arr_delay )
#use ! and exclude all other columns
# use starts or ends with, e.g.
flights |> select(starts_with("dep"), starts_with("arr"))

flights |> select(day, day, day)


flights |> select(contains("TIME"))

#Rename air_time to air_time_min to indicate units of measurement and move it to the beginning of the data frame.
flights |>
  rename(air_time_min = air_time) |>
  relocate(air_time_min)

# Which carrier has the worst average delays? - answer frontier airlines

flights %>% 
  summarise(delay = mean(dep_delay, na.rm = TRUE),
  .by = carrier)         
  
 # Can you disentangle the effects of bad airports versus bad carriers? 
#Why/why not? (Hint: Think about flights |> group_by(carrier, dest) |> summarize(n()).) 

flights %>% 
  summarise(delay = mean(dep_delay, na.rm = TRUE),
  n = n(), 
  .by = c(carrier, dest))

#Find the flights that are most delayed upon departure from each destination.
flights %>%
  group_by(dest) %>%
  slice_max(dep_delay, n = 1, with_ties = FALSE) %>% 
  relocate(dest)

 #How do delays vary over the course of the day. Illustrate your answer with a plot.
library(ggplot2)  - see figure 1. 

delay_by_hour <- flights %>%
  group_by(hour) %>% 
  summarise(delay = mean(dep_delay, na.rm = TRUE))

ggplot(delay_by_hour, aes(x = hour, y = delay)) +
  geom_line() +
  labs(
    title = "Average Departure Delay by Hour of Day",
    x = "Hour of Day",
    y = "Average Delay (minutes)")


#What happens if you supply a negative n to slice_min() and friends?
# assume it will take the 'opposite' - i.e. working the oppopsite direction.

flights %>%
  group_by(dest) %>%
  slice_max(dep_delay, n = -1, with_ties = FALSE) %>% 
  relocate(dest)

# it actually seems to list all rows still so the slice does not work

# Explain what count() does in terms of the dplyr verbs you just learned. What does the sort argument to count() do?
flights |> 
  group_by(month) |>
  count(sort= TRUE)

# count appears to count the number of rows in each group, description states it counts number of variables
# sort then orders them in descending order of n
flights %>% 
  count(dest, sort = TRUE)

#Write down what you think the output of below df will look like; 
#then check if you were correct and describe what group_by() does.
#alligned 1 to 5 to subsequent axis as predicted, group will creat two groups, a and b
#two groups shown in output # Groups:   y [2]. although df looks the same
#subsequent actions will be carried out on individual groups

df <- tibble(
  x = 1:5,
  y = c("a", "b", "a", "a", "b"),
  z = c("K", "K", "L", "L", "K")
)

df

df |>
  group_by(y)

#describe what arrange() does. Also comment on how it’s different from the group_by() in part (a).

#arrange will just put in order, df will look i.e. be shown with and then b, however the groups will not be formed and treated seperately in further actions
df |>
  arrange(y) # output as expected

#Write down what you think the output will look like; then check if you were correct and describe what the pipeline does.
# pipeline means, 'and then do': i.e. take what is before and then apply the result of this to the next function
df |>
  group_by(y) |>
  summarize(mean_x = mean(x)) #predict, groups by y and calculates mean of x for each group
#assumption was correct

df |>
  group_by(y, z) |>
  summarize(mean_x = mean(x)) # predict groups by y and then z and then summarises mean of resulting final groups
#prediction was correct

# how is output of below different to d?

df |>
  group_by(y, z) |>
  summarize(mean_x = mean(x), .groups = "drop") 
# predict it ignores groups when calculating mean of x
# it did not drop groups..
# reading explained that it removed the groups after, so data no longer grouped
#if .groups=drop not included then the last grouping variable(z) is removed but other ones are kept.

df |>
  group_by(y, z) |>
  summarize(mean_x = mean(x))

df |>
  group_by(y, z) |>
  mutate(mean_x = mean(x))

```



<a href="https://sjackson-ds25.github.io/VisualsingData/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Visualising Data</a>


