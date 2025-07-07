---
layout: minimal
title: "Data Activity 5"
---

# Data Activity 6 / Unit 8

## Task

Using the Health_Data, please perform the following functions in R:

Find out the mean, median and mode of ‘age’ variable.
Find out whether median diastolic blood pressure is same among diabetic and non-diabetic participants.
Find out whether systolic BP is different across occupational group.

## Process and Findings

```r
library(haven)
health_data <- read_sav("path location")
mean(health_data$age, na.rm = TRUE)
median(health_data$age, na.rm = TRUE)
mode(health_data$age) # this does not give required result as in R it provides storage mode of the objects rather than the statistical mode (as shown below)

> mean(health_data$age, na.rm = TRUE)
[1] 26.51429
> median(health_data$age, na.rm = TRUE)
[1] 27
> mode(health_data$age)
[1] "numeric"

#in order to get mode we need to first create a frequency table
find_mode <-table(health_data$age)
mode <- names(find_mode)[which.max(find_mode)]
mode
> mode
[1] "26"

#Find out whether median diastolic blood pressure is same among diabetic and non-diabetic participants.
#first test for normality of data
shapiro.test(health_data$sbp)

	Shapiro-Wilk normality test

data:  health_data$sbp
W = 0.95474, p-value = 3.345e-06

#P<0.5 therefore reject null hypothesis, data is not normally distributed

shapiro.test(health_data$dbp)

	Shapiro-Wilk normality test

data:  health_data$dbp
W = 0.97052, p-value = 0.0002204

#P<0.5 therefore reject null hypothesis, data is not normally distributed
#non parametric test required.  2 non-paired samples, Mann-Whitney U test

wilcox.test(dbp~diabetes,health_data)
#H0 median dbp is the same in diabeteic and non-diabetic participants
#H1 there is a difference in dbp between diabetic and non-diabetic participants

	Wilcoxon rank sum test with continuity correction

data:  dbp by diabetes
W = 3804.5, p-value = 0.7999
alternative hypothesis: true location shift is not equal to 0

#result P = 0.799 therefore there is no evidence to reject H0, DBP is the same amongst diabetic and non-diabetic patients

#Find out whether systolic BP is different across occupational group
#ocupation has multiple categories, therefore use kruskal wallice test

kruskal.test(dbp~occupation,health_data)
#H0 dbp is not different across occupational groups 
#H1 dbp is different across occupational groups

	Kruskal-Wallis rank sum test

data:  dbp by occupation
Kruskal-Wallis chi-squared = 2.6281, df = 3, p-value = 0.4526

#result P = 0.4526 therefore there is no evidence to reject H0, DBP is not different across ocuppational groups
```


<p style="text-align: center; margin-top: 2em;">
  <a href="../index.html" style="text-decoration: none; background: #f0f0f0; padding: 0.5em 1em; border-radius: 5px; display: inline-block;">
    ⬅️ Back to Home
  </a>
</p>
