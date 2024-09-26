# Cyclistic Bike-Share: How does a bike-share navigate speedy success?

In this case study, a data analyst works for a fictional company, Cyclistic, along with some key team members. The steps of data analysis process are followed to answer business questions: Ask, Prepare, Process, Analyze, Share and Act. The case study scenario is presented hereunder.

## Data Analyst: James Andres Ruiz Vasquez
A junior data analyst working on the marketing analyst team at Cyclistic, a bike-share company in Chicago.

## Client: Cyclistic
Cyclistic is a bike-share program with more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by offering reclining bikes, hand tricycles, and cargo bikes. It makes bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use bikes to commute to work each day.

## Purpose
The marketing director believes the company’s future success depends on maximizing the number of annual memberships. Therefore, the team wants to understand how casual riders and annual members use Cyclistic bikes differently. The goal is to design marketing strategies to convert casual riders into yearly members, as they are more profitable than casual riders. The final deliverable will give recommendations to the Cyclistic executive team on how annual members and casual riders use Cyclistic differently, to improve profitability.

## Scope / Major Project Activities

| **Activity** | **Description** |
| --- | --- |
| Define business task | Define a clear statement of the business task |
| Retrieve data sources| Get the data and describe all data sources used |
| Document data cleaning | Document all cleaning and manipulation of data|
| Data Analysis | Analyze data, summarize it, and support it with visualizations of key findings |
| Deliver final report | Give a Final report to the Executive team with top recommendations based on the analysis |

## This project does not include

* This project does not implement any solutions or recommendations as “Cyclistic” is a fictional company.
* No data older than the previous 12 months of Cyclistic trip data is analyzed.
* The project will not connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area, or if they have purchased multiple single passes due to data-privacy policy.

## Deliverables

| **Deliverable** | **Description/Details** |
| --- | --- |
| Business task | A clear statement of the business task |
| Data sources | A description of all data sources used |
| Documentation | Documentation of any cleaning or manipulation of data |
| Summary | A summary of the analysis made |
| Visualizations | Supporting visualizations and key findings |
| Final report | Top three recommendations based on the analysis made |

## Schedule Overview / Major Milestones

| **Milestone** | **Expected Completion Date** | **Description/Details** |
| --- | --- | --- |
| Business task | 15/09/2024 | Statement of the business task completed |
| Data Review | 16/09/2024 | Review of all data sources completed |
| Data Cleaning | 18/09/2024 | Data manipulation and documentation of the data cleaning process completed |
| Data Analysis | 20/09/2024 | Data analysis completed|
| Visualizations | 21/09/2024 | Visualizations and key findings completed |
| Final report | 22/09/2024 | Final report with recommendations completed |

### Estimated date for completion:  September 23rd, 2024

## Business Task Statement
Analyze Cyclist's historical trip data to understand how annual members and casual riders use Cyclistic bikes differently, to increase profitability.

## Key stakeholders

* Lily Moreno: Director of marketing, responsible for the development of campaigns and initiatives to promote the bike-share program.
* Cyclistic executive team: the detailed-oriented executive team responsible for deciding whether to approve the recommended marketing program.

##  Data Sources

