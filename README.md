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

**Installing and loading packages and libraries**

        install.packages("tidyverse")
        library("tidyverse")
      
        install.packages("setwd")
        library("lubridate")
        
        install.packages("skimr")
        library("skimr")                             #for summarizing data
        install.packages("dplyr")
        library("dplyr")                             # Using dplyr for grouping and summarizing
        library("ggplot2")                           #for data visualization for a scatter plot 
        
        
        install.packages("janitor")
        library("janitor")
        
        install.packages("plotrix")
        library("plotrix")

**Importing data into RStudio**

In this section, I named the data frames 'daily_activity', 'daily_sleep', and 'weight_log' for easier use during this analysis
       
        library(readr)
        daily_activity <- read_csv("Fitabase Data/dailyActivity_merged.csv")
        library(readr)
        daily_sleep <- read_csv("Fitabase Data/sleepDay_merged.csv")
        library(readr)
        weight_log <- read_csv("Fitabase Data/weightLogInfo_merged.csv")

**Viewing the data**

Review the basic information, column names, summary statistics, structure of data, and summary reports  to see if there are any missing values and obvious errors in formatting 


        head(daily_activity) # Display first few rows of the data
        head(daily_sleep)
        head(weight_log) 
        
        
        colnames(daily_activity)  #identify all column names
        colnames(daily_sleep)  #identify all column names
        colnames(weight_log)  #identify all column names
        
        summary(daily_activity)   # Summary statistics
        summary(daily_sleep)   # Summary statistics
        summary(weight_log)   # Summary statistics
        
        
        str(daily_activity)   # Structure of the data
        str(daily_sleep)   # Structure of the data
        str(weight_log)   # Structure of the data
        
        skim(daily_activity) # Generate a summary report for the dataset 
        skim(daily_sleep)
        skim(weight_log)

        After executing these lines, I gained the following insights:
        
        - Data types of the columns
        - Name and number of columns 
        - Summary statistics for each column 

### Cleaning and Processing

1. Now that I have viewed the datasets and their content, I noticed a few areas that need cleaning for a more streamlined process. I went ahead and cleaned the column names, which were not as uniform as I expected them to be. I corrected this issue and reviewed the results 
        
                  
         daily_activity <- clean_names(daily_activity)
         daily_sleep <- clean_names(daily_sleep)
         weight_log <- clean_names(weight_log)
         
         #Double-check the columns are uniformed 
         colnames(daily_activity) 
         colnames(daily_sleep)
         colnames(weight_log)
         
         > colnames(daily_activity) 
          [1] "id"                         "activity_date"              "total_steps"               
          [4] "total_distance"             "tracker_distance"           "logged_activities_distance"
          [7] "very_active_distance"       "moderately_active_distance" "light_active_distance"     
         [10] "sedentary_active_distance"  "very_active_minutes"        "fairly_active_minutes"     
         [13] "lightly_active_minutes"     "sedentary_minutes"          "calories"                  
         > colnames(daily_sleep)
         [1] "id"                   "sleep_day"            "total_sleep_records"  "total_minutes_asleep"
         [5] "total_time_in_bed"   
         > colnames(weight_log)
         [1] "id"               "date"             "weight_kg"        "weight_pounds"    "fat"             
         [6] "bmi"              "is_manual_report" "log_id"           

  2. Rename the date columns in activity_date, sleep_day, and date to the same title to merge later on.
       
         #Rename the 'date' columns in each set to activity date for uniformity
         
         daily_activity <- daily_activity %>%  rename(activity_date = activity_date)
         daily_sleep <- daily_sleep %>% rename(activity_date = sleep_day)
         weight_log <- weight_log %>% rename(activity_date = date)

