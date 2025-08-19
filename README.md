# Bellabeat Fitness Data Analysis
##### How Can a Wellness Technology Company Play It Smart?
##### By: Sandra John
#

## Scenario
Bellabeat is a successful, high-tech manufacturer of health-related devices for women. Cofounders, Urška Sršen and Sando Mur, believe that thorough and detailed analysis of smart device fitness data can help unlock new growth opportunities for the company. 
Bellabeat currently offers 5 products: 
- Bellabeat app: Tracks health data (Ex. Sleep, Stress, Menstrual Cycles)
- Leaf: Wellness tracker that can be worn as a bracelet, necklace, or clip
- Time: Wellness watch that connects to the Bellabeat app
- Spring: Waterbottle that tracks daily water intake with smart technology
- Bellabeat membership: 24/7 access to personalized guidance on wellness

Although Bellabeat is a small company, it has the potential to become a larger competitor in the fitness device market. As a junior data analyst on the marketing analyst team, I have been given the task to analyze smart device data and gain insight into how consumers utilize such products. These answers will help guide Bellabeat's marketing strategy and support the development of new products. 

#

## 1. Ask
#### *BUSINESS TASK: Analyze non-Bellabeat smart device data to discover potential growth opportunities and tactical marketing strategies for Bellabeat.* ####

Key Stakeholders:
- Urška Srśen: Cofounder and Chief Creative Officer
- Sando Mur: Cofounder and Mathematician; key member of executive team
- Bellabeat marketing analytics team

#

## 2. Prepare
Data Source - Fitbit Fitness Tracker Data: https://www.kaggle.com/datasets/arashnic/fitbit

This dataset has 18 CSV. Thirty Fitbit users consented to the submission of personal tracker data through a survey from Amazon Mechanical Turk between March 2016 - May 2016. 

This data follows ROCCC:
- Reliability: The data was collected through a reliable source, Amazon Mechanical Turk, who used data from 30   individuals who consented to the utilization of their information.
- Original: This information is from first-party data through data straight from the FitBit users
- Comprehensive: All 18 CSV include information such as minute-level output for physical activity, heart rate, and sleep monitoring
- Current: The data is from 2016, so it is not current and information may be outdated
- Cited: Kaggle page, Zenodo DOI

However, there are many limitations to this dataset
- The data is gathered from volunteers, which can create selection bias in the data rather than gathering information from a random sample
- Data may not be the most accurate due to the lack of updates since 2016
- Only 30 user data, according to the central limit theorem a sample size of more than 30 is preferred for accuracy
- Absence of demographics such as gender or age

#

## 3. Process
Transform the data in order to analyze effectively.

After loading packages and adding the CSV files into R, begin the cleaning by fixing the date/time format
```
daily_activity <- daily_activity %>%
+ mutate(ActivityDate = mdy(ActivityDate))
sleep_day <- sleep_day %>%
+ mutate(SleepDay = mdy_hms(SleepDay))
hourly_steps <- hourly_steps %>%
+ mutate(ActivityHour = mdy_hms(ActivityHour))
```

Check for null values
```
sum(is.na(daily_activity))
sum(is.na(sleep_day))
sum(is.na(weight_info))
```
*65 NA were found in weight_info under the Fat column, which is due to manual entry by users. Due to the overwhelming amount of no data, we choose to erase this from any analysis/conclusions*

Check and clean duplicates
```
sum(duplicated(daily_activity))
sum(duplicated(sleep_day))
sum(duplicated(weight_info))
sleep_day <- sleep_day %>% distinct()
```

Looked for validity and ran some code to find that there were 33 user inputs for daily activity, 24 for sleep, and only 8 for weight. 
```
n_distinct(daily_activity$Id) 
n_distinct(sleep_day$Id) 
n_distinct(weight_info$Id)
```
Then, to look further into why this irregularity is occuring, lets look at the manual entries on the weight dataset. Many users submitted their information multiple times which therefore leads to more discrepencies and is a limitation to gaining credible analysis. 
```
weight_info %>%
+     filter(IsManualReport == TRUE) %>%
+     group_by(Id) %>% 
+     summarise("Manual Weight Report"=n()) %>%
+     distinct()
```

#

## 4. Analyze

### Summary
Gather the data and create summary charts to get information such as the min, max, mean, and any outliers. 

```
summary(daily_activity)
summary(sleep_day)
summary(weight_info)
```

The mean weight is 158.8 with a BMI of 25.19. For reference, the [National Heart, Blood, and Lung Institute](https://www.nhlbi.nih.gov/health/heart-healthy-living/healthy-weight#:~:text=Language%20switcher&text=A%20healthy%20weight%20for%20adults,more%20information%20about%20these%20topics.&text=Your%20browser%20can't%20play%20this%20video.&text=An%20error%20occurred.,is%20disabled%20in%20your%20browser.) states that healthy BMI range is typically between 18.5 and 24.9.
```
WeightPounds        BMI       
 Min.   :116.0   Min.   :21.45  
 1st Qu.:135.4   1st Qu.:23.96  
 Median :137.8   Median :24.39  
 Mean   :158.8   Mean   :25.19  
 3rd Qu.:187.5   3rd Qu.:25.56  
 Max.   :294.3   Max.   :47.54  
```

Users have an average of 419.2 minutes asleep, which converts to 6.9 hours, and 458.5 minutes in bed, which is 7.6 hours. According to [Harvard Health](https://www.health.harvard.edu/blog/how-much-sleep-do-you-actually-need-202310302986) adults need at least 7 hours of sleep per night, meaning these participants are pretty well rested!
```
TotalMinutesAsleep TotalTimeInBed 
Min.   : 58.0      Min.   : 61.0  
1st Qu.:361.0      1st Qu.:403.8  
Median :432.5      Median :463.0  
Mean   :419.2      Mean   :458.5  
3rd Qu.:490.0      3rd Qu.:526.0  
Max.   :796.0      Max.   :961.0
```

The daily_activity file tells us lots of useful information. For example, we find that the mean number of steps taken in a day is 7406, which is below the recommended 8,000 - 10,000 recommended steps from the [CDC](https://www.cdc.gov/physical-activity-basics/benefits/index.html). When comparing the minutes that users are very active, fairly active, lightly active, or sedentary, we find that the mean for fairly active is the lowest at 13.56 minutes. Another useful piece of information we find is that the average number of calories burned in a day is 2304. Additional information can be found [here](daily_activity_summary.txt).
```

### Steps v Calories
First, lets look at the daily number of steps taken.

The blue line on the histogram displays 10k, which is the

























 