All datasets were retrieved from [divvy-tripdata](https://divvy-tripdata.s3.amazonaws.com/index.html). The datasets have been made available by Motivate International Inc. under this [license](https://divvybikes.com/data-license-agreement). The last 12 months of data were downloaded and stored in a Google Drive folder, where each file is named by its month and year for easy reference. 

Based on the ROCCC criteria the datasets meet the requirements. The datasets are from a Reliable source, Original, Comprehensive, Current, and Cited, as the license and terms of the agreement say: 
“Lyft Bikes and Scooters, LLC (“Bikeshare”) operates the City of Chicago’s (“City”) Divvy bicycle sharing service. Bikeshare and the City are committed to supporting bicycling as an alternative transportation option. As part of that commitment, the City permits Bikeshare to make certain Divvy system data owned by the City (“Data”) available to the public, subject to the terms and conditions of this License Agreement (“Agreement”).” ([Lyft Inc., 2024](https://divvybikes.com/data-license-agreement)).

In terms of licensing, privacy, security, and accessibility, the datasets are fully licensed, it is public data from a secure source and does not include personally identifiable information, following the privacy policy. On the other hand, data integrity was verified using BigQuery. For this, each zip file was downloaded to the local drive and stored in a Google Drive folder, unzipped, and then uploaded to Google Cloud Storage, as some of the CSV files were more than 100 MB. Then, a dataset named ‘Cyclistic’ was added to the BigQuery Project, and a table was created for each month, starting from September 2023 until August 2024. For each table, the schema was checked for its integrity and consistency across files, with the following structure:

* ride_id: string type
* rideable_type: string type
* started_at: timestamp type
* ended_at: timestamp type
* start_station_name: string type
* start_station_id: string type
* end_station_name: string type
* end_station_id: string type
* start_lat: float type
* start_lng: float type
* end_lat: float type
* end_lng: float type
* member_casual: string type

It is worth mentioning that column name ‘member_casual’ is a boolean value: member or casual. This will help understand how different rider types use Cyclistic bikes differently with time. Nevertheless, there are a lot of missing values, expressed as Null, in the start_station_name, start_station_id, end_station_name, and end_station_id fields. This incomplete data could interfere with the analysis if not removed.

## Data Manipulation Process

For cleaning and manipulation purposes, the BigQuery tool will be used as it allows huge amounts of data to be manipulated in a very short time. To ensure data integrity, first, all tables were merged using the following SQL code:

```ruby
SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2023-09`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2023-10`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2023-11`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2023-12`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2024-01`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2024-02`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2024-03`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2024-04`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2024-05`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2024-06`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2024-07`

UNION DISTINCT

SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2024-08`
```

To avoid duplicates, the DISTINCT function was used after each UNION. Then, the resulting table was saved in a new table named ‘2023-09_to_2024_08’. Next, for each data field, missing values were checked and removed from the table. In this case, missing values were only presented in the start_station_name, start_station_id, end_station_name, and end_station_id columns, as checked in the ‘Table Explorer’ tab. For removing missing values, the next SQL code was executed:

```ruby
SELECT *
FROM `mystical-ally-435802-k0.Cyclistic.2023-09_to_2024_08`
WHERE start_station_name IS NOT NULL
AND start_station_id IS NOT NULL
AND end_station_name IS NOT NULL
AND end_station_id IS NOT NULL
```

The resulting table was saved under the name ‘2023-09_to_2024-08’. Then, the first merging table was deleted to avoid confusion. At this point, the data is clean from NULLs, duplicates, and irregularities. The following step is to create a column called ‘ride_length’, where the length of each ride will be calculated in seconds and a column named ‘day_of_week’, to calculate the day of the week that each ride started. The next SQL command was written for this purpose:

```ruby
SELECT *
    TIMESTAMP_DIFF(ended_at, started_at, SECOND) AS ride_length,
    EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week,
EXTRACT(MONTH FROM started_at) AS month,
EXTRACT(YEAR FROM started_at) AS year
FROM `mystical-ally-435802-k0.Cyclistic.2023-09_to_2024-08`
```

The ‘TIMESTAMP_DIFF’ statement calculates the time difference between the ending and starting time and expresses it in minutes. In conjunction with ‘DAYOFWEEK’, the' EXTRACT' statement computes the day of the week where 1 = Sunday and 7 = Saturday. Besides, the ‘ride_id’, ‘rideable_type’, ‘started_at’, ‘ended_at’, and ‘member_casual’, ‘start_station_name’ and ‘end_station_name’ fields were kept, as they are relevant for the analysis process. The month and the year were also extracted from the ‘started_at’ field. While ordering the data in ascending order, it was noted that there were values where ‘ride_length’ was negative. As time cannot be negative, then those values were excluded from the analysis using the following query:

```ruby
SELECT *
FROM (
    SELECT ride_id,
        rideable_type,
        started_at,
        ended_at,
        member_casual,
        start_station_name,
        end_station_name,
        TIMESTAMP_DIFF(ended_at, started_at, SECOND) AS ride_length,
        EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week,
        EXTRACT(MONTH FROM started_at) AS month,
EXTRACT(YEAR FROM started_at) AS year
    FROM `mystical-ally-435802-k0.Cyclistic.2023-09_to_2024-08`
) AS rides
WHERE ride_length >= 0
ORDER BY ride_length;
```

The resulting table was saved as ‘cleaned_data’ inside the ’Cyclistic’ dataset. Once done, the next step is to analyze the dataset and draw insights from it. 

## Data Analysis Summary

For the analysis process, first, the mean and maximum values were calculated for the ride length using an SQL query.  As the ride length was in seconds, it was converted to minutes by dividing it by 60. Then, the mode of the day of the week where most riders use Cyclistic bike-share was calculated, as well as the total amount of riders. The following code was run in BigQuery:

```ruby
SELECT
  AVG(ride_length)/60 AS avg_ride_length_minutes,
  MAX(ride_length)/60 AS max_ride_length_minutes,
  APPROX_TOP_COUNT(day_of_week,1) AS mode_day_of_week,
  COUNT(ride_id) AS total_riders
FROM
  `mystical-ally-435802-k0.Cyclistic.cleaned_data`
```

This code gave the following [table](https://github.com/jaruizv/Google_Data_Analytics_Capstone/blob/main/General_Stats.csv) with results. Then, in a second query, the average ride length, the number of users, the number of electric bike riders, and the number of classic bike riders were calculated and resumed by the member or casual status. The next code was executed for this:

```ruby
SELECT
  member_casual,
  AVG(ride_length)/60 AS avg_ride_length,
  COUNT(member_casual) AS number_of_users,
     COUNT(CASE WHEN rideable_type = 'electric_bike' THEN ride_id END) AS Electric_bike_riders,
     COUNT(CASE WHEN rideable_type = 'classic_bike' THEN ride_id END) AS Classic_bike_riders,
FROM
  `mystical-ally-435802-k0.Cyclistic.cleaned_data`
GROUP BY member_casual;
```

Which resulted in the following [table](https://github.com/jaruizv/Google_Data_Analytics_Capstone/blob/main/Stats_by_member_casual.csv). For the upcoming part of the analysis, the goal was to retrieve data (average ride length for casual and member status, number of casual and member users, and rideable type) based on the day of the week, to see trends and differences between casual and member riders. For this purpose, the next SQL code was run:

```ruby
SELECT
  day_of_week,
  AVG(ride_length)/60 AS Avg_ride_length,
  AVG(CASE WHEN member_casual = 'casual' THEN ride_length END)/60 AS Avg_ride_length_Casual,
  AVG(CASE WHEN member_casual = 'member' THEN ride_length END)/60 AS Avg_ride_length_Member,
  COUNT(CASE WHEN member_casual = 'casual' THEN ride_id END) AS Casual_riders,
  COUNT(CASE WHEN member_casual = 'member' THEN ride_id END) AS Member_riders,
  COUNT(CASE WHEN rideable_type = 'electric_bike' THEN ride_id END) AS Electric_bike_riders,
  COUNT(CASE WHEN rideable_type = 'classic_bike' THEN ride_id END) AS Classic_bike_riders,
  COUNT(ride_id) AS Total_riders
FROM
  `mystical-ally-435802-k0.Cyclistic.cleaned_data`
GROUP BY day_of_week
ORDER BY day_of_week;
```

This resulted in the following [pivot table](https://github.com/jaruizv/Google_Data_Analytics_Capstone/blob/main/Stats_by_day_of_week.csv) based on the day of the week. This same analysis can be applied to the months and years to see how different seasons affect the use of the bike-share service. The same data was extracted from the latter, and grouped by the ‘month_year’ field as shown:

```ruby
SELECT
  CONCAT(month, "/", year) AS month_year,
  AVG(ride_length)/60 AS Avg_ride_length,
  AVG(CASE WHEN member_casual = 'casual' THEN ride_length END)/60 AS Avg_ride_length_Casual,
  AVG(CASE WHEN member_casual = 'member' THEN ride_length END)/60 AS Avg_ride_length_Member,
  COUNT(CASE WHEN member_casual = 'casual' THEN ride_id END) AS Casual_riders,
  COUNT(CASE WHEN member_casual = 'member' THEN ride_id END) AS Member_riders,
  COUNT(CASE WHEN rideable_type = 'electric_bike' THEN ride_id END) AS Electric_bike_riders,
  COUNT(CASE WHEN rideable_type = 'classic_bike' THEN ride_id END) AS Classic_bike_riders,
  COUNT(ride_id) AS Total_riders
FROM
  `mystical-ally-435802-k0.Cyclistic.cleaned_data`
GROUP BY month_year;
```

This code resulted in the following pivot [table](https://github.com/jaruizv/Google_Data_Analytics_Capstone/blob/main/Stats_by_start_station_name.csv). Last but not least, it is possible to get insights into how users' statuses differ by looking at where members and casual users start their rides. Knowing this allows Cyclistic to focus on the marketing strategies where most casual users start their ride. The next code was developed:

```ruby
SELECT
  start_station_name,
  AVG(ride_length)/60 AS Avg_ride_length,
  AVG(CASE WHEN member_casual = 'casual' THEN ride_length END)/60 AS Avg_ride_length_Casual,
  AVG(CASE WHEN member_casual = 'member' THEN ride_length END)/60 AS Avg_ride_length_Member,
  COUNT(CASE WHEN member_casual = 'casual' THEN ride_id END) AS Casual_riders,
  COUNT(CASE WHEN member_casual = 'member' THEN ride_id END) AS Member_riders,
  COUNT(CASE WHEN rideable_type = 'electric_bike' THEN ride_id END) AS Electric_bike_riders,
  COUNT(CASE WHEN rideable_type = 'classic_bike' THEN ride_id END) AS Classic_bike_riders,
  COUNT(ride_id) AS Total_riders
FROM
  `mystical-ally-435802-k0.Cyclistic.cleaned_data`
  GROUP BY start_station_name
  ORDER BY Casual_riders DESC;
```

As a result, the next [pivot table](https://github.com/jaruizv/Google_Data_Analytics_Capstone/blob/main/Stats_by_month_year.csv) was obtained.

## Visualizations

Based on the analysis made in Big Query, the next step was to transform the insights into visualizations. For this, Tableau Public's stories are a great tool to share the findings. The visualizations can be seen in the following [link](https://public.tableau.com/shared/7XP7N73ZQ?:display_count=n&:origin=viz_share_link), or in this [pdf](https://github.com/jaruizv/Google_Data_Analytics_Capstone/blob/main/Visualizations.pdf).

## Conclusions and recommendations

By adhering to the six phases of data analysis: Ask, Prepare, Process, Analyze, Share and Act, significant insights were discovered in the data under review. Notable distinctions were observed in the usage patterns of bikes by members and casual riders in their daily activities. As revealed in the sharing phase, casual riders tend to have fewer but longer rides, especially on weekends, suggesting they primarily use bikes for leisure purposes. In contrast, members appear to use bikes for work and other productive activities predominantly during weekdays, with shorter rides. These findings could aid the team in marketing the bikes to casual riders by highlighting the benefits of programs such as ride-to-work schemes.

Therefore, three key recommendations can be made:

1. **Targeted Advertising for Casual Riders**: Create advertisements aimed at casual riders, promoting the benefits of using bikes for weekday activities such as commuting to work, running errands, or shopping.

2. **Highlight Leisure vs. Productive Use**: Leverage the insight that casual riders primarily use bikes for leisure on weekends, while members use them for work-related activities during the week. Tailor messaging to show casual riders the potential for weekday use.

3. **Collect Additional User Data**: To validate the current findings, gather direct feedback from users on whether they use bikes for work or leisure. This additional data would provide more precise insights and confirm the trends identified.





