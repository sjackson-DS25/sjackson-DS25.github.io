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
> 594/10617
[1] 0.05594801
> 224/10617
[1] 0.02109824

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
