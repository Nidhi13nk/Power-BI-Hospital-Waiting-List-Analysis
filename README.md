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
