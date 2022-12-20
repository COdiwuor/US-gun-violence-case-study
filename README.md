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

* **Reliability - MED:** The Gun Violence Incidents in the Unites States data is complete and accurate. It comes from the Gun Violence Archive (GVA). The GVA website maintains a database of known shootings in the United States, coming from law enforcement, media and government sources from all 50 states. The US Police shootings data was scraped from Wikipedia.
* **Original - MED:** GVA is an independent data collection and research group. Data on the US Police Shootings dataset gathered from The Counted, a website that tracks the number of people killed by police in the US.
* **Comprehensive - HIGH:** The data includes names, dates, manner of death, state where death occurred, city, address, number of people killed & injured, age, race, gender, whether victim was armed, and if there were signs of mental illness. 
* **Current - HIGH:** The data is current. It goes from 2013 to September 2022.
* **Cited - MED:** The data is cited from <a href="https://www.gunviolencearchive.org/">Gun Violence Archive</a>, and <a href="https://en.wikipedia.org/wiki/Lists_of_killings_by_law_enforcement_officers_in_the_United_States">Wikipedia</a>

### Loading packages

```
library(tidyverse)
library(lubridate) 
library(dplyr)
library(ggplot2)
library(tidyr)
library(janitor)
```

## 3. Process

### Importing the datasets

```
# Read the dataframes
all_incidents <- read_csv(all_incidents.csv)
mass_shootings <- read_csv("mass_shootings.csv")
police_shootings <- read_csv("police_shootings.csv")
```

Examining the data:

```
head(all_incidents)
colnames(all_incidents)
dim(all_incidents)

head(mass_shootings)
colnames(mass_shootings)
dim(mass_shootings)

head(police_shootings)
colnames(police_shootings)
dim(police_shootings)
```

Removing columns that will not be used in the analysis:

```
all_incidents_clean <- all_incidents %>% select(-c(incident_id))
police_shootings_clean <- police_shootings %>% select(-c(id, name))
mass_shootings_clean <- mass_shootings %>% select(-c('Incident ID', Address))
```
Removing null values from columns in each dataset:

```
all_incidents_new <- na.omit(all_incidents_new)
mass_shootings_clean <- na.omit(mass_shootings_clean)
police_shootings_clean <- na.omit(police_shootings_clean)
```

The all_incidents dataset covers data from 2013 - present day, while the police_shootings dataset covers data from 2015 - September 2022. This means that data from 2013-2014 will need to be factored out from all_incidents so that the same years can be compared.

```
# removing years 2013-2014
all_incidents_new <- all_incidents_clean %>% filter(date >= "2014-12-31")
```

Let's confirm that the years 2013-2014 were removed by checking the end of the dataframe.

```
tail(all_incidents_new)
```

```
# A tibble: 6 × 6
  date       state      city          address           n_kil…¹ n_inj…²
  <date>     <chr>      <chr>         <chr>               <dbl>   <dbl>
1 2015-01-01 Florida    Alachua       7108 NW 92nd Pla…       0       0
2 2015-01-01 New Jersey Jersey City   Virginia Avenue         0       3
3 2015-01-01 New York   Staten Island 1307 Arthur Kill…       0       2
4 2015-01-01 Michigan   Saint Joseph  396 Upton Drive         1       0
5 2015-01-01 New York   Rochester     402 West Ridge R…       0       1
6 2015-01-01 Ohio       Lorain        2217 East 28th St       0       3
```

## 4: Analyze 

Now that the correct years for both datasets have been verified, the next step is fixing the 'state' categories. The 'state' category in the police_shootings dataset is abbreviated, while the full name of the state is spelled out in the all_incidents_new and mass_shootings datasets. This must be fixed so the state formats are all the same.

```
head(all_incidents_new)

# A tibble: 6 × 6
  date       state          city        address         n_kil…¹ n_inj…²
  <date>     <chr>          <chr>       <chr>             <dbl>   <dbl>
1 2022-05-28 Arkansas       Little Rock W 9th St and B…       0       1
2 2022-05-28 Colorado       Denver      3300 block of …       0       1
3 2022-05-28 Missouri       Saint Louis Page Blvd and …       0       1
4 2022-05-28 South Carolina Florence    Old River Rd          0       2
5 2022-05-28 California     Carmichael  4400 block of …       1       0
6 2022-05-28 Kentucky       Louisville  400 block of M…       0       1
```

```
head(police_shootings)

# A tibble: 6 × 17
     id name    date       manne…¹ armed   age gender race  city  state
  <dbl> <chr>   <date>     <chr>   <chr> <dbl> <chr>  <chr> <chr> <chr>
1     1 Tim El… 2015-01-02 shot    gun      53 M      A     Shel… WA   
2     2 Lewis … 2015-01-02 shot    gun      47 M      W     Aloha OR   
3     3 John P… 2015-01-03 shot a… unar…    23 M      H     Wich… KS   
4     4 Matthe… 2015-01-04 shot    toy …    32 M      W     San … CA   
5     5 Michae… 2015-01-04 shot    nail…    39 M      H     Evans CO   
6     6 Kennet… 2015-01-04 shot    gun      18 M      W     Guth… OK 
```

