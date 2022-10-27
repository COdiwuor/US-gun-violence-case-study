## CASE STUDY: U.S Gun Violence 
**Author: Domingo Guzman**

**Date: October 24, 2022**
#
_The case study follows the six step data analysis process:_

### ❓ [Ask](#1-ask)
### 💻 [Prepare](#2-prepare)
### 🛠 [Process](#3-process)
### 📊 [Analyze](#4-analyze)
### 📋 [Share](#5-share)
### 🧗‍♀️ [Act](#6-act)

#
![CSVGLogo](https://user-images.githubusercontent.com/77591203/197647981-c78f3dc8-f120-4499-ab57-d8b371a45c0d.png)


## Introduction

The Coalition to Stop Gun Violence (CSGV) is a non-profit gun control advocacy organization that is opposed to gun violence. The CSGV was founded in 1974, making it the nation’s oldest gun violence prevention organization. The CSGV believes that gun violence should be rare and abnormal. They pursue the goal of ending gun violence through policy development, advocacy, community engagement, and effective training. Through a combination of evidence-based policy development and aggressive lobbying, the CSGV is leading the way forward in fighting for a safer and stronger America. The organization has nine areas of focus regarding issues and campaigns. This case study will focus on two of them: 

1: Support for stricter mental health screening for firearm purchases. <br>
2: Ban the private sale of guns by instituting universal background checks.

Our analytics team has been asked to come up with strong, evidence-backed points that will assist the CSGV in gaining supporters.

## 1. Ask

**BUSINESS TASK: Analyze U.S gun violence data to create strong arguments that support the two areas of focus above for an upcoming campaign**

Primary stakeholders: Joshua Horwitz, executive director.

Secondary stakeholders: Local communities

## 2. Prepare
Data Sources: Gun Violence Incidents in the United States from Emmanuel F. Werr: https://www.kaggle.com/datasets/emmanuelfwerr/gun-violence-incidents-in-the-usa <br>
US Police Shootings from 2015- Sep 2022 from Ram Jas: https://www.kaggle.com/datasets/ramjasmaurya/us-police-shootings-from-20152022

The datasets have 3 CSV files, 24 columns, and 500,000 rows. The data also follows a ROCCC approach:

* Reliability: The Gun Violence Incidents in the Unites States data is complete and accurate. It comes from the Gun Violence Archive (GVA). The GVA website maintains a database of known shootings in the United States, coming from law enforcement, media and government sources from all 50 states. The US Police shootings data was scraped from Wikipedia.
* Original: GVA is an independent data collection and research group. Data on the US Police Shootings dataset gathered from The Counted, a website that tracks people killed by the police in the US.
* Comprehensive: The data includes names, dates, manner of death, state where death occurred, city, address, number of people killed & injured, age, race, gender, whether victim was armed, and if there were signs of mental illness. 
* Current: The data is current. It goes from 2013 to September 2022.
* Cited: The data is cited from <a href="https://www.gunviolencearchive.org/">Gun Violence Archive</a>, and <a href="https://en.wikipedia.org/wiki/Lists_of_killings_by_law_enforcement_officers_in_the_United_States">Wikipedia</a>

## 3. Process

Examining the data:

```
//insert data here
```

* The Gun Violence Incidents in the United States data covers data from 2013 - present day, while the US Police shootings covers data from 2015 - September 2022. This means that data from 2013-2014 will need to be factored out from the Gun Violence Incidents dataset so that the same years can be compared for both.

* The 'state' category in the US Police shootings dataset is abbreviated, while the full name of the state is typed out in the Gun Violence Incidents dataset. This must be fixed so both are the same.
