# Customer Analysis for Cyclists | "Bike-Share"

## 📖 Table of Contents  

<p align="center">
<table>
  <tr>
    <th>📌 Project Overview</th>
    <th>💼 Business Task</th>
    <th>📊 Data Source</th>
    <th>🛠 Tools Used</th>
    <th>🧹 Data Cleaning</th>
  </tr>
  <tr>
    <td align="center"><a href="#project-overview">Project Overview</a></td>
    <td align="center"><a href="#business-task">Business Task</a></td>
    <td align="center"><a href="#data-source">Data Source</a></td>
    <td align="center"><a href="#tools-used">Tools Used</a></td>
    <td align="center"><a href="#data-cleaning-process">Data Cleaning Process</a></td>
  </tr>
  <tr>
    <th>📈 Data Analysis</th>
    <th>📌 Results</th>
    <th>💡 Recommendations</th>
    <th>⚠️ Limitations</th>
    <th>🔗 References</th>
  </tr>
  <tr>
    <td align="center"><a href="#data-analysis">Data Analysis</a></td>
    <td align="center"><a href="#results">Results</a></td>
    <td align="center"><a href="#recommendations">Recommendations</a></td>
    <td align="center"><a href="#limitations">Limitations</a></td>
    <td align="center"><a href="#references">References</a></td>
  </tr>
</table>
</p>

### Project Overview
This data analysis project aims to provide insights about how different customers are using the bike-share service, for the marketing team to create a new marketing strategy. Because the company wants to convert more casual riders into annual paying members, to increase revenue and customer life time value.

### Business Task
```
  1. How do annual members and casual riders use Cyclistic bikes differently?
  2. What trends can be identified from annual members and casual riders trips?
  3. How could the marketing team apply the insights? 
```
### Data Source
Customer data: The primary dataset used for the analysis is in the "202401-divvy-tripdata.csv" - "202412-divvy-tripdata.csv" files, contaning information about each trip made by the customers. The dataset is for each month during 2024, and includes information such as type of bike, trip start & end time and if they are a casual rider (non member) or an annnual paying member. 

### Tools Used
- Excel -> Data Cleaning & Visualizations | [Download](https://downgit.github.io/#/home?url=https://github.com/SimonStenelid/BikeShare_Project/blob/main/Charts.png)
- SQL BigQuery -> Data Analysis
- PowerPoint -> Creating Reports | [Download]()

---

### Data Cleaning Process
- Downloading each csv.file and storing them appropriately
- Joining all the files together using PowerQuery
- Data loading and inspection
- Removing unnecessary fields, keeping only (type of bike, trip start & end time, date, casual rider/member)
- Handling missing values
- Data cleaning (`TRIM`, formatting, sorting)

<tr>
  
### Data analysis

#### Excel
- Created new custom column to calculate each **ride_time** `=[end_time]-[start_time]`
- Created new custom column to extract **day_of_week** from the start time `=Date.DayofWeek`
- Created new custom column to extract **month_of_year** from the start time `Date.MonthName`
- Created a **pivot table** to calculate total rides per type of customer and average ride time
  - **Bar chart** over total rides for each day of the week, by customer type
  - **Bar chart** overr total rides for each month of the year, by customer type
 
#### SQL BigQuery
Query for calculating average ride the for casual riders and annual members, for all of 2024
```sql
SELECT 
  member_casual AS Customer_Type,
  CONCAT(
    LPAD(CAST(FLOOR(AVG(DATETIME_DIFF(ended_at, started_at, SECOND)) / 3600) AS STRING), 2, '0'), ':',
    LPAD(CAST(FLOOR(MOD(CAST(AVG(DATETIME_DIFF(ended_at, started_at, SECOND)) AS INT64), 3600) / 60) AS STRING), 2, '0'), ':',
    LPAD(CAST(MOD(CAST(AVG(DATETIME_DIFF(ended_at, started_at, SECOND)) AS INT64), 60) AS STRING), 2, '0')
  ) AS AVG_Ride_Time
FROM `argon-ace-449717-p6.Bicycle_Data_Project.2024`
GROUP BY 
  member_casual;
```
Query for calculating each trip time/duration
```sql
SELECT 
  started_at,
  ended_at,
  CONCAT(
    LPAD(CAST(FLOOR(DATETIME_DIFF(ended_at, started_at, SECOND) / 3600) AS STRING), 2, '0'), ':',
    LPAD(CAST(FLOOR(MOD(DATETIME_DIFF(ended_at, started_at, SECOND), 3600) / 60) AS STRING), 2, '0'), ':',
    LPAD(CAST(MOD(DATETIME_DIFF(ended_at, started_at, SECOND), 60) AS STRING), 2, '0')
  ) AS trip_time
FROM `argon-ace-449717-p6.Bicycle_Data_Project.2024`;
```
### Results
The analysis results are summarized as follows:
1.  Most bike riders use the service during the summer months, June-September
2.  Casual bikers barely use bike-share during the "non summer months", while annual members use it more year-around
3.  Most popular days for casual riders are during the weekend, and for annual members during the Monday-Wednesday
4.  Average ride time are nearly double of casual riders vs. annual members  

---

### Recommendations
Based on the analysis, these following actions can be recommended: 

- Introduce seasonal membership for casual riders during the summer, for those who do not want to commit to a full-year
- Implement customer-targeted marketing strategy for casual riders during low season
- Test different forms of memberships for casual riders, to convert them into membership customers
- Inform casual riders about the benefits of a memberships, especially during longer ride times

### Limitations
- Because of incomplete data, an analysis on location based differences could not be made
- Analysis is limited to only one year of data
- Data did not include further information about customers, more detailed information could provide with a deeper analysis of how/why riders are using bike-share differently

### References
1. [Google Data Analytics Capstone Project](https://www.coursera.org/professional-certificates/google-data-analytics?action=enroll&utm_medium=sem&utm_source=gg&utm_campaign=b2c_emea_x_multi_ftcof_career-academy_cx_dr_bau_gg_pmax_gc_s1_en_m_hyb_23-12_x&campaignid=20858198824&adgroupid=&device=c&keyword=&matchtype=&network=x&devicemodel=&creativeid=&assetgroupid=6484888893&targetid=&extensionid=&placement=&gad_source=1&gclid=Cj0KCQjwm7q-BhDRARIsACD6-fWi62ZuuK36ImaplieMHf3rDP5hXn23ikGpvrbPVW-Dz8VKwuLglBAaAqKYEALw_wcBhttps://www.coursera.org/professional-certificates/google-data-analytics?action=enroll&utm_medium=sem&utm_source=gg&utm_campaign=b2c_emea_x_multi_ftcof_career-academy_cx_dr_bau_gg_pmax_gc_s1_en_m_hyb_23-12_x&campaignid=20858198824&adgroupid=&device=c&keyword=&matchtype=&network=x&devicemodel=&creativeid=&assetgroupid=6484888893&targetid=&extensionid=&placement=&gad_source=1&gclid=Cj0KCQjwm7q-BhDRARIsACD6-fWi62ZuuK36ImaplieMHf3rDP5hXn23ikGpvrbPVW-Dz8VKwuLglBAaAqKYEALw_wcB) 
