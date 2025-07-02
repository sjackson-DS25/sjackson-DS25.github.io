```r
library(haven)
healthdata <- read_sav("file path")
#there are 10617 obersrvations/people included in the data set

names(healthdata) # look at column names

#what percentage of people drink
healthdata$dnnow <- as_factor(healthdata$dnnow)
table(healthdata$dnnow)
> table(healthdata$dnnow)

            Refusal          Don't know Item not applicable                 Yes 
                  0                   0                   0                6712 
                 No 
               1822

6712+1822 # = 8534 total valid responses (excluding minors for whom N/A)
6712/8534 # = 0.786, = 79 % drink alcohol nowadays

#what is percentage women in the sample 
healthdata$sex <- as_factor(healthdata$Sex)
> table(healthdata$sex)

                Refusal              Don't Know Schedule not applicable 
                      0                       0                       0 
    Item not applicable                    Male                  Female 
                      0                    4852                    5765

4852+5765 # = 10617 (as expected from observation numbers)
5765/10617 # 0.5429 = 54% female

#what is the highest education level 
> unique(healthdata$topqual3)
[1] Foreign/other              NVQ2/GCE O Level equiv     NVQ4/NVQ5/Degree or equiv 
[4] NVQ3/GCE A Level equiv     No qualification           Higher ed below degree    
[7] <NA>                       NVQ1/CSE other grade equiv

# % divorced or separated
library(labelled)
healthdata$marstatc <- as_factor(healthdata$marstatc)
> table(droplevels(healthdata$marstatc))

                                         Single 
                                           1613 
                                        Married 
                                           4501 
Civil partnership including spontaneous answers 
                                              4 
                                      Separated 
                                            224 
                                       Divorced 
                                            594 
                                        Widowed 
                                            693 
                                     Cohabitees 
                                            979 
valideresponses <- na.omit(healthdata$marstatc) #remove NA responses
length(valideresponses) #8608 responses that are not NA
594/8608 # = 0.069
224/8608 # = 0.026

#graph drinks alcohol

drinks <- table(droplevels(healthdata$dnnow))
drinks
barplot(drinks, ylim = c(0, 7000), main = "Drinks Alcohol", col = "orange")

# Find the mean, median, mode, minimum, maximum, range and sd of household size, BMI and age at last birthday.
> colMeans(healthdata[, c("HHSize", "bmival", "Age")], na.rm = TRUE)
   HHSize    bmival       Age 
 2.850711 25.920498 41.561364

#medians
> apply(healthdata[,c("HHSize", "bmival", "Age")], 2, median, na.rm = TRUE)
  HHSize   bmival      Age 
 3.00000 25.59321 42.00000 

#mode
#need first to create a frequency table
freqHHsize <-table(healthdata$HHSize)
modeHHsize <- names(freqHHsize)[which.max(freqHHsize)]
modeHHsize # = 2

freqbmival <-table(healthdata$bmival)
modebmival <- names(freqbmival)[which.max(freqbmival)]
modebmival # = 13.76 #this value does not mean much; BMI is continuous variable and unlikely to get values the same

freqage <-table(healthdata$Age)
modeage <- names(freqage)[which.max(freqage)]
modeage # = 42

> describe(healthdata$HHSize)
   vars     n mean   sd median trimmed  mad min max range skew kurtosis   se
X1    1 10617 2.85 1.37      3    2.75 1.48   1  10     9 0.83      1.3 0.01
> describe(healthdata$bmival)
   vars    n  mean   sd median trimmed  mad  min   max range skew kurtosis   se
X1    1 8376 25.92 6.14  25.59   25.92 5.53 8.34 65.28 56.94 0.56     1.03 0.07
> describe(healthdata$Age)
   vars     n  mean    sd median trimmed   mad min max range  skew kurtosis   se
X1    1 10617 41.56 23.83     42    41.5 28.17   0 100   100 -0.02    -0.99 0.23

#box plots for household size, BMI and Age
#Household size
library(ggplot2)
ggplot(data = healthdata, aes(x = "", y = HHSize)) +
  geom_boxplot(fill = "steelblue", outlier.colour = "red", outlier.shape = 16) +
  labs(title = "Household Size", x = NULL, y = "Number of people") +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5))

# BMI boxplot
ggplot(data = healthdata, aes(x = "", y = bmival)) +
  geom_boxplot(fill = "steelblue", outlier.colour = "red", outlier.shape = 16) +
  labs(title = "Body Mass Index", x = NULL, y = "BMI") +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5))

#Age boxplot 
ggplot(data = healthdata, aes(x = "", y = Age)) +
  geom_boxplot(fill = "steelblue", outlier.colour = "red", outlier.shape = 16) +
  labs(title = "Age", x = NULL, y = "Years") +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5))


#stats

# 3a, which gender drinks more?
# Categorical variable and continuous variable. use independent t test or mann whitney
#test if alcohol drank is normally distributed
qqnorm(healthdata$totalwu) #data does not look normal
library(nortest) # data set was too large to run Sharipo-Wilk so use Anderson-Darling
> ad.test(healthdata$totalwu) #confirmed not normal 

	Anderson-Darling normality test

data:  healthdata$totalwu
A = 935.5, p-value < 2.2e-16

#therefore use non-parametic test; mann whitney u test 
> wilcox.test(totalwu~Sex, healthdata)

	Wilcoxon rank sum test with continuity correction

data:  totalwu by Sex
W = 11224018, p-value < 2.2e-16
alternative hypothesis: true location shift is not equal to 0

#P<0.05 therefore a significant difference, but which gender drinks more?
#visualise with a box plot?
boxplot(totalwu~Sex, healthdata)
library(labelled)
val_labels(healthdata$Sex)
#boxplot but remove outliers as the data was compressed due to these
boxplot(totalwu~Sex, healthdata,
        main = "Units Per Week by Gender",
        xlab = "Gender",
        ylab = "units per week",
        col = c("blue", "red"),
        outline = FALSE)
#conclude that males drink signficiantly more.

3b

#test which region drinks the most alcohol
#already shown that units per week is not normally distributed, need to check means of 3+ groups, Kruskal Wallis test
> kruskal.test(totalwu~gor1,healthdata) #P< 0.05 therefore there is a significant difference in total units per region

	Kruskal-Wallis rank sum test

data:  totalwu by gor1
Kruskal-Wallis chi-squared = 81.722, df = 8, p-value = 2.199e-14

# but which region drinks the most? need to do a post hoc analysis
library(dunn.test)
healthdata$totalwu_num <- as.numeric(healthdata$totalwu)
dunn.test(healthdata$totalwu_num, healthdata$gor1, method = "bonferroni")
                           Comparison of x by group                            
                                 (Bonferroni)                                  
Col Mean-|
Row Mean |          1          2          3          4          5          6
---------+------------------------------------------------------------------
       2 |   2.373457
         |     0.3172
         |
       3 |   2.302689   0.039008
         |     0.3833     1.0000
         |
       4 |   0.597156  -1.759432  -1.713791
         |     1.0000     1.0000     1.0000
         |
       5 |   2.086747  -0.200290  -0.227308   1.494336
         |     0.6644     1.0000     1.0000     1.0000
         |
       6 |   2.339014   0.050604   0.010470   1.744243   0.240699
         |     0.3480     1.0000     1.0000     1.0000     1.0000
         |
       7 |   6.631198   4.832598   4.540354   6.109648   4.770516   4.591121
         |    0.0000*    0.0000*    0.0001*    0.0000*    0.0000*    0.0001*
         |
       8 |   0.553627  -2.191854  -2.098200  -0.124031  -1.845799  -2.142915
         |     1.0000     0.5110     0.6460     1.0000     1.0000     0.5782
         |
       9 |  -0.418847  -2.989218  -2.876995  -1.054853  -2.648381  -2.924436
         |     1.0000     0.0503     0.0723     1.0000     0.1456     0.0621
Col Mean-|
Row Mean |          7          8
---------+----------------------
       8 |  -7.201977
         |    0.0000*
         |
       9 |  -7.480808  -1.081051
         |    0.0000*     1.0000

alpha = 0.05
Reject Ho if p <= alpha/2

val_labels(healthdata$gor1) #1,2,3,4,5,6 are all different from 7
> val_labels(healthdata$gor1)
              North East               North West 
                       1                        2 
Yorkshire and The Humber            East Midlands 
                       3                        4 
           West Midlands          East of England 
                       5                        6 
                  London               South East 
                       7                        8 
              South West                    Wales 
                       9                       10 
                Scotland 
                      11 

boxplot(totalwu~gor1, healthdata,
        main = "Units Per Week by Region",
        xlab = "region",
        ylab = "units per week",
        col = "orange",
        outline = FALSE,
        horizontal = TRUE)

medianunits <- tapply(healthdata$totalwu, healthdata$gor1, median, na.rm = TRUE)
medianunits

3c
# is there a difference betwen males/females and valid heigh or valid weight
# data is continuously distributed - check normality 
qqnorm(healthdata$htval) # the line is skewed so will check with shapiro-wilk test
shapiro.test(healthdata$htval) #the sample size is too large 
library(nortest)
> ad.test(healthdata$htval)

	Anderson-Darling normality test

data:  healthdata$htval
A = 398.24, p-value < 2.2e-16

#P-value > 0.05 therefore reject H0, and conclude that the data is not normally distributed

#repeat for valid weight data 
qqnorm(healthdata$wtval) # plot looks skewed, run andersson-darling test for normality (due to large sample size)
> ad.test(healthdata$wtval) #P-value > 0.05 therefore reject H0, and conclude that the data is not normally distributed
	Anderson-Darling normality test

data:  healthdata$wtval
A = 100.8, p-value < 2.2e-16

# need non parametric tests to answer these questions
#Mann whitney U test carried out as non-parametric data and two independent groups.
> wilcox.test(htval~Sex, healthdata)

	Wilcoxon rank sum test with continuity correction

data:  htval by Sex
W = 14713021, p-value < 2.2e-16
alternative hypothesis: true location shift is not equal to 0

# P < 0.05 therefore reject H0.  There is a statistically significant difference in the heights 
#between males and females (Mann-Whitney U test, P <0,05)

#difference in weight for males/females?
#Mann whitney U test carried out as non-parametric data and two independent groups. 
> wilcox.test(wtval~Sex, healthdata)

	Wilcoxon rank sum test with continuity correction

data:  wtval by Sex
W = 12449400, p-value < 2.2e-16
alternative hypothesis: true location shift is not equal to 0

# P < 0.05 therefore reject H0.  There is a statistically significant difference in weight 
#between males and females


3d
#What is the correlation between whether a person drinks nowadays,
#total household income, age at last birthday and gender?

# drinks nowadays and income , chi-squared as two categorical variables
table <- table(healthdata$totinc, healthdata$dnnow)
table  # results include do not know and refuse to answer, need to drop these levels. 
attr(healthdata$totinc, "labels") # showwhat codes refer to, need to drop 96 and 97
healthdata_clean <- subset(healthdata, !totinc %in% c(96, 97))
table <- table(healthdata_clean$totinc, healthdata_clean$dnnow)
table
chisqres <- chisq.test(table)
> chisqres

	Pearsons Chi-squared test

data:  table
X-squared = 370.77, df = 30, p-value < 2.2e-16

#drinking vs age, categorical and continuous variable, use Spearman's correlation
> cor.test(healthdata$Age, healthdata$dnnow, method = "spearman") 

	Spearmans rank correlation rho

data:  healthdata$Age and healthdata$dnnow
S = 9.7343e+10, p-value = 2.506e-08
alternative hypothesis: true rho is not equal to 0
sample estimates:
       rho 
0.06027989 

#drinking and gender
#two categorical values chi-squared test
table <- table(healthdata$Sex, healthdata$dnnow) 
chisqres <- chisq.test(table)
> chisqres

	Pearsons Chi-squared test with Yates continuity correction

data:  table
X-squared = 114.15, df = 1, p-value < 2.2e-16

#income and Age at last birthday
#categorical and contiuous variable, use Spearman's correlation
> cor.test(healthdata_clean$totinc, healthdata_clean$Age, method = "spearman")

	Spearmans rank correlation rho

data:  healthdata_clean$totinc and healthdata_clean$Age
S = 1.1326e+11, p-value < 2.2e-16
alternative hypothesis: true rho is not equal to 0
sample estimates:
       rho 
-0.2058734 


#income and gender - two categorical, chi-squared 
table <- table(healthdata_clean$totinc, healthdata_clean$Sex) 
table
chisqres <- chisq.test(table)
chisqres
> chisqres

	Pearsons Chi-squared test

data:  table
X-squared = 45.866, df = 30, p-value = 0.032

#gender and age, continous and categorical use spearmans
> cor.test(healthdata$Age, healthdata$Sex, method = "spearman")

	Spearmans rank correlation rho

data:  healthdata$Age and healthdata$Sex
S = 1.9343e+11, p-value = 0.001829
alternative hypothesis: true rho is not equal to 0
sample estimates:
       rho 
0.03024415 
