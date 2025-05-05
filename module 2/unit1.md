---
layout: minimal
title: "Data Activity 1"
---

## Task

Getting started with R; creating summary statistics based on Crime Survey for England and Wales, 2013-2014

## Process and Findings

```r
# Install and load necessary package
install.packages("haven")
library(haven)

# Read the data from the .sav file
my_data <- read_sav("filelocation.sav")

# List column headings
names(my_data)

# Display frequency table of 'antisocx' variable
table(my_data$antisocx)  # shows the data in the output window, however realized it is better to view this in the ‘my_data’ tab in the programming window.  

# Check the mean and median of 'antisocx'
mean(my_data$antisocx)  # this returned NA, so decided to try median to see if same issue
median(my_data$antisocx)  # - NA again, visualisation of data shows that ‘NA’ is a recorded response in many rows. Therefore, we need to tell R to ignore missing values, as shown below.

# Ignore missing values (NA)
mean(my_data$antisocx, na.rm = TRUE)
median(my_data$antisocx, na.rm = TRUE)

# Create summary statistics
summary(my_data$antisocx, na.rm = TRUE)

Results
Min.     1st Qu.  Median    Mean     3rd Qu.  Max.    NAs
-1.215  -0.788    -0.185    -0.007   0.528    4.015   6694

# Using 'describe' function to get more detailed summary statistics
library(psych)
describe(my_data$antisocx)

      vars    n   mean   sd  median trimmed   mad   min   max  range   skew kurtosis   se
X1    1     2149 -0.01  0.99  -0.18   -0.11  1.06  -1.22  4.01   5.23   0.8  0.23     0.02

```
## Interpretation
The interpretation of a negative value is not clear from the data table. Does it mean that antisocial behavior is not a concern?

The fact that the median is smaller than the mean suggests that the table is skewed to the right and that there could be some high-score outliers influencing the mean score. The min is -1.22 and the max is 4.01. The positive skew value confirms that the distribution is not normal and is right-skewed.

Learning
Potential missing/NA values must be checked for (can be seen in the ‘summary’), and R needs to be told to ignore these. Using ‘Describe’ gives more information about the variation and ‘skewness’ of the data.