3. Since the ‘’id’’ columns are identifiers in this data set, I do not want them to be seen as a numerical value. Next, I want to convert the ID field to a character data type across all data sets and check the structure after this change. 
      
          #Convert ID field to a character data type
          daily_activity <- daily_activity %>%  
            mutate_at(vars("id"), as.character)
          
          daily_sleep <- daily_sleep %>%
            mutate_at(vars("id"), as.character)
          
          weight_log <- weight_log %>%
            mutate_at(vars("id"), as.character)
          
          #check the structure 
          str(daily_activity)
          str(daily_sleep)
          str(weight_log)

            > str(daily_activity)
         tibble [940 × 15] (S3: tbl_df/tbl/data.frame)
          $ id                        : chr [1:940] "1503960366" "1503960366" "1503960366" "1503960366" ...
          $ activity_date             : chr [1:940] "4/12/2016" "4/13/2016" "4/14/2016" "4/15/2016" ...
          $ total_steps               : num [1:940] 13162 10735 10460 9762 12669 ...
          $ total_distance            : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
          $ tracker_distance          : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
          $ logged_activities_distance: num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
          $ very_active_distance      : num [1:940] 1.88 1.57 2.44 2.14 2.71 ...
          $ moderately_active_distance: num [1:940] 0.55 0.69 0.4 1.26 0.41 ...
          $ light_active_distance     : num [1:940] 6.06 4.71 3.91 2.83 5.04 ...
          $ sedentary_active_distance : num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
          $ very_active_minutes       : num [1:940] 25 21 30 29 36 38 42 50 28 19 ...
          $ fairly_active_minutes     : num [1:940] 13 19 11 34 10 20 16 31 12 8 ...
          $ lightly_active_minutes    : num [1:940] 328 217 181 209 221 164 233 264 205 211 ...
          $ sedentary_minutes         : num [1:940] 728 776 1218 726 773 ...
          $ calories                  : num [1:940] 1985 1797 1776 1745 1863 ...
         > str(daily_sleep)
         tibble [413 × 5] (S3: tbl_df/tbl/data.frame)
          $ id                  : chr [1:413] "1503960366" "1503960366" "1503960366" "1503960366" ...
          $ activity_date       : chr [1:413] "4/12/2016 12:00:00 AM" "4/13/2016 12:00:00 AM" "4/15/2016 12:00:00 AM" "4/16/2016 12:00:00 AM" ...
          $ total_sleep_records : num [1:413] 1 2 1 2 1 1 1 1 1 1 ...
          $ total_minutes_asleep: num [1:413] 327 384 412 340 700 304 360 325 361 430 ...
          $ total_time_in_bed   : num [1:413] 346 407 442 367 712 320 377 364 384 449 ...
         > str(weight_log)
         tibble [67 × 8] (S3: tbl_df/tbl/data.frame)
          $ id              : chr [1:67] "1503960366" "1503960366" "1927972279" "2873212765" ...
          $ activity_date   : chr [1:67] "5/2/2016 11:59:59 PM" "5/3/2016 11:59:59 PM" "4/13/2016 1:08:52 AM" "4/21/2016 11:59:59 PM" ...
          $ weight_kg       : num [1:67] 52.6 52.6 133.5 56.7 57.3 ...
          $ weight_pounds   : num [1:67] 116 116 294 125 126 ...
          $ fat             : num [1:67] 22 NA NA NA NA 25 NA NA NA NA ...
          $ bmi             : num [1:67] 22.6 22.6 47.5 21.5 21.7 ...
          $ is_manual_report: logi [1:67] TRUE TRUE FALSE TRUE TRUE TRUE ...
          $ log_id          : num [1:67] 1.46e+12 1.46e+12 1.46e+12 1.46e+12 1.46e+12 ...

4. Remove the hour/minutes/seconds from the activity_date column in 'daily_sleep' data set and 'weight_log' data set since it will not be needed for this analysis.

          #Convert activity_date to reflect only date and not time in daily_sleep and weight_log
          daily_sleep$activity_date <- sub(" .*", "", daily_sleep$activity_date)
          
          weight_log$activity_date <- sub(" .*", "", weight_log$activity_date)

5. Now that the column activity_date is uniform in each dataset - let's go ahead and convert the format from character to Date format. Let's also doublecheck to make sure it converted correctly.

          library(lubridate)                              
          daily_activity$activity_date <- mdy(daily_activity$activity_date)
          daily_sleep$activity_date <- mdy(daily_sleep$activity_date)
          weight_log$activity_date <- mdy(weight_log$activity_date)
          
          #Check our columns to ensure the changes were made correctly
          
          head(daily_activity)
          head(daily_sleep)
          head(weight_log) 

