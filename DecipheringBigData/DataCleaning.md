---
layout: minimal
title: "Collaborative Discussion 1"
---

### Cleaning and Transformation - Units 4 and 5

## Task
Activities in unit 4 lecturecast were undertaken to understand and gain practical experience in basic data cleaning using python

The script below demonstrates how to clean, inspect, and replace coded column headers  
with human-readable labels 

It works through several data cleaning steps in python including:
- Reading CSV files 
- Handling encoding and newline issues
- Matching and replacing coded headers with descriptive labels
- Managing data/header mismatches safely

## Learnings and Reflections
The task reinforced the importance of review of the raw dataset, and selection of appropriate techniques for data cleaning and for managing missing or potentially inaccurate data.  Data cleaning methods must be considered in context of both the dataset and the analysis objectives to ensure reliable and unbiased outputs.

<br><br>

```r
# Review of CSV file shows that headings are coded.
# Headings were scraped by the authors and uploaded as a separate file.

# Step 1: Replace heading codes with readable headings.

from csv import DictReader

with open(r"C:\Users\alexa\Sonya\Module 3\book examples\mn.csv", 'r') as f1, \
     open(r"C:\Users\alexa\Sonya\Module 3\book examples\mn_headers.csv", 'r') as f2:

    data_rows = DictReader(f1)
    header_rows = DictReader(f2)
    data_rows = [d for d in data_rdr]
    header_rows = [h for h in header_rdr]

print(data_rows[:5])
print(data_rows[:5])


# Programming steps in book are outdated (for Python 2), so updated:
# - Removed 'rb' (no binary mode needed)
# - Use 'with open' to automatically close files

from csv import DictReader

with open(r"C:\Users\alexa\Sonya\Module 3\book examples\mn.csv", 'r') as f1, \
     open(r"C:\Users\alexa\Sonya\Module 3\book examples\mn_headers.csv", 'r') as f2:

    data_rdr = DictReader(f1)
    header_rdr = DictReader(f2)

    data_rows = [d for d in data_rdr]
    header_rows = [h for h in header_rdr]

print(data_rows[:5])
print(header_rows[:5])


# Needed to specify character encoding (utf-8) and newline to handle line endings correctly.
from csv import DictReader

with open(r"C:\Users\alexa\Sonya\Module 3\book examples\mn.csv", 'r', newline='', encoding='utf-8') as f1, \
     open(r"C:\Users\alexa\Sonya\Module 3\book examples\mn_headers.csv", 'r', newline='', encoding='utf-8') as f2:

    data_rdr = DictReader(f1)
    header_rdr = DictReader(f2)

    data_rows = [d for d in data_rdr]  # list generator function
    header_rows = [h for h in header_rdr]

print(data_rows[:2])
print(header_rows[:2])


# Now want to replace the current headers with readable information.
# Header information is a dictionary of code (in 'Name') and long value (in 'Label').

for data_dict in data_rows:  # iterates over dictionary rows
    for dkey, dval in data_dict.items():  # iterates over each key/value
        for header_dict in header_rows:  # iterates over all header rows
            for hkey, hval in header_dict.items():
                if dkey == hval:  # prints match if found between data keys and header data
                    print("match")

# This was code in book to check if keys matched — it was very long running. Do not run!


# Replace titles with more readable versions by matching keys from 'data' with values from 'header'
new_rows = []  # create empty list

for data_dict in data_rows:
    new_row = {}  # create dictionary for each row
    for dkey, dval in data_dict.items():
        for header_dict in header_rows:
            if dkey in header_dict.values():  # check if data key matches header values
                new_row[header_dict.get('Label')] = dval  # use label as key, data as value
    new_rows.append(new_row)

# Check if successful by printing first row
print(new_rows[0])

# Initial error due to missing capital 'L' in 'Label'; took a long time to resolve!


# Use zipping method to zip list of header values with data values
from csv import reader  # reader imports a list for each row instead of dict; zip needs lists

with open(r"C:\Users\alexa\Sonya\Module 3\book examples\mn.csv", 'r', newline='', encoding='utf-8') as f1, \
     open(r"C:\Users\alexa\Sonya\Module 3\book examples\mn_headers.csv", 'r', newline='', encoding='utf-8') as f2:

    data_rdr = reader(f1)
    header_rdr = reader(f2)

    data_rows = [d for d in data_rdr]  # list generator function
    header_rows = [h for h in header_rdr]

# Check to see if data and header lists are the same length
print(len(data_rows[0]))
print(len(header_rows))


# Mismatch of data length
# Examine the data
data_rows[0]
header_rows[:2]


# Remove rows where there is no match
bad_rows = []

for h in header_rows:
    if h[0] not in data_rows[0]:  # check if header code is in the first row of the data
        bad_rows.append(h)  # list of header codes not in data

for h in bad_rows:
    header_rows.remove(h)  # remove those headers where there is no match

print(len(header_rows))  # check updated length


# 159 values in data list and 150 in header list
# Why are there still 9 in 'data' without a match in 'header'?

all_short_headers = [h[0] for h in header_rows]  # make list of all short headers

for header in data_rows[0]:  # iterate over headers in data
    if header not in all_short_headers:  # check if header code exists in short header list
        print('mismatch', header)

# Small case mismatches are not of interest (UNICEF internal methods)
# Codes in upper case were not picked up with web scraping for some reason; decide whether to drop or manually look up.
# Consider implications of dropping data; do not just pick the simplest fix!
# Updated headers list was available in book data.

```
 


<hr>

<a href="https://sjackson-ds25.github.io/DecipheringBigData/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Deciphering Big Data</a>

<hr>
