---
layout: minimal
title: "Web Scraping"
---

# Web scraping activity / unit 3

## Task

The aim of the task was to scrape a website to identify 'data scientist' jobs advertised and then parse this data in to a JSON file.  Web scraping was undertaken using Requests and Beautifulsoup4 modules.  The task aligned with learning outcome 3.

[Module Learning Outcomes](https://sjackson-ds25.github.io/DecipheringBigData/LearningObjectives.html)


## Refelctions and Learnings

Before web scraping, it is important to thoroughly examine the source data to understand its location, structure, variability and availability.  This ensures that the coding is written in a suitably comprehensive manner to obtain the required output  It is also important to be aware of potential inconsistencies and variability in how the source data is presented and structured, as this can significantly impact results, leading to skewed conclusions or missing information.  For example, job posting for data scientists may be advertised under "data science", "data scientist" or "bioinformatics" amongst others.  Therefore it is essential to be cognisant of both the limitations of the scraping code, and the completeness of the resulting readout. 

## Process and Findings

```r
# Install and load necessary llbraries
!pip install requests beautifulsoup4
import requests
from bs4 import BeautifulSoup
import json


# # URL of target site (a github fake job page
url = "https://realpython.github.io/fake-jobs/"

#get HTML content using requests
response = requests.get(url)
response.raise_for_status()  
# stops if there's an HTTP error;checks hhtp status code of the response; 
# if success then nothing happens, if error it raises exception therefore stops program (failed requests get flagged)

#3. parse the html using beautifulsoup; html.parser processes html in to structured format
soup = BeautifulSoup(response.text, "html.parser")

 find jobs containing data scientist
#need to start by inspecting the webpage to understand what to search for
#right click a job title and select 'inspect' - can see job title is stored under h2
data_sci_jobs = soup.find_all("h2", string=lambda text: "Data Scientist" in text)
 #only match if the sting inside the tag contains this phrase - need to be aware of variations in potential matches to ensure all is captured.

DS_jobs = [] #create the list of dictionaries
for job in data_sci_jobs:
    title = job.text.strip()
    company_element = job.find_next_sibling("h3", class_="company")
    company = company_element.text.strip() if company_element else "N/A"
    location_element = job.find_next_sibling("p", class_="location")
    location = location_element.strip() if location_element else "N/A"

    DS_jobs.append({"title": title, "company": company, "location": location})
    #strip to remove extra characters


#save scraped data (in jason file)
with open("data_science_jobs.json", "w", encoding="utf-8") as myfile:
    json.dump(DS_jobs, myfile, ensure_ascii=False, indent=4)

# (writes the information to the file (which is known by variable myfile, ensure_ascii=False makes sure any special characters etc are kept
#indent to improve readability

#check it has worked
print(f"Scraping complete! Found {len(DS_jobs)} 'Data Scientist' jobs.")
print("Data saved to 'data_scientist_jobs.json'")

Scraping complete! Found 0 'Data Scientist' jobs.
Data saved to 'data_scientist_jobs.json'

# hhmm, no data science jobs.  replace code to look for legal 

legal_jobs = soup.find_all("h2", string=lambda text: "Legal" in text) #only match if the sting inside the tag contains this phrase
legaljobs = [] #create the list of dictionaries
for job in legal_jobs:
    title = job.text.strip()
    company_element = job.find_next_sibling("h3", class_="company")
    company = company_element.text.strip() if company_element else "N/A"
    location_element = job.find_next_sibling("p", class_="location")
    location = location_element.text.strip() if location_element else "N/A"

    legaljobs.append({"title": title, "company": company, "location": location})
    #strip to remove extra characters

with open("legal_jobs.json", "w", encoding="utf-8") as myfile2:
     json.dump(legaljobs, myfile2, ensure_ascii=False, indent=4)   

print(f"Scraping complete! Found {len(legaljobs)} 'legal' jobs.")
print("Data saved to 'legal_jobs.json'")

Scraping complete! Found 2 'legal' jobs.
Data saved to 'legal_jobs.json'

#read file
import json
# Open the file for reading
with open("legal_jobs.json", "r", encoding="utf-8") as f:
    data = json.load(f)  # load JSON into a Python list/dict

{'title': 'Legal executive', 'company': 'Jackson, Chambers and Levy', 'location': 'N/A'}
{'title': 'Legal executive', 'company': 'Hartman PLC', 'location': 'N/A'}

```
 


<hr>

<a href="https://sjackson-ds25.github.io/DecipheringBigData/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Deciphering Big Data</a>

<hr>