6. In the initial introduction, I was informed this data included only 30 surveys. To confirm this information is correct, I want to count the number of distinct  IDs

          #Count distinct ID's
          
          n_distinct(daily_activity$id)
          n_distinct(daily_sleep$id)
          n_distinct(weight_log$id)


          > n_distinct(daily_activity$id)
          [1] 33
          > n_distinct(daily_sleep$id)
          [1] 24
          > n_distinct(weight_log$id)
          [1] 8

So far in this analysis, we have determined the following information for our data summary:

- There are 940 records for the daily_activity data, 413 records for daily_sleep, and 67 records in weight_log
- There were no null values in each data set
- The activity_date column was in character format and converted to date during our cleaning process
- We have converted the ID column to a character format so they are not seen as numerical
- There are 33 IDs in the daily_activity data set which means we did not receive 30 surveys as mentioned but 33
    - We further determine there are more IDs in the daily activity data set compared to the daily_sleep data set which has 24 and the weight_log data set which has 8. 


7. Now, let's identify how many duplicates we have after receiving this information.

           #Identify how many duplicates we have 
           
           sum(duplicated(daily_activity))
           sum(duplicated(daily_sleep))
           sum(duplicated(weight_log))

              > sum(duplicated(daily_activity))
           [1] 0
           > sum(duplicated(daily_sleep))
           [1] 3
           > sum(duplicated(weight_log))
           [1] 0

Review the duplicate entries to confirm if they are duplicates with the same entry in every row. If they are, we can go ahead and remove the duplicate entries and confirm this was done properly. 


           #Review the duplicate entries to confirm if they are actual duplicates with the same entry in every row
           
           library(janitor)
           get_dupes(daily_sleep)

              > get_dupes(daily_sleep)
              No variable names specified - using all columns.
              
              # A tibble: 6 × 6
                id         activity_date total_sleep_records total_minutes_asleep total_time_in_bed dupe_count
                <chr>      <date>                      <dbl>                <dbl>             <dbl>      <int>
              1 4388161847 2016-05-05                      1                  471               495          2
              2 4388161847 2016-05-05                      1                  471               495          2
              3 4702921684 2016-05-07                      1                  520               543          2
              4 4702921684 2016-05-07                      1                  520               543          2
              5 8378563200 2016-04-25                      1                  388               402          2
              6 8378563200 2016-04-25                      1                  388               402          2


           #Remove all confirmed duplicates from daily_sleep
           daily_sleep <- distinct(daily_sleep, .keep_all = TRUE)
           
           #Double-check the duplicates have been removed 
           sum(duplicated(daily_sleep))
                       
            > sum(duplicated(daily_sleep))
            [1] 0

**Conclusion to processing**

Here, I have identified duplicates in each data set. Determining the daily_sleep data set only contained the duplicates, so I used the getdupes() function to determine if they were purely identical. We discovered there were 3 identical duplicates in this data set and removed them using the distinct function.

This concludes the documentation of the cleaning and manipulation portion of our data. Now, we can now move on to the analysis phase! 


## Analyze

Based on Bella Beat’s request, we are going to examine the daily activities, weight, and sleep of the users to see if we can find any trends and correlations. We will then use all of the data collected and analyzed to determine proper marketing strategy suggestions that could help improve the company's marketing.

To start off, I want to combine all of the data frames together to get a better summary of the information. 
         
         #Combine data frames by id and activity_date
         
         combined_data <- daily_sleep %>%
           right_join(daily_activity, by = c("id", "activity_date")) %>%
           left_join(weight_log, by = c("id", "activity_date"))
         
         #View the new data frame
         View(combined_data)

Next, I want to add a weekday column to determine the correlation between the day of the week and various columns. 

         
         #Create a new column for the weekday based on activity_date and view the new column
         combined_data$weekday <- wday(combined_data$activity_date, label = TRUE)
         
         head(combined_data$weekday)
         
         view(combined_data)

