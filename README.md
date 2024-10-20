# Human Resources Tableau Dashboard

This repository contains the original and working datasets used for creating the Human Resources Dashboard in Tableau. In addition to that, the Data Cleaning and Transformation steps in Excel and a data description of the final dataset used in Tableau.

## Datasets used:

* **DATASET 1:**
[Employee Performance and Productivity Data](https://www.kaggle.com/datasets/mexwell/employee-performance-and-productivity-data)

This dataset is obtained from Kaggle and it contains 20 columns and 100000 rows of employee details.

* **DATASET 2:**
[Human Resources](https://data.world/markbradbourne/rwfd-real-world-fake-data/workspace/file?filename=Hospitality.csv)

This dataset is obtained from data.gov and it contains 13 columns and 22,267 rows of employee details.

## Data Cleaning Steps:

1. Removed 53 rows from Human Resources Dataset where there were no employee associated, but only a column of job titles.

2. Consolidated the two datasets with total rows of 22,215.

3. Replaced id column with a serial Employee Id. Renamed the column headers.

4. Formatted the birth, hire and term dates from MDY to DMY.

    a. There were two types of dates entered into the database - one as dates, and another as text.

    b. The dates were formatted as MDY and were needed to change it to DMY

    c. To do that I first sorted the dates. I extracted Year, Month and Day using YEAR(), MONTH() and DAY() functions from the dates formatted as dates. To extract the same information from dates formatted as text, I used LEFT(), MID() and RIGHT(). These three attributes were saved in 3 separate columns- year, month and day.

    d. I changed the formatting of day, month and year data columns into Text 

    e. Finally using DATE() function I recreated the dates in DMY format and saved it as Birth/Hire Date.

5. Concatenated the first and last name of the employees into full name.

6. Changed the term dates greater than 30-01-2030 with date values ranging between 2015-01-01 and 2030-01-01 to reduce the tenure and age of employees (to keep the age of the employee needs to be under 60 years and tenure less than 30).

7. Some employees have left the company before they were hired. To identify these dates where hire date is later in future than termination date, I used IF() function to flag these data points as R(Right) and E(Error). Then I filtered out the data points with E flag.

8. Tenure is defined as the years spent between hire and last working date. For employees who've resigned, the last working date was their term data and for others it's a date in the future chosen arbitrarily (31-12-2030 in this case). To calculate Tenure, I took these steps:
	
   a. I copied the Term Date column and renamed it Last Working Day. Then filled the blank cells (which reflects employee who are still working for the company) with a future date (i.e., 31-12-2030).
	
   b. Calculated the Tenure by using YEARFRANC() and ROUND() funtions using Hire Date as start date and Last Working Day as end date.

9. Calculated Age of Employee by using YEARFRANC() and ROUND() funtions using Birth Date as start date and Hire Date as end date. It gave me the age of the employee at the date of hiring. 

To double check the calculation, age during hiring + tenure = age during terminationof service

10. After calculating the age, I've found some employees with an age range of 0 - 14 years. Since these ages don't make sense in the context of working professionals. I filtered these datapoints out from the dataset. Now the age range of employees is between 15 to 55.

11. Also the people who have been left are need to be flagged as resigned. If there is a term date, it means the employee has resigned and the Resigned column will have a flag T (True). If the term date is null it means the employee is still working for the company and thus has not resigned (False).

12. After the data manipulations, the number of datapoints have reduced from 22,215 to 17,389. 

13. In this step, I modified the Employee ID column to a serially incrementing column reflecting the final count of employees.

14. Finally, I copied all the data into a new worksheet called Human Rresources Dataset where I removed the columns I created to transform the dates from MDY to DMY. The final dataset contain 23 columns and 17,389 rows. This is the dataset used to create the Human Resources Dashboard.

## Data Description:

The final dataset (Human Resources Data) contains demographic, productivity and attrition information on 17389 employees across 23 attributes. It has been created by combining two datasets. The dates of hiring ranges between 17-10-2000 to 13-10-2020. The term date i.e., the date employees terminated their service ranges from 22-08-2002 to 31-12-2030.

| Column(Data Type) | Description |
| :--- | :--- |
| EmployeeID (Int) | Unique Employee Identifier |	
| Name(Char) | Full name of the employee |	
| Birth Date (Date) | Birth Date of the employee |	
| Age (Int) | Age of the employee at the time of hiring |	
| Gender(Char) | Gender of the employee (Male, Female, Non-Conforming) |
| Race(Char) | Ethnicity of the employee |
| Department(Char) | Name of the Department |
| Job Title(Char) |	Role of the employee |
| Location(Char) | Mode of work (Remote, Headquarter) |
| Hire Date(Date) | Date of hiring the employee |	
| Term Date(Date) |	Date of end of term of the employee |
| Tenure(Int) | The number of years worked for the company |
| State(Char) |	States in the United States where the Company is located |
| Education(Char) | Level of Education of the employee | 
| Performance Score(Int) | Performance Score of the employee (0-5) |
| Monthly Salary ($)(Int) |	Monthly Salary in $ |
| Working Hours per Week(Int) |	Working Hours per Week |
| # of Projects Handled(Int) | Number of Projects Handled |
| # of Sick Days(Int) |	Number of Sick Days |
| Training Hours(Int) |	Total Training Hours |
| # of Promotions(Int) |	Number of Promotions |
| Employee Satisfaction Score(Float) | Employee Satisfaction Score (0-5) |	
| Resigned(Boolean) |	Whether the employee left the company or not (Yes, No) |	

## About the Dashboard:
