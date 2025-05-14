---
layout: minimal
title: "Data Activity 3"
---

# Data Activity 3

## Task

Using the Crime Survey for England and Wales, 2013-2014: Unrestricted Access Teaching Dataset (see Unit 1), perform the following activities:

1. Create a subset of individuals who belong to the ‘75+’ age group and who were a ‘victim of crime’ that occurred in the previous 12 months. Save this dataset under a new name 'crime_75victim'.

## Process and Findings

```r
library(haven)
crime <- crime <- read_sav("C:/Users/alexa/Sonya/SQL unit 5/numerical analysis data/UKcrimedata/spss/spss24/csew1314teachingopen.sav")
crime$bcsvictim <- as_factor(crime$bcsvictim)
crime$agegrp7 <- as_factor(crime$agegrp7)
table(crime$agegrp7) # to check output and age clasiifications
crime_75victim <- subset(crime, agegrp7 == "75+" & bcsvictim == "Victim of crime")

```
## Learnings

On first attempt the resulting crime_75victim table had no observations, it was (eventually) noticed that this was simply down to missing the capitalised 'V' at the begining of 'victim'.  This was corrected and a the table with 67 observations (out of the original 8843) was produced.  If the labels for the factors is understood then I would conider it easier to produce the table using the numerical part of the factor to help minimise such errors.  This can be checked using e.g.:

```r
attributes(crime$agegrp7)
attributess(data$bcsvictim)

#or alternatively

install.packages("labelled")
library(labelled)
val_labels(crime$agegrp7)
16-24 25-34 35-44 45-54 55-64 65-74   75+ 
    1     2     3     4     5     6     7 
val_labels(crime$bcsvictim)
Not a victim of crime       Victim of crime 
                    0                     1 

#create subset table now that numeric part of factor is understood.  
crime_75victim <- subset(crime, agegrp7 == 7 & bcsvictim == 1) 