```
head(mass_shootings)

# A tibble: 6 × 7
  `Incident ID` `Incident Date`   State City …¹ Address # Kil…² # Inj…³
          <dbl> <chr>             <chr> <chr>   <chr>     <dbl>   <dbl>
1        271363 December 29, 2014 Loui… New Or… Poydra…       0       4
2        269679 December 27, 2014 Cali… Los An… 8800 b…       1       3
3        270036 December 27, 2014 Cali… Sacram… 4000 b…       0       4
4        269167 December 26, 2014 Illi… East S… 2500 b…       1       3
5        268598 December 24, 2014 Miss… Saint … 18th a…       1       3
6        267792 December 23, 2014 Kent… Winche… 260 Ox…       1       3
```

I will use the built in state.abb and match functions to achieve this:

```
# change all states into their abbreviations 

all_incidents_new$state <- state.abb[match(all_incidents_new$state, state.name)]

mass_shootings$State <- state.abb[match(mass_shootings$State, state.name)]
```

```
head(all_incidents_new)

# A tibble: 6 × 6
  date       state city        address                  n_kil…¹ n_inj…²
  <date>     <chr> <chr>       <chr>                      <dbl>   <dbl>
1 2022-05-28 AR    Little Rock W 9th St and Broadway St       0       1
2 2022-05-28 CO    Denver      3300 block of Clay St          0       1
3 2022-05-28 MO    Saint Louis Page Blvd and Vandevent…       0       1
4 2022-05-28 SC    Florence    Old River Rd                   0       2
5 2022-05-28 CA    Carmichael  4400 block of Manzanita…       1       0
6 2022-05-28 KY    Louisville  400 block of M St              0       1
```

```
head(police_shootings_clean)

# A tibble: 6 × 15
  date       manner_of_d…¹ armed   age gender race  city  state signs…²
  <date>     <chr>         <chr> <dbl> <chr>  <chr> <chr> <chr> <lgl>  
1 2015-01-02 shot          gun      53 M      A     Shel… WA    TRUE   
2 2015-01-02 shot          gun      47 M      W     Aloha OR    FALSE  
3 2015-01-03 shot and Tas… unar…    23 M      H     Wich… KS    FALSE  
4 2015-01-04 shot          toy …    32 M      W     San … CA    TRUE   
5 2015-01-04 shot          nail…    39 M      H     Evans CO    FALSE  
6 2015-01-04 shot          gun      18 M      W     Guth… OK    FALSE  
```

```
head(mass_shootings_clean)

# A tibble: 6 × 6
  `Incident Date`   State `City Or County` `# Killed` `# Injured` state
  <chr>             <chr> <chr>                 <dbl>       <dbl> <chr>
