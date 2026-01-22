---
layout: minimal
title: "Reading, Organising and Tidying Data in R"
---

# Reading, Organising and Tidying Data in R

<br>

## Task

Complete exercises from Tidy Data (chapter 5) and Data Import (chapter 7) from Wickham et al (2017).  
<br>

### References
Wickham, H. (2017) *R for data science: import, tidy, transform, visualise, and model data*. Sebastopol, CA: O’Reilly Media.

<br><br>


```r

# 1. What function would you use to read a file where fields were separated with |?
# answer - would use read_delim() or could explicitly specify, i.e read_delim("file", delim = "|")

# 2. Apart from file, skip, and comment, what other arguments do read_csv() and read_tsv() have in common?
# can compare using:
?read_csv

# appears all arguements can be used for both read_csv and read_tsv so difference is just tab vs comma seperated

# 3.  What are the most important arguments to read_fwf()?
# the path to the file and the field width or the field position.

# 4, sometimes ',' may be used in strings.what argument would be used to read the below
# "x,y\n1,'a,b'"

read_csv("x,y\n1,'a,b'", quote = '')

# what is wrong with following code lines
# viewing data suggests different number of variables to data

read_csv("a,b\n1,2,3\n4,5,6") #a

# running code gave a parsing error

read_csv("a,b,c\n1,2\n1,2,3,4") #b

# b. again the there is inconsistency in the number of columns and data provided for new lines.  it also created a parsing error

read_csv("a,b\n\"1") # c

# c. the use of " around the 1 is not closed.  it created a tibble that was 0 x 2 in shape. 

read_csv("a,b\n1,2\na,b") # d

# I cannot see what is wrong with this, although it standardises the 1,2 to characters rather than numerical data; otherwise code ran ok (ask in tutorial)?

read_csv("a;b\n1;3")  # e

# guess semi colons may cause an issue? should be read using read_delim instead?
# when running this it produces a 1 x 1 tibble with a;b and 1;3.  to eperate it should run read_delim
read_delim("a;b\n1;3", delim = ";")

#exercise 6
annoying <- tibble(
  `1` = 1:10,
  `2` = `1` * 2 + rnorm(length(`1`))
)

annoying
# extract variable 1 (note the back ticks)
annoying$`1`

#plot scatterplot of 1 vs 2

library(ggplot2)
annoying |>
  janitor::clean_names() |>
  ggplot(aes(x = x1, y = x2))+
  geom_point()

# Create a new column called 3, which is 2 divided by 1.

annoying$`3` <- annoying$`2`/annoying$`1` 
annoying

# Rename the columns to one, two, and three:
names(annoying) <- c("one", "two", "three")

annoying


#can read multiple files and stack them
sales_files <- c(  "https://pos.it/r4ds-01-sales",
                   "https://pos.it/r4ds-02-sales",
                   "https://pos.it/r4ds-03-sales"
)
read_csv(sales_files, id = "file") # id argument adds new column called file which shows where data obtained from

#if many files then instead of typing all, use list_files
sales_files <- list.files("data", pattern = "sales\\.csv$", full.names = TRUE)
sales_files

#write files to disk with write_csv() or write_tsv(), arguments are x (df to save) and file (save location)
# e.g write_csv(students, "students.csv")
#but when reload the file you lose the column fomating as it is read as text again
#instead can save in R binary format, RBS - then reload exact same R object that was saved

write_rds(students, "students.rds")
read_rds("students.rds")
```



<a href="https://sjackson-ds25.github.io/VisualsingData/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Visualising Data</a>