Now we need to summarize the data and discover any insights about the data.

         
         #Gather statistic summaries to gain insight
         
         combined_data %>%
           select(total_steps, total_distance, very_active_minutes, fairly_active_minutes, lightly_active_minutes, sedentary_minutes)%>%
           summary()
         
         
          total_steps    total_distance   very_active_minutes fairly_active_minutes
          Min.   :    0   Min.   : 0.000   Min.   :  0.00      Min.   :  0.00       
          1st Qu.: 3790   1st Qu.: 2.620   1st Qu.:  0.00      1st Qu.:  0.00       
          Median : 7406   Median : 5.245   Median :  4.00      Median :  6.00       
          Mean   : 7638   Mean   : 5.490   Mean   : 21.16      Mean   : 13.56       
          3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.: 32.00      3rd Qu.: 19.00       
          Max.   :36019   Max.   :28.030   Max.   :210.00      Max.   :143.00       
          lightly_active_minutes sedentary_minutes
          Min.   :  0.0          Min.   :   0.0   
          1st Qu.:127.0          1st Qu.: 729.8   
          Median :199.0          Median :1057.5   
          Mean   :192.8          Mean   : 991.2   
          3rd Qu.:264.0          3rd Qu.:1229.5   
          Max.   :518.0          Max.   :1440.0  
          
**Insights:**

- According to the data pulled, The average total steps are 7,638 steps. CDC recommended daily steps should be around 10,000 steps a day. However, NIH and CDC have recently provided research showing that more than 8,000 steps can also provide improvement in health when meeting daily steps.  (CDC and NIH)

- The average very active minutes are 21.16 minutes while the sedentary minutes are an average of 991.20 minutes or 16.52 hours. Users' average for light activities is 192.8 minutes or 3.2 hours. This means they are spending most of their time doing light activities compared to the other categories. 
     - It is advised that spending more than 7-10 hours sedentary is bad for your health. 
     - It is recommended to do an average of 30 minutes a day of moderate activity and 75 minutes a week of vigorous activity. (CDC 2)



            combined_data %>%
              select( total_minutes_asleep,total_time_in_bed )%>%
              summary()

              total_minutes_asleep total_time_in_bed
               Min.   : 58.0        Min.   : 61.0    
               1st Qu.:361.0        1st Qu.:403.8    
               Median :432.5        Median :463.0    
               Mean   :419.2        Mean   :458.5    
               3rd Qu.:490.0        3rd Qu.:526.0    
               Max.   :796.0        Max.   :961.0    
               NA's   :530          NA's   :530

 
- The average user sleeps for a total of 419.2 minutes or 6.9 hours which is just a little under the limit of the recommended 7-10 hours of sleep. (NIH, sleep)
- The user spends an average of 458.5 minutes or 7.5 hours in bed. This would mean there is a difference of 39 minutes spent on average where the user spends trying to fall asleep. 
       
          
         combined_data %>%
           select( calories, fat, weight_pounds, bmi )%>%
           summary()
         
               calories         fat        weight_pounds        bmi       
          Min.   :   0   Min.   :22.00   Min.   :116.0   Min.   :21.45  
          1st Qu.:1828   1st Qu.:22.75   1st Qu.:135.4   1st Qu.:23.96  
          Median :2134   Median :23.50   Median :137.8   Median :24.39  
          Mean   :2304   Mean   :23.50   Mean   :158.8   Mean   :25.19  
          3rd Qu.:2793   3rd Qu.:24.25   3rd Qu.:187.5   3rd Qu.:25.56  
          Max.   :4900   Max.   :25.00   Max.   :294.3   Max.   :47.54  
                         NA's   :938     NA's   :873     NA's   :873  

- The average user weighs 158.8 pounds and has a BMI of 25.19, which is slightly higher than the average.


## Share

Now that I have completed the analysis, it's time to create the data visualizations.


            #Gather the sum of the very active minutes
            sum(combined_data$very_active_minutes)
            sum(combined_data$fairly_active_minutes)
            sum(combined_data$lightly_active_minutes)
            sum(combined_data$sedentary_minutes)
            
            #Visual for total steps by the day of the week. 
            
            ggplot(combined_data, aes(x = weekday, y = total_steps)) +
              geom_bar(stat = "identity", fill = "#E69F00") +
              labs(title = "Steps Logged by Day in the Week", x = "Day of the Week", y = "Total Steps")



