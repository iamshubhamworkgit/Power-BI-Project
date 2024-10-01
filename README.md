


# HealthCare Pateint Waiting List - Dashbord

In healthcare systems, efficient patient management is crucial for providing timely care and improving overall patient satisfaction. Long waiting times can lead to negative patient experiences, delayed treatments, and potential health complications.
#### Dashboard Link :https://app.powerbi.com/view?r=eyJrIjoiNGY3MmJjYzgtM2MwMC00ZTgzLWE1ZjMtMjNiNmI1Mjc3YjYxIiwidCI6IjI0MGMzZGNiLTE5NjgtNDFkMS05ZDliLWVlNWRjNzFlZTc1ZCJ9

        - Inpatient = A patient who stays in hospital more than a day while undergoing treatment.

        - Outpatient = A patient who gets medical treatment without being admittied.

        - Day Case = A patient who stays in hospital only for a single day while undergoing treatment.


## Problem Statement 
The healthcare facility is experiencing challenges in managing patient waiting lists due to the following issues:

* Lack of Visibility: There is insufficient visibility into the current status of patient waiting lists, making it difficult to identify bottlenecks and assess the efficiency of scheduling processes.

* Inefficient Resource Allocation: Without a clear understanding of waiting times, resource allocation (staffing, room availability, etc.) is often reactive rather than proactive.

* Delayed Response to Trends: The ability to identify trends over time is limited, hindering the facility's capacity to implement timely interventions.

## Business Objective
### Project Goals
 To develop a comprehensive Power BI dashboard to visualize patient waiting lists, enabling stakeholders to:

* Monitor real-time patient waiting list.
* Analyze historical monthly trend of waiting list in Inpatient and Outpatient categories.
* Get detailed specialty and age profile analysis.
* Enhance decision-making for resource allocation and operational efficiency.

By addressing these challenges, the dashboard aims to provide actionable insights that can lead to more efficient patient management and better healthcare outcomes.

### Data Scope
 2018 -2021

### Metrics Required
* Average & Median Waiting List
* Current Total Wait List

### Views Required

* Summary Page
* Detailed Page for Granular Anlysis


## Steps followed 

###  Step 1 : Requirment Gathering 
- Understanding business objective
- High level data study
### Step 2 : Data Collection
- We used (Folder data connector) for data collection.
- Loaded Inpatient folder with 4 Excel files.
- Loaded Outpatient folder with 4 Excel files
### Step 3 : Data Transformation & Modeling
- Changed data type for several coloumns according to their values.
- Renamed column "Specialty" to "Specialty_name" in Outpatient table to make the column names similar in both the tables.
- Created a missing "Case_type" column in Outpatient table.
- Created new table by appending "Inpatient" & "Outpatient" table.
- Renamed the new appending table to "All Data".
- Hide the Inpatient & Outpatient table in report view because we do not need them anymore for dashbord creation.

