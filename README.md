# **Bellabeat Case Study: Smart Device Usage Analysis Using FitBit Data**

                                                     **By Revanth Chinthada** 

## **1\. Ask Phase**

### **Business Task**

* **Objective**: Analyze FitBit smart device usage data to identify trends in how consumers use non-Bellabeat smart devices, apply these insights to one Bellabeat product (e.g., Leaf tracker, Time watch, Spring bottle, or Bellabeat app), and provide high-level recommendations to inform Bellabeat’s marketing strategy.  
* **Key Questions**:  
  * What are some trends in smart device usage?  
  * How could these trends apply to Bellabeat customers?  
  * How could these trends help influence Bellabeat marketing strategy?  
* **Stakeholders**:  
  * Urška Sršen (Cofounder and Chief Creative Officer)  
  * Sando Mur (Cofounder and Executive Team Member)  
  * Bellabeat Marketing Analytics Team

  ## **2\. Prepare Phase**

  ### **Data Source**

* **Dataset**:  
  * **Name**: FitBit Fitness Tracker Data  
  * **Source**: Kaggle ([https://www.kaggle.com/datasets/arashnic/fitbit](https://www.kaggle.com/datasets/arashnic/fitbit)), public domain (CC0), provided by Mobius.  
  * **Description**: Data from 30 Fitbit users, collected in 2016, including daily activity (steps, distance, calories), sleep, heart rate, and weight logs. Organized in 18 CSV files in long format, with key files including:  
    * dailyActivity\_merged.csv: Steps, distance, calories, and activity intensity.  
    * sleepDay\_merged.csv: Sleep duration and time in bed.  
    * hourlySteps\_merged.csv: Hourly step counts.  
  * **Relevance**: Provides metrics on activity, sleep, and heart rate, aligning with Bellabeat’s product features (Leaf tracker’s activity and sleep tracking).  
  * **Limitations**:  
    * Small sample size (30 users), potentially non-representative.  
    * Outdated (2016), may not reflect current smart device trends.  
    * Lacks demographic data (e.g., gender), critical for Bellabeat’s female-focused audience.  
    * Potential bias from self-selected users sharing data publicly.  
    * Incomplete data (e.g., only 8 users in weightLogInfo\_merged.csv).  
  * **ROCCC Evaluation**:  
    * Reliability: Low (small sample size).  
    * Originality: Low (third-party collection via Amazon Mechanical Turk).  
    * Comprehensiveness: Medium (covers activity, sleep, heart rate, but lacks demographics).  
    * Current: Low (2016 data).  
    * Cited: Medium (reputable Kaggle source).  
* **Storage and Organization**:  
  * Data stored in \[e.g., local folder: Bellabeat\_Case\_Study/Data/FitBit\].  
  * File naming conventions: \[e.g., dailyActivity\_merged.csv, sleepDay\_merged.csv\].  
  * Data format: Long format, suitable for R analysis with user IDs and time-based records.  
* **Data Integrity**:  
  * Verified consistency of user IDs and dates across files.  
  * Checked for missing values, duplicates, or format inconsistencies.  
* **Licensing and Privacy**:  
  * Public domain (CC0), anonymized, ensuring ethical use and accessibility.

  ## **3\. Process Phase**

  ### **Data Cleaning and Manipulation**

* **Tools Used**:  
  * R (RStudio) with packages: tidyverse (for data manipulation and visualization), lubridate (for date handling), skimr (for summaries), janitor (for cleaning column names).  
* **Cleaning Steps**:  
  * **Imported Data**:  
    * Loaded key CSV files ( dailyActivity\_merged.csv, sleepDay\_merged.csv, hourlySteps\_merged.csv) using readr::read\_csv().  
  * **Checked for Errors**:  
    * Identified missing values, duplicates, or inconsistent formats (e.g., date strings).  
    * Example: Found 3 rows duplicates in sleepDay\_merged.csv, 

    ![duplicates](https://raw.githubusercontent.com/sairevanth1432/bellabeatcasestudy/main/Screenshot 2025-04-23 101632.png

    * removed using distinct().

    ![][image2]

  * **Transformed Data**:  
    * Converted date columns to consistent formats using lubridate::mdy() or mdy\_hms().

    ![][image3]

    * Merged datasets (e.g., daily activity and sleep) by Id and Date using inner\_join().

    ![][image4]

    * Aggregated data (e.g., daily averages for steps, sleep).  
  * **Verified Cleanliness**:  
    * Used skimr::skim\_without\_charts() to summarize missing values and data types.  
    * Confirmed no duplicate rows or inconsistent IDs.  
* **Documentation**:  
  * R script saved as \[e.g., bellabeat\_data\_cleaning.R\] with comments detailing each step.

  cleaning code-  
    library(tidyverse)

  library(lubridate)

  daily\_activity \<- read\_csv("dailyActivity\_merged.csv") %\>%

    mutate(Date \= mdy(ActivityDate)) %\>%

    remove\_empty()

  sleep\_day \<- read\_csv("sleepDay\_merged.csv") %\>%

    mutate(Date \= mdy\_hms(SleepDay)) %\>%

    distinct()

  merged\_data \<- inner\_join(daily\_activity, sleep\_day, by \= c("Id", "Date"))


* **Issues Encountered**:  
  * \[ Missing sleep data for some users, addressed by filtering complete cases or noting in analysis limitations\].  
  * \[Discrepancy in reported user count (33 vs. 30), verified unique IDs\].

  ## **4\. Analyze Phase**

  ### **Summary of Analysis**

* **Data Organization**:  
  * Aggregated data into daily summaries for steps, calories, sleep, and activity intensity.  
  * Created subsets for specific analyses (e.g., weekday vs. weekend activity).  
* **Key Calculations**:  
  * Average daily steps: \[7,638 steps\].  
  * Average sleep duration: \[419 minutes\].  
  * Correlations: \[Positive correlation between steps and calories burned.  
* **Trends and Relationships**:  
  * **Usage Trends**: \[Users are most active on Saturdays, least active on Sundays; peak activity between 5-7 PM\].  
  * **Behavioral Patterns**: \[Higher step counts correlate with longer sleep duration\].  
  * **Surprises**: \[ Significant sedentary time (average X hours/day), indicating potential for activity prompts\].  
* **Relevance to Business Questions**:  
  * **Trends in Smart Device Usage**: \[Consistent step tracking but underutilized sleep monitoring suggests focus on sleep features\].  
  * **Application to Bellabeat Customers**: \[Assuming trends apply to women, users may benefit from tailored sleep or activity reminders\].  
  * **Marketing Strategy**: \[Promote app notifications for low-step days or sleep deficits to enhance user engagement\].

  **R Code** :  
    library(tidyverse)

  summary\_stats \<- merged\_data %\>%

    summarise(

      avg\_steps \= mean(TotalSteps, na.rm \= TRUE),

      avg\_sleep \= mean(TotalMinutesAsleep, na.rm \= TRUE)

    )

  correlation \<- cor.test(merged\_data$TotalSteps, merged\_data$TotalMinutesAsleep)

  ## **5\. Share Phase**

  ### **Supporting Visualizations and Key Findings**

* **Visualization Tools**: R (ggplot2) and \[ PowerPoint for presentation slides\].

* **Visualizations**:  
  * **Bar Chart**: Average steps by day of the week.  
    * Finding: \[Saturday peak at 9871 steps, Sunday low at 7297 steps\].

    ![][image5]


  * **Scatter Plot**: Steps vs. sleep duration.  
    * Finding: \[ Weak positive correlation (r \= \-0.19), suggesting active users sleep more\].  
        
      ![][image6]  
        
        
  * **Line Chart**: Hourly activity intensity trends.  
    * Finding: \[Peak activity at 5-7 PM, ideal for scheduling app notifications\].

    ![][image7]

  ## **6\. Act Phase**

  ### **High-Level Recommendations**

* **Recommendation 1**: Enhance Bellabeat app with personalized activity notifications for low-step days, based on observed sedentary time  
* **Recommendation 2**: Market Leaf tracker’s sleep tracking feature to address underutilization, emphasizing benefits for women’s health  
* **Recommendation 3**: Run targeted digital campaigns on social media (e.g., Instagram) during peak activity hours (5-7 PM) to engage users  
* **Next Steps**:  
  * Collect Bellabeat-specific user data to validate trends, especially for female demographics.  
  * Test proposed app features in a pilot program and measure user engagement.  
* **Additional Data Needs**:  
  * \[ Recent FitBit-like data with gender demographics\].  
  * \[Bellabeat app usage data to compare with FitBit trends\].

  ## **7\. Appendices**

* **R Scripts**: \[e.g., bellabeat\_data\_cleaning.R, bellabeat\_analysis.R\].  
* **Data Source**: Kaggle FitBit Fitness Tracker Data ([https://www.kaggle.com/datasets/arashnic/fitbit](https://www.kaggle.com/datasets/arashnic/fitbit), CC0).  
* **Visualizations**: Exported PNG files of all plots.  
* **References**: \[ Kaggle dataset citation\].
