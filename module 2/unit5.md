---
layout: minimal
title: "Data Activity 3"
---

# Data Activity 4

## Task

Using the Crime Survey for England and Wales, 2013-2014: Unrestricted Access Teaching Dataset (see Unit 1), perform the following activities:

1. Create a boxplot for the variable 'antisocx'
Follow the instructions below to create a boxplot for assessing levels of anti-social behaviour that the survey respondents experience in their neighbourhood (use the variable: antisocx).

If you’re using ‘graphics’: Add “Levels of anti-social behaviour in neighbourhood ‘antisocx’” as a title and colour the plot in purple and colour the outliers in blue.

If you’re using ‘ggplot2’: Add “Levels of anti-social behaviour in neighbourhood ‘antisocx’ as a title, colour the plot in yellow and colour the outliers in red.

2. Create a bar plot using either the barplot() function or the ggplot() function to assess whether or not the survey respondents experienced crime in the 12 months prior to the survey (use the variable 'bcsvictim'). Give the graph a suitable title and choose a colour for the bars (e.g., orange).

## Process and Findings

```r
library(haven)
my_data <- crime <- read_sav("filelocation.sav")

#using ggplot - Figure 1.

library(ggplot2)

ggplot(data = my_data, mapping = aes(x = "", y = antisocx )) + 
  geom_boxplot(fill = "yellow", outlier.colour = "red") +
  labs(title = "Levels of anti-social behaviour in neighbourhood 'antisoc'")+
  theme_bw() #remove grey shading 

#can simplify with 
ggplot(my_data, aes("", antisocx )) + 
  geom_boxplot(fill = "yellow", outlier.colour = "red") +
  labs(title = "Levels of anti-social behaviour in neighbourhood 'antisoc'")+
  theme_bw() #remove grey shading

# if not using ggplot the boxplot seems more straight forward - Figure 2
boxplot(my_data$antisocx, main = "Levels of anti-social behaviour in neighbourhood 'antisoc'", 
        col = 'purple') #however no easy to colour outliers (?)

#outliers have to be added separately

# Add red outliers manually
# Get boxplot stats - identifies the outliers
stats <- boxplot.stats(my_data$antisocx)

# Add outliers in red
points(rep(1, length(stats$out)), stats$out, col = "blue", pch = 19) 

# activity 2

#ggplot - Figure 3

ggplot(my_data, aes(bcsvictim)) +
       geom_bar(fill = "orange") + 
  labs(title = "Victim of crime in past 12 months", )
  theme_bw()
  # x axis does not make sense as has a continuous scale (should be 0,1)
# need to convert bcsvictim to a factor. 
  my_data$bcsvictim <- factor(my_data$bcsvictim, 
                              levels = c(0, 1), 
                              labels = c("Not a Victim", "Victim"))
  ggplot(my_data, aes(bcsvictim)) +
    geom_bar(fill = "orange") +
    xlab("Victim Status") +
    ylab("Count") +
    ggtitle("Count of Victim Status") #DID NOT WORK - all data was converted to NA
 
#had to reload data and start again

str(my_data$bcsvictim) - #checking the data type, can see its floats. 
  library(ggplot2)

# Step 1: Make sure bcsvictim is numeric (even if it's a float)
my_data$bcsvictim <- as.numeric(as.character(my_data$bcsvictim))

# Step 2: Check unique values to confirm only 0s and 1s
unique(my_data$bcsvictim)
# Should return: 0, 1 — which is does

# Step 3: Remove any rows with NA (optional, if they exist)
# (was not required but keeping for info) my_data <- my_data[!is.na(my_data$bcsvictim), ]

# Step 4: Convert to factor with labels
my_data$bcsvictim <- factor(my_data$bcsvictim,
                            levels = c(0, 1),
                            labels = c("Not a victim", "Victim"))

# Step 5: Plot using ggplot2
ggplot(my_data, aes(bcsvictim)) +
  geom_bar(fill = "orange") +
  labs(title = "Victim of Crime",
       x = "Status",
       y = "Count") +
  theme_bw()

#barplot funtion - Figure 4
#remember to tabulate the frequencies (which i forgot the first time as seen below

barplot(my_data$bcsvictim, main = "victim of crime", col = "orange")

# gives error - r needs a table of frequencies

victim_counts <- table(my_data$bcsvictim)
barplot(victim_counts, main = "Victim of Crime", col = "orange")

# remember to convert 0/1 to labels (if not done)
my_data$bcsvictim <- factor(my_data$bcsvictim, 
                              levels = c(0, 1), 
                              labels = c("Not a Victim", "Victim"))
barplot(victim_counts,
        names.arg = c("Not a victim", "Victim"),
        main = "Victim of Crime",
        col = "orange",
        xlab = "Status",
        ylab = "Count") - #gives nice bar graph. #(image 4)


![Figure 1](r"C:\Users\alexa\Sonya\numerical analysis\unit5figure1.png")
  

  
```
## Learnings

remember to review data, check data type and possible values, e.g. for bar plot, the data was not initially present correctly as it was plooted on a continuous scale, and on correction of this, the values of 0 and 1 were given to the bars on the x-axis, these are unclear to the reader and should be converted to label of the factor for clarity. Ensuring that the data is in the correct format saves a lot of troubleshooting!


```

<p style="text-align: center; margin-top: 2em;">
  <a href="../index.html" style="text-decoration: none; background: #f0f0f0; padding: 0.5em 1em; border-radius: 5px; display: inline-block;">
    ⬅️ Back to Home
  </a>
</p>
