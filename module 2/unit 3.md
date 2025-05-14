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