1 December 29, 2014 LA    New Orleans               0           4 LA   
2 December 27, 2014 CA    Los Angeles               1           3 CA   
3 December 27, 2014 CA    Sacramento                0           4 CA   
4 December 26, 2014 IL    East St. Louis            1           3 IL   
5 December 24, 2014 MO    Saint Louis               1           3 MO   
6 December 23, 2014 KY    Winchester                1           3 KY  
```
** Next is changing St. to Saint in cities **
potential issues:
fort worth, saint paul, saint louis, fort wayne, st petersburg, port st lucie, fort collins, 

## 5. Share

- [Gun-related deaths:](#gun-related-deaths)
- [Gun-related injuries:](#gun-related-injuries)
- [Gun-related deaths per year:](#gun-related-deaths-per-year)
- [Gun-related injuries per year:](#gun-related-injuries-per-year)
- [Number of Victims of Police Shootings by Race since 2015:](#number-of-victims-of-police-shootings-by-race-since-2015)
- [Percentages of Shootings by Mental Illness:](#percentages-of-shootings-by-mental-illness)

### Gun-related deaths:

Check the number of total gun-related deaths per state in the US. 

``` 
ggplot(data = all_incidents_new, aes(x = state, y = n_killed)) + geom_bar(stat = "identity", fill = "black", color = "darkred") + labs(title = "Gun-related Deaths per State") + theme(axis.text.y = element_text(hjust = 1, size = 8)) + coord_flip()
```
<br>

![DeathsbyState](https://user-images.githubusercontent.com/77591203/201796531-17d814ad-e648-41a1-ae18-02ecb0a734fd.png)

The above visualization clearly depicts the number of people that died from gun violence in each state from 2015-2022. Texas, California, and Florida are leading the country when it comes to these types of deaths. The CSVG can use this information to strengthen their precense in these higher-risk states. By holding more rallies and events, the CSVG will gain a large number of supporters due to these communities being impacted by gun violence the most.

### Gun-related injuries:

Check the number of total gun-related injuries per state in the US.

```
ggplot(data = all_incidents_new, aes(x = state, y = n_injured)) + geom_bar(stat = "identity", fill = "black", color = "darkred") + labs(title = "Gun-related Injuries per State") + theme(axis.text.x = element_text(angle = 90, hjust = 1, size = 8))
```
<br>

![InjuriesbyState](https://user-images.githubusercontent.com/77591203/203421261-e3bfab49-a98d-4d2c-b62f-d4b66877c188.png)

After inspecting the above graph it's clear that Illinois has the most gun-related injuries. Next is California, Texas, then Pennsylvania. We know from the first graph that California and Texas were high gun violence states. With the information from this injuries graph, we can see that Illinois and Pennsylvania also have a high number of gun violence incidents. This makes them great candidates for an increase in CSVG presence.

### Gun related deaths per year:

```
ggplot(data = df, aes(x = year, y = n_killed, group = 1)) + geom_line(color = "red") + geom_point() + labs(title = "Gun-related Deaths per Year")
```
<br>

![DeathsperYear](https://user-images.githubusercontent.com/77591203/204145993-dae27e5c-6b73-45cb-91ff-6ca372b5745f.png)

We can see from the ggplot() line graph that there is an increase in the number of deaths each year, with the exception of 2018. This is concerning because more and more people are getting killed yearly as a result of gun violence. 

### Gun related injuries per year:

```
ggplot(data = df2, aes(x = year, y = n_injured, group = 1)) + geom_line(color = "orange") + geom_point() + labs(title = "Gun-related Injuries per Year")
```
<br>

![InjuriesperYear](https://user-images.githubusercontent.com/77591203/205464629-2538d15c-58cb-41e5-a361-f0856045ec87.png)

From first glancing at this line graph, one can see that from 2015-2019 the number of injuries slightly increase each year. When it gets to 2020 and 2021, however, there is a huge increase in injuries. We can see from this that gun-related violence is only getting worse and worse as the years go by, just like it was depicted in the previous graph.

### Number of Victims of Police Shootings by Race since 2015

<br>

Now let's explore the police_shootings dataset. To start off, let's see the breakdown of victims according to their race. 

```
police_shoot_race <- police_shootings_clean %>% drop_na(race) %>% group_by(race) %>% dplyr::summarise(count = n()) %>% arrange(desc(count))
```

```
police_shoot_race$race <- factor(police_shoot_race$race, levels = unique(as.character(police_shoot_race$race)))
```

``` 
ggplot(data = police_shoot_race, aes(x = race, y = count)) + geom_col(fill = 'brown') + labs(title = "Number of Victims of Police Shootings by Race since 2015", x = 'Race', y = 'Number of Shootings') + geom_text(aes(label = count), vjust = -.5)
```
<br>

![](https://user-images.githubusercontent.com/77591203/208267634-97d41c3c-a4c6-41c7-9960-e1d56d73eb59.png)

W = White B = Black H = Hispanic A = Asian N = Native American O = Other

According to the data, the majority of lethal police shooting victims are white/caucasian. The next two with highest number of victims are blacks and hispanics. This is a valuable visual that the CSGV can use to gain the support of white, black, and hispanic community members.


The CSVG is in favor of stricter mental health screenings for firearm purchases. The next visualization will allow us to see the percentage of victims that were mentally ill.

### Percentages of Shootings by Mental Illness

```
pie(police_shoot_illness$percent, labels = police_shoot_illness$percent, main = 'Percentages of Shootings by Mental Illness', col = c('light blue', 'pink'))
legend('right', c("Not ill", "ill"), fill = col)
```

![](https://user-images.githubusercontent.com/77591203/208314431-192cbb9d-a2e6-4c97-b2c7-122977df5b1d.png)

76% of victims were not mentally ill while 24% had signs of mental illness. 24% is an alarming number because it means that these people were able to gain access to guns.

<br>

```
#Breakdown of victims by age group
us_pol_age_brackets <- police_shootings_clean 
%>% mutate(
age_group = dplyr::case_when(
 age >= 0 & age <= 20 ~ "0-20",
 age >= 21 & age <= 40 ~ "21-40",
 age >= 41 & age <= 60 ~ "41-60",
 age >= 61 & age <= 80 ~ "61-80",
 age >= 81 & age <=100 ~ "81-100"),
 age_group = factor(age_group, levels = c("0-20", "21-40", "41-60", "61-80", "81-100")))
 age_brackets <- us_pol_age_brackets %>% tabyl(age_group) %>% drop_na()
 ggplot(data = age_brackets, aes(x = age_group, y = n)) + geom_col(fill = 'salmon') + labs(title = "Police Shooting Victims by Age Group Since 2015", x = 'Age Group', y = 'Number of Victims') + geom_text(aes(label = n), vjust = -.5)
```
<br>

![](https://user-images.githubusercontent.com/77591203/208552837-a2dff19d-42b3-43fe-b388-3c054e6b782f.png)

The age brackets of "21-40" and "41-60" have the highest number of fatal police shootings. This closely aligns with the information given by the Federal Bureau of Prisons through August 27 that shows the median age to be 36. 

## 6. Act

## Insights Summary:
