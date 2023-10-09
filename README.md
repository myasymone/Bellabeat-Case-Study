# Bellabeat Case Study
## A Google Data Analytics Case Study on how can a wellness company play it smart.



### Introduction

Bellabeat is a technology company specializing in innovative wellness and fitness solutions, with a particular focus on women's health and well-being. They offer a range of smart wearable devices and associated mobile applications designed to empower individuals to monitor and improve their health and fitness. Bellabeat's products provide users with the tools to track various aspects of their well-being, including physical activity, sleep, stress levels, and reproductive health. The company is committed to promoting a holistic approach to health and wellness, offering features and insights that help users make informed decisions about their lifestyles and daily routines. Bellabeat's products aim to seamlessly integrate technology into daily life while prioritizing user health and comfort.

View  [Bellabeat](https://bellabeat.com/) for more information.

## ASK

In this phase, we need to identify who the key stakeholders are. In this case, we have the following stakeholders: 

- **Urška Sršen:** Co-founder of Bellabeat - Chief Creative Officer - initiated the project.
-  **Sando Mur:** Cofounder of Bellabeat - Mathematician - Member of Bellabeat executive team
-  **Bellabeat marketing analytics team:** Bellabeat's analyst team - Helping to guide the company's marketing strategy  


#### Business Task:

Analyze fitness data from Fitbit users to uncover consumer usage insights and apply high-level marketing recommendations to one of Bellabeat's products to drive growth and enhance digital marketing strategies for the company. 

## PREPARE

Sršen encourages me to use public data that explores smart device users’ daily habits. She recommends a specific data set for us to view: 

- [FitBit Fitness Tracker Data](https://www.kaggle.com/arashnic/fitbit) Available on Kaggle (CC0: Public Domain, dataset made available through Mobius): This Kaggle data set contains personal fitness tracker from thirty Fitbit users. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits
 - This data is contained in 18 CSV files that were generated from a survey via Amazon Mechanical Turk between March 12, 2016 - May 12, 2016.

#### Data Integrity 

After viewing the data, I discovered there are some limitations: 

- The data only used 30 individuals, which is not a big sample size
- It wouldn't be considered original as it was received by a third-party provider, Amazon Mechanical Turk.
- I can see that it is slightly comprehensive as it provides information on physical activity, heart rate, and sleep which aligns with most of Bellbeat's products as well.
- The data is currently 7 years old and would no longer be considered relevant to the current representation of women today.
- The logs regarding weight contain only a few logs from 8 users which is less than a third of the users included in this survey.
- The citing for this data is low as it was collected from a third party.

## PROCESS 

In this phase, I viewed all 18 files and  decided to use 3 of the 18 datasets that are available to help with my analysis. The datasets chosen will be the following:

- dailyActivity_merged.csv
- sleepDay_merged.csv
- weightLoginfo_merged.csv

### Tools

At this time, I used Excel to load all files for my initial review of the data provided to see if there were any initial errors I see that needed to be cleaned. I used RStudio for my data cleaning, transformation, analysis, and visualization. 

