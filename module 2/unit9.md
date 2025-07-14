---
layout: minimal
title: "Data Activity 7"
---

# Data Activity 7 / Unit 9

## Task

Using the Crime Survey for England and Wales, 2013-2014, perform the following activities:

Create a crosstab to assess how individuals’ experience of any crime in the previous 12 months bcsvictim vary by age group agegrp7. Create the crosstab with bcsvictim in the rows and agegrp7 in the columns, and produce row percentages, rounded to 2 decimal places.
Looking at the crosstab you have produced, which age groups were the most likely, and least likely, to be victims of crime?

## Process and Findings

```r
library(haven)
crime_data <- read_sav("path location")

table(crimedata$agegrp7, crimedata$bcsvictim)

> table(crimedata$agegrp7, crimedata$bcsvictim)
   
       0    1
  1  523  162
  2 1049  310
  3 1194  248
  4 1242  273
  5 1226  202
  6 1194  121
  7 1032   67

# this produced a table based on the number element of the factor, but to interpret this we need the label.
str(crimedata$agegrp7) #check that it is a labelled class (confirmed) - therefore use as_factor()

agegrp7_factor <- as_factor(crimedata$agegrp7)
bcsvictim_factor <- as_factor(crimedata$bcsvictim)
table(agegrp7_factor, bcsvictim_factor)

              bcsvictim_factor
agegrp7_factor Not a victim of crime Victim of crime
         16-24                   523             162
         25-34                  1049             310
         35-44                  1194             248
         45-54                  1242             273
         55-64                  1226             202
         65-74                  1194             121
         75+                    1032              67

#have the data required but needed to reformat with bcsvictim in rows

table(bcsvictim_factor, agegrp7_factor)

                       agegrp7_factor
bcsvictim_factor        16-24 25-34 35-44 45-54 55-64 65-74  75+
  Not a victim of crime   523  1049  1194  1242  1226  1194 1032
  Victim of crime         162   310   248   273   202   121   67

# add row percentages and round to two decimal places

round(prop.table(table(bcsvictim_factor, agegrp7_factor),1)*100,2)
                       agegrp7_factor
bcsvictim_factor        16-24 25-34 35-44 45-54 55-64 65-74   75+
  Not a victim of crime  7.01 14.06 16.01 16.65 16.43 16.01 13.83
  Victim of crime       11.71 22.42 17.93 19.74 14.61  8.75  4.84

¤ Which age group is most/least likely to be a victim of crime? See learnings. 


```
# Findings and Learnings
The task asks for victim of crime per age group to be presented with bcs victim in row and age group in column.  However, when then caluclating the row percentage this gives information on the proportion of overall repsondants who report that they are a victim of crime or not a victim of crime (divided by each age bracket) based on the row totals (total victims/not victims) without taking in to account that there is a difference in the total number of responses between each age group.  For example, looking at the readout above it could be interpreted that the group with the highest proportion reporting 'victim of crime' row are 45-54 year olds, however this group also has the highest reported precentage for 'not a victim of crime' due to the fact that total respondant numbers in each group are not considered.
To answer the question of which age group is most/least likely to be a victim of crime, we need to calculate the proportion of total repondants in each group that report being a victim of crime (ie with age-group in the rows) as shown below

Based on the below analysis, the youngest age group were most likely to report being a victim of crime and the oldest age group were least likely to report being a victim of crime.

This highlights the importance of understanding the data format and the analyses undertaken.  Additionally it may be considered that certain groups may be more likely to respond to the survey if they have been a victim of crime and we should always be aware of confounding factors.  

```r
round(prop.table(table(bcsvictim_factor, agegrp7_factor),1)*100,2)

              bcsvictim_factor
agegrp7_factor Not a victim of crime Victim of crime
         16-24                 76.35           23.65
         25-34                 77.19           22.81
         35-44                 82.80           17.20
         45-54                 81.98           18.02
         55-64                 85.85           14.15
         65-74                 90.80            9.20
         75+                   93.90            6.10


```

<p style="text-align: center; margin-top: 2em;">
  <a href="../index.html" style="text-decoration: none; background: #f0f0f0; padding: 0.5em 1em; border-radius: 5px; display: inline-block;">
    ⬅️ Back to Home
  </a>
</p>
