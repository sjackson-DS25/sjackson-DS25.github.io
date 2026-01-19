---
layout: minimal
title: "Classification methods"
---


## Task

---
layout: minimal
title: "Case Study: Exploring corporate tax rate variation in the USA"
---


## Task

Carry out the lab activities in the pages given below from the An Introduction to Statistical Learning with Applications in R textbook:

4.7 Lab: Classification Methods (Pages 171 – 177) - The Stock Market Data
8.3 Lab: Decision Trees (Pages 353 – 357) - Logistic Regression

<br><br>

Example graphs/data that were generated are shown below along with full coding. 

### References
Wickham, H. (2017) *R for data science: import, tidy, transform, visualise, and model data*. Sebastopol, CA: O’Reilly Media.

<br><br>


## Figure 1.  Scatter plot of raw data (coding shown below)


![Bar graph - Penguins species per island](https://raw.githubusercontent.com/sjackson-DS25/sjackson-DS25.github.io/master/VisualsingData/unit%203%20ch%201%20penguins%20species%20per%20island.jpeg)


<br><br>

# Chapter 1

```r
install.packages("ISLR2")
library(ISLR2)
data(Smarket)   # <— Load the dataset into the workspace
names(Smarket)
data(Smarket)
dim(Smarket)
summary(Smarket)

#cor() produces a matrix with all pairwise correlations among the predictors
cor(Smarket) # error because Direction is qualitative

cor(Smarket[, -9])  # corrleations are close to zero, except may be year/volume

attach(Smarket)
plot(Volume) # makes scatter plot (for this data type, different plot if different type) #Figure 1
View(Smarket)

#logistic regression
glm.fits <- glm(Direction ~ Lag1 + Lag2 + Lag3 + Lag4 + Lag5 + Volume, data = Smarket, family = binomial)
summary(glm.fits)

coef(glm.fits) # lists just the coefficients

summary(glm.fits)$coef # summary stats for coefficients 


summary(glm.fits)$coef[, 4] # gives column 4, ie p values

# predict () predicts probability that market will go up given value of predictors, 
# type = "response" tells R to output using P( Y=1|X ) rather than logit.  
#if no data provided to predict(), then probs are computed for training data
# to find direction of probability use contrasts ()

glm.prob <- predict(glm.fits, type ="response")
glm.prob[1:10]

contrasts(Direction)

#convert probabilities in to class labels 'up' and 'down' based on if more or less than 0.5

glm.pred <- rep("Down", 1250) # makes vector of 1250 down elements
glm.pred[glm.prob >.5] = "Up" # makes all > 0. 5 "up"

#create confusion matrix to determing how many were correctly classified

table(glm.pred, Direction)

#percentage accurately predicted
(507+145)/1250 # 0.5216 = 52.2 percent

#however training error rate is often underestimated (page 175)
#better to split data and have a training and a test set for more realistic error rate

#create vector for data from 2001-2004 and keep 2005 as test data

train <- (Year < 2005) 
Smarket.2005 <- Smarket[!train, ]
dim(Smarket.2005)

Direction.2005 <- Direction[!train]

#now fit logistic regression on 2001-2004 data using 'subset' argument
#then obtain predicted probs for 2005 (test set)

glm.fits <- glm(Direction ~ Lag1 + Lag2 + Lag3 + Lag4 + Lag5 + Volume, data = Smarket, family = binomial, subset = train)

glm.probs <- predict(glm.fits, Smarket.2005, type = "response")

# now table the predictions and actual results
glm.pred <- rep("Down", 252)
glm.pred[glm.probs >.5] = "Up"
table(glm.pred, Direction.2005)

> table(glm.pred, Direction.2005) #confusion matrix
        Direction.2005
glm.pred Down  Up
    Down   35  35
    Up     76 106
> 

mean(glm.pred == Direction.2005)
mean(glm.pred != Direction.2005)  # this is the test set error, ie the prediction did not match the direction

# authors suggest refit with just Lag 1 and Lag2 as they had smallest p-values (although were still high)

glm.fits <- glm(Direction ~ Lag1 + Lag2, data = Smarket, family = binomial, subset = train)

glm.probs <- predict(glm.fits, Smarket.2005, type = "response")

glm.pred <- rep("Down", 252)
glm.pred[glm.probs > .5] <- "Up"

table(glm.pred, Direction.2005)

mean(glm.pred == Direction.2005)
mean(glm.pred != Direction.2005)

#if want to predict returns associated with specific values of Lag1 or Lag2
# ie predict Direction on dat where Lag 1 = 1.2 and Lag 2 = 1.1  and when = 1.5 and -0,8
# use predict() function

predict(glm.fits, newdata = data.frame(Lag1 = c(1.2, 1.5), Lag2 = c(1.1, -0.8)), type = "response")


```



<a href="https://sjackson-ds25.github.io/VisualsingData/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Visualising Data</a>

<br><br>

Example graphs that were generated are shown below along with full coding. 

### References
Wickham, H. (2017) *R for data science: import, tidy, transform, visualise, and model data*. Sebastopol, CA: O’Reilly Media.

<br><br>

## Learnings and Reflections
When aggregating data to make sure that not drawing assumptions based on a low number of data points, i.e some columns may have large amounts of missing data; it is important to explore the data before diving in to analysis. 


<br><br>

## Figure 1.  Bar Graph illustrating penguin species found on each island.


![Bar graph - Penguins species per island](https://raw.githubusercontent.com/sjackson-DS25/sjackson-DS25.github.io/master/VisualsingData/unit%203%20ch%201%20penguins%20species%20per%20island.jpeg)



##  Figure 2.  Density plot illustrating distribution of body weight by species.

![Density plot - Penguin body weight](https://raw.githubusercontent.com/sjackson-DS25/sjackson-DS25.github.io/master/VisualsingData/density.png)


<br><br>

# Chapter 1

```r

# Install and load required packages
install.packages("tidyverse")
library(tidyverse)

install.packages("palmerpenguins")
library(palmerpenguins)

install.packages("ggthemes")
library(ggthemes)

# View the dataset
penguins

# Open interactive data viewer
View(penguins)

# Open help page for the dataset
?penguins

# Create an empty plot (dataset only)
ggplot(data = penguins)

# Map variables to axes
ggplot(
  data = penguins,
  mapping = aes(x = flipper_length_mm, y = body_mass_g)
)

# Add points
ggplot(
  data = penguins,
  mapping = aes(x = flipper_length_mm, y = body_mass_g)
) +
  geom_point()

# Colour by species
ggplot(
  data = penguins,
  mapping = aes(
    x = flipper_length_mm,
    y = body_mass_g,
    colour = species
  )
) +
  geom_point()

# Add line of best fit (linear model)
ggplot(
  data = penguins,
  aes(
    x = flipper_length_mm,
    y = body_mass_g,
    colour = species
  )
) +
  geom_point() +
  geom_smooth(method = "lm")

# Single trend line (global vs local aesthetics)
ggplot(
  data = penguins,
  aes(x = flipper_length_mm, y = body_mass_g)
) +
  geom_point(aes(colour = species)) +
  geom_smooth(method = "lm")

# Add shapes for each species
ggplot(
  data = penguins,
  aes(x = flipper_length_mm, y = body_mass_g)
) +
  geom_point(aes(colour = species, shape = species)) +
  geom_smooth(method = "lm")

# Add labels and colour-blind friendly palette
ggplot(
  data = penguins,
  aes(x = flipper_length_mm, y = body_mass_g)
) +
  geom_point(aes(colour = species, shape = species)) +
  geom_smooth(method = "lm") +
  labs(
    title = "Body mass vs flipper length",
    subtitle = "Adelie, Chinstrap, and Gentoo penguins",
    x = "Flipper length (mm)",
    y = "Body mass (g)",
    colour = "Species",
    shape = "Species"
  ) +
  scale_color_colorblind()

# Dataset dimensions
nrow(penguins)
ncol(penguins)
dim(penguins)

# Scatterplot of bill length vs bill depth
ggplot(
  data = penguins,
  aes(x = bill_length_mm, y = bill_depth_mm)
) +
  geom_point()

# Scatterplot by species
ggplot(
  data = penguins,
  aes(x = species, y = bill_depth_mm)
) +
  geom_point()

# Handle missing values
ggplot(
  data = penguins,
  aes(x = species, y = bill_depth_mm)
) +
  geom_point(na.rm = TRUE)

# Add caption
ggplot(
  data = penguins,
  aes(x = species, y = bill_depth_mm)
) +
  geom_point(na.rm = TRUE) +
  labs(title = "Data from the palmerpenguins package")

# Histogram
ggplot(penguins, aes(x = body_mass_g)) +
  geom_histogram(binwidth = 200)

# Density plot
ggplot(penguins, aes(x = body_mass_g)) +
  geom_density()

# Boxplot
ggplot(
  penguins,
  aes(x = species, y = body_mass_g)
) +
  geom_boxplot()

# Stacked bar chart
ggplot(
  penguins,
  aes(x = island, fill = species)
) +
  geom_bar()

# Relative frequency
ggplot(
  penguins,
  aes(x = island, fill = species)
) +
  geom_bar(position = "fill")

# Faceting
ggplot(
  penguins,
  aes(x = flipper_length_mm, y = body_mass_g)
) +
  geom_point(aes(colour = species, shape = species)) +
  facet_wrap(~island)

# Save plot to disk
ggplot(
  penguins,
  aes(x = flipper_length_mm, y = body_mass_g)
) +
  geom_point()

ggsave("penguin-plot.png")

# Save most recent plot
ggplot(mpg, aes(x = class)) +
  geom_bar()

ggplot(mpg, aes(x = cty, y = hwy)) +
  geom_point()

ggsave("mpg-plot.png")

# Help for saving files
?ggsave

```

# Chapter 2 Exercises

```r

this_is_a_really_long_name <- 2.5
this_is_a_really_long_name
seq(from = 1, to = 10)
#can often omit names of first several aguements, i.e
seq(1, 10)
x <- "hello world"

# Exercises
#1. why does below code not work

my_variable <- 10
my_varıable

#answer, typo in calling my_variable

# 2 - correct below commands code
#libary(todyverse)

#ggplot(dTA = mpg) + 
#  geom_point(maping = aes(x = displ y = hwy)) +
#  geom_smooth(method = "lm)

library(tidyverse)

ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() +
  geom_smooth(method = "lm")

# 3. Press Option+Shift+K/Alt+Shift+K. What happens ? How get to same place using menus?

# this shows keyboard shortcut quick reference, also found under tool tab

# 4. run below code, which plot is saved as mpg-plot.png and why?

my_bar_plot <- ggplot(mpg, aes(x = class)) +
  geom_bar()
my_scatter_plot <- ggplot(mpg, aes(x = cty, y = hwy)) +
  geom_point()
ggsave(filename = "mpg-plot.png", plot = my_bar_plot)

#it saves the bar plot, because this has been specified in the argument?
```



<a href="https://sjackson-ds25.github.io/VisualsingData/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Visualising Data</a>