### Step 4 : Data Visualizations Blueprint
![Screenshot 2024-09-28 201957](https://github.com/user-attachments/assets/0e5a73f7-2a04-4a2b-98d2-725920f3fef8)

### Step 5 : Dashboard Design & Layout
#### Page 1(Summary)
- Created a dynamic latest month waitlist measure which can update every month.
        
         Latest month wait list = CALCULATE(SUM('All Data'[Total]),'All Data'[Archive_Date]= MAX('All Data'[Archive_Date]))

 - Created a dynamic previous year's latest month waitlist measure:
 
        PY Latest Month Wait List = CALCULATE(SUM('All Data'[Total]),'All Data'[Archive_Date]= EDATE(MAX('All Data'[Archive_Date]),-12)) 

 - Created a "Calc_table" with single column "Calc_method" with text values Average & Median.
 - Created a slicer with "Calc_method".
 - Created two measures with Average and Median Waitlist values.

        Avg_WL = AVERAGE('All Data'[Total])

        Median_WL = MEDIAN('All Data'[Total])

- Created a measure with "Switch()" function so that the "Calc_method" Slicer can interact and switch between Avg_WL & Median_WL measure.

        Avg/Med Wait List = SWITCH(VALUES('Calc method'[Calculation Method]),"Average",'All Data'[Avg_WL],"Median",'All Data'[Median_WL])

- Created a (Pie chart) of Avg/Med Wait List by "Case_type".
- Created a (Coloumn chart) of Avg/Med Wait List by "Time_Band & Age_Profile".
- Here we found some dupliacte values and spaces and some simliar category of values with different names. 
- So we did some Data Transformation again using "Replace values" to rename the simliar category of values with similar name and "Trim" functions to replace duplicate values and unwanted spaces.
- Created a (Multi-Row Card) of Avg/Med Wait List and "Specialty_name" and applied Top N filter for top 5 Specialty by Avg/Med Wait List.
- Created (Line Chart) with Sum of total "Waitlist" by "Archive_Date & Case_type".
- Applied filter to select Outpatient only and similarly for Inpatient line chart.
- Created (Slicers) with Archive_Date, Case_type and Specialty_name.

#### Page 2 (Detalied View)

- Created (Slicers) with Archive_Date, Case_type, Specialty_name, Time_Band and Age_Profile.
- Created a (Matrix) table with all relevant coloumns for Granular analysis.

### Step 6 : Ineractivity & Navigation
- If selecting "Archive_Date" before 2018, there was no data for that and showing blank space in PY Latest Month WaitList. So the measure was edited so get "0" instead of blank space.

        PY Latest Month Wait List = CALCULATE(SUM('All Data'[Total]),'All Data'[Archive_Date]= EDATE(MAX('All Data'[Archive_Date]),-12))+ 0

- Turned off Interaction of "Case_type" Filter with "Outpatient" & "Inpatient" Line Chart's.
- Added a button's in "Detailed View" page and "Summary" page to directly navigate between each other.
- Craeted a measure to show "Dynamic Title" when switching between Average and Median.

        Dynamic Title = SWITCH(VALUES('Calc method'[Calculation Method]),"Average","Key Indicators- Patient Wait List(Average)",
        "Median","Key Indicators- Patient Wait List(Median)")

### Step 7 : Sharing Dashboard
- Loged in into PowerBI Sevices through Microsoft Developer Account and published my dashboard.

## Summary Page
![Screenshot 2024-09-25 123908](https://github.com/user-attachments/assets/3faf9950-14b6-4cd6-9140-470fada3ac74)

## Detailed View Page
![Screenshot 2024-09-25 124106](https://github.com/user-attachments/assets/763a94d5-6c58-45ff-bd65-d90a50d3ee27)

## Insights

Following inferences can be drawn from the dashboard;

* Latest Month Waitlist = 709K
* Previous Year's Latest Month Waitlist = 640K

        thus, the waitlist number's has been increased by 69K from the last year's latest month.

### Patient Average Waitlist
#### [1] Case Type :
1.1) 10% paitent waitlist is coming from "Inpatient" case type.

1.2) 72% paitent waitlist is coming from "Outpatient" case type.

1.3) 16% paitent waitlist is coming from "Day Case" case type.

        thus, Outpatient case type has a very high contrubution in overall Waitlist, indicating a need for prioritization in these case types.

 #### [2] Time Band VS Age profile :
 2.1) 0-3 months time band has maximum waitlist for 16-64 Age group.

 2.2) 3-6 months time band has maximum waitlist for 16-64 Age group.

 2.3) 6-9 months time band has maximum waitlist for 16-64 Age group.

 2.4) 9-12 months time band has maximum waitlist for 16-64 Age group.

 2.5) 12-15 months time band has maximum waitlist for 16-64 Age group.

 2.6) 15-18 months time band has maximum waitlist for 16-64 Age group.

 2.7) 19+ months time band has maximum waitlist for 16-64 Age group.

        thus, 16-64 Age Group has maximum waitlist in every time band,  indicating a need for prioritization in this demographic.

#### [3] Top 5 Specialty:

3.1) Paediratic Dermatology has max waitlist of 167.89.

3.2) Paediratic ENT has waitlist of 147.55.

3.3) Paed Orthopaedic has waitlist of 114.50.

3.2) Accident & Emergency has waitlist of 111.19.

3.2) Paed Cardiology has waitlist of 101.77.

        thus, these 5 specialty's needs priotization at first.

## HealthCare Pateint Waiting List
Power BI Dashboard presented by Shubham Kumar.

