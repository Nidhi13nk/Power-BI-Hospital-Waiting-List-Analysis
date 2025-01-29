# Power-BI-Hospital-Waiting-List-Analysis
## 1. Understanding the Problem
Hospitals have long waiting lists for different types of patients. Management wants to analyze these waitlists to improve efficiency.

The key goals of this project are:
* Track the current number of patients on the waitlist.
* Analyze monthly trends in Inpatient and Outpatient categories.
* Understand the waitlist for different medical specialties and age groups.
* Calculate the average and median wait times.
* Find the total number of patients waiting.

## 2. Data Collection
I received healthcare data which includes 8 CSV files (4 for Inpatients, 4 for Outpatients) covering data from January 2018 to March 2021.

Types of Patients:
* Outpatients: Visit for a short consultation or procedure.
* Inpatients: Stay in the hospital for one or more nights.
* Day Cases: Patients who come for a procedure but go home the same day.

Each dataset includes:
* Archive Date: Date of the record.
* Specialty: Medical department the patient is waiting for.
* Case Type: Whether the patient is an Outpatient, Inpatient, or Day Case.
* Age Profile: Age group of the patient.
* Time Bands: How long the patient has been waiting.

## 3. Preparing the Data
Take a look at the datasets, we can see that all four of the inpatient files have the same structure; the four outpatient files also have the same structure. This means that we can append(combine) the four files for each patient type using Power BI.

Before combining the files in Power BI, some data cleaning was required:
* Renaming fields: "Specialty" field in outpatient data was renamed to "Specialty_Name" to match inpatient data for consistency.
* Adding missing columns: The "Case_Type" column was added to Outpatient data.
* Cleaning Duplicates: Duplicate Age Groups (0-15) showing twice we fixed using Trim function.
* Inconsistent Time Bands: Standardized time bands ("18+ months" to "18 Months+") using  with Replace function.
* Handling Missing Values: Replaced blanks in Age Profile and Time Bands columns with "No Input".

After cleaning, all files were appended into a single dataset called All_Data.
The data reflects many specialty names, and our stakeholder wants us to classify them based on groups. They provided a file called Mapping_Specialty which we can load into Power BI.

## 4. Data Model
We created a relationship between our All_Data table and our Mapping_Specialty data so that we can refer to the specialty groupings when we do our analysis.
Now our data is prepared and we can start analyzing.

## 5. Data Analysis 
The analysis was split into two pages: Summary Page and Detailed View Page.

### A. Summary Page
For the Summary page, we want to create a few different charts:

#### Total Waiting List (CY vs PY)
We have to use DAX and create new measures:

* Latest Wait List CY = CALCULATE(SUM(All_Data[Total]), All_Data[Archive_Date]=MAX(All_Data[Archive_Date]))
* Latest Wait List CY = CALCULATE(SUM(All_Data[Total]), All_Data[Archive_Date]=(MAX(All_Data[Archive_Date]),-12))
  
Displayed values in KPI cards.

#### Average vs. Median Toggle buttons
Next, we want to create a few more charts but we don’t want to just look at the average, as we have seen some outliers in the data. We want to show median as well, and to show both in the charts we need to have a toggle for the average and median.

After creating two slicers called "Average" and "Median," we will create two new DAX measures for Average Waiting List and Median Waiting List:
* Average Waiting list = AVERAGE(All_Data[Total])
* Median Waiting list = Median(All_Data[Total])

Now create one more measure that will allow us to interact with the slicer buttons:
* Avg/Median Waiting list = SWITCH(VALUES('Calculation Method'[Calc Method]), "Average", [Average Waiting list], "Median", [Median Waiting list])

#### Donut Chart: Wait List by Case Type
* Legend: Case Type (Inpatient, Outpatient, Day Case)
* Values: Avg/Median Waiting List

#### Stacked Column Chart: Wait List by Age Profile & Time Bands
It is used to show us the relationship between Time Band and each Age profile
* X-Axis: Time Bands (0-3M, 3-6M, 6-9M, etc.)
* Y-Axis: Avg/Median Waiting List
* Legend: Age Profile (0-15, 16-64, 65+)

#### Multi-Row Card: Top 5 Specialties based on Avg/Median waitlist
* Fields: Specialty Name, Avg/Median Waiting List
* Applied a TOPN filter to show only the top 5 specialties.

#### Line chart: Create separate line charts for Inpatients and Outpatients for Total Waitlist over Archive_Date
* X-Axis: Archive Date
* Y-Axis: Total 
* Legend: Case Type (Inpatient, Outpatient, Daycase)
* Applied filters to separate inpatient & outpatient data.

#### Slicers for Interactivity
* Archive_Date (to filter based on time range).
* Case_Type (to switch between Inpatient, Outpatient, and Day Case).
* Speciality_Type (for specialty-wise filtering).

### B. Detailed View Page
* For the Detail page, we will create a matrix of the total patient wait list using rows-(Archive_Date, Age_Profile, Time_Band, Speciality_Type) and columns-(Case_Type) and values-(Total)
* Added slicer-Archive_Date,Case_Type,Specialty_Type

## 6. Key Insights from the Analysis
* Growing Waiting Lists:The number of people waiting increased from 640K to 709K over the past year.
* Most Patients are Outpatients:Outpatients make up 72% of the waitlist.
* Older Patients Wait Longer:Patients aged 65+ tend to have longer wait times.
* Specialties with the Longest Delays:Paediatric Dermatology, ENT, and Orthopaedics have the highest wait times.
* COVID-19 Impact on Waitlists:Inpatient and Day Case wait times increased in 2020, likely due to COVID-19 delays.

## 7. Recommendations
* Increase hospital capacity for Outpatients since they make up the majority of the waitlist.
* Prioritize older patients (65+ years) as they face the longest delays.
* Hire more doctors in high-waitlist specialties like Paediatric Dermatology and ENT.
* Optimize scheduling to manage high wait times in the 0-3 month and 18 month+ categories.

