---
layout: minimal
title: "Data Activity 2"
---

# Data Activity 2

## Task

Using the Crime Survey for England and Wales, 2013-2014: Unrestricted Access Teaching Dataset (see Unit 1), perform the following activities:
1.	Explore whether survey respondents experienced any crime in the 12 months prior to the survey using the variable bcsvictim.
2.	Create a frequency table to count if the survey respondents experienced any crime in the previous 12 months. Use the table() command.
3.	Assess the results and decide if you need to convert this variable into a factor variable. Use as_factor

## Process and Findings

```r

library(haven)
> my_data <- read_sav("filelocation.sav")
> table(my_data$bcsvictim)
   0    1 
7460 1383 

> as_factor(my_data$bcsvictim) # this produced an output for every entry as to whether they were a victim of crime or not,  my aim was to get the code/key for 0 and 1
> class(my_data$bcsvictim) # this did not give required information. Instead, it listed data structure as follows 
[1] "haven_labelled" "vctrs_vctr"     "double"  
> attr(my_data$bcsvictim, "labels")  # this command gives the labels associated with the factor.
Not a victim of crime       Victim of crime 
                    0                     1
# after trying various methods to get the 'labels' the following seems to be the most straightforward

library(haven)
crime <- read_sav("filelocations.sav")
head(crime)
table(crime$bcsvictim)
   0    1 
7460 1383

crime$bcsvictim <- as_factor(crime$bcsvictim)
table(crime$bcsvictim)

Not a victim of crime       Victim of crime 
                 7460                  1383 
```
