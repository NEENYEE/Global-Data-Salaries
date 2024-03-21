# "Unlocking Insights: Exploring Global Data Field Salaries"
![salaries](https://www.notion.so/image/https%3A%2F%2Fdataladder.com%2Fwp-content%2Fuploads%2F2021%2F09%2Fdata-profiling-2.jpg?table=block&id=0fcd02f8-363f-48d6-92f4-2c8268117cfe&spaceId=03a5331f-7c39-4eff-9a3e-e3ea3964c3df&width=1600&userId=1c9e6545-ac84-4e85-ae0f-a7428cd7a631&cache=v2)

## Overview
This documentation explores information about salaries for data jobs. The dataset provides insights into salary trends from 2020 - 2024, geographical distribution, remote work opportunities, and factors affecting salary disparities within various roles in the data field.

## Problem Statement
Being an entry-level data analyst, I undertook this project out of curiosity to delve into the intricacies of the data job market and understand the salary dynamics within the global data industry. With a particular focus on entry-level roles, the objective is to gain a nuanced understanding of various facets:

- **Exploring Salary Trends:** Analyzing salary dynamics across regions and industries to uncover patterns and growth trajectories within the data science domain.

- **Identifying Popular Job Titles:** Examining the most sought-after job titles in data science and their corresponding salary distributions to understand career progression paths and emerging industry trends.

- **Analyzing Geographic Contributions:** Investigating how geographical factors influence salary disparaties and opportunities in entry-level data science roles, including regional variations in compensation and factors contributing to these disparities.

## Data Details
The dataset utilized was obtained from [here](https://ai-jobs.net/salaries/download/). It contains 11 columns and 14,515 rows. It includes details such as the year of salary payment, experience level, employment type, job title, salary amount, currency, remote work ratio, company location, and company size. 

Additionaly, a supplementary dataset containing ISO 2 country codes and the corresponding country names was obtained from [here](https://www.kaggle.com/datasets/wbdill/country-codes-iso-3166). This dataset was beneficial during geographical analysis, as the countrries in the main dataset were represented in ISO-2 country codes.

Both of these datasets were stored within the "Data_Jobs" database schema represented in the Entity Relationship Diagram(ERD) below:

![Screenshot 2024-03-20 at 8 57 28 PM 2](https://github.com/NEENYEE/Global-Data-Salaries/assets/101926233/6c8e651d-9f20-41fb-a9b5-4757f34093b3)

## Data Cleaning and Feature Engineering

The dataset underwent preliminary data cleaning steps to ensure its quality and integrity. The following checks were performed using Power Query in Microsoft Excel:

- **Null Values:** The dataset was inspected for null values, and none were found across any of the columns.

- **Duplicates:** Duplicate records were checked and removed from the dataset. No duplicate rows were detected.

To enhance the dataset's usability and analytical potential, some feature engineering tasks were carried out, as seen below:

**1. Removing Unnecessary Columns:**
The columns "salary", "salary_currency", and "employee_residence" were removed from the salaries table to streamline the dataset, as they were not necessary for this analysis.

**2. Updating Experience Levels:**
The experience_level column in the salaries table was updated to expand abbreviated experience levels. The abbreviations 'SE', 'MI', 'EN', and 'EX' were replaced with 'Senior', 'Mid-level', 'Entry-level', and 'Executive', respectively.

**3. Updating Employment Types:**
The employment_type column in the salaries table was updated to expand abbreviated employment types. The abbreviations 'FT', 'CT', 'FL', and 'PT' were replaced with 'full-time', 'contract', 'freelance', and 'part-time', respectively.

**4. Renaming and Modifying Columns:**
The remote_ratio column in the salaries table was renamed to remote_status, and its data type was modified to accommodate descriptive values.

**5. Updating Remote Status:**
The remote_status column in the salaries table was updated based on the remote_ratio values. The numeric values representing remote work ratios were replaced with descriptive categories ('fully remote', 'hybrid', 'on-site').


```sql
-- Removing unnecessary columns from the 'salaries' table

ALTER TABLE salaries
DROP COLUMN salary, DROP COLUMN salary_currency, DROP COLUMN employee_residence;

-- Update the 'experience_level' column in the 'salaries' table to expand abbreviated experience levels

UPDATE salaries 
SET 
    experience_level = CASE
        WHEN experience_level = 'SE' THEN 'Senior'
        WHEN experience_level = 'MI' THEN 'Mid-level'
        WHEN experience_level = 'EN' THEN 'Entry-level'
        WHEN experience_level = 'EX' THEN 'Executive'
    END;
    
-- Update the 'employment_type' column in the 'salaries' table to expand abbreviated employment types

UPDATE salaries 
SET 
    employment_type = CASE
        WHEN employment_type = 'FT' THEN 'full-time'
        WHEN employment_type = 'CT' THEN 'contract'
        WHEN employment_type = 'FL' THEN 'freelance'
        WHEN employment_type = 'PT' THEN 'part-time'
    END;
    
-- Renaming the 'remote_ratio' column to 'remote_status' in the 'salaries' table
ALTER TABLE salaries
RENAME COLUMN remote_ratio TO remote_status;

-- Modifying the data type of the 'remote_status' 
ALTER TABLE salaries
MODIFY COLUMN remote_status VARCHAR(30);

-- Updating the 'remote_status' column in the 'salaries' table based on the 'remote_ratio' values
UPDATE salaries 
SET 
    remote_status = CASE
        WHEN remote_status = 100 THEN 'fully remote'
        WHEN remote_status = 50 THEN 'hybrid'
        WHEN remote_status = 0 THEN 'on-site'
    END;
    
```

## Data Exploration

Following the completion of the data cleaning process, a comprehensive exploration of the dataset was conducted to gain insights and understand key trends. Here's an overview of the data exploration journey:

1. How many job titles are we looking at?
```sql
SELECT COUNT(DISTINCT(job_title))
FROM salaries;
```
There are **150** distinct job titles.

2. what are these job titles?
```sql
   SELECT DISTINCT(job_title)
FROM salaries;
```
The dataset includes a diverse range of job titles, including Data Science, Data Engineer, Data Analyst, ETL Developer, 
Data Analytics Manager, Data Scientist, Data Specialist, AI Architect, AI Engineer, Data Architect, Data Integration Specialist, 
Robotics Engineer, Research Analyst, Applied Scientist, BI Developer, Business Intelligence Analyst, Machine Learning Engineer, 
Research Scientist, Head of Data, Data Science Manager, Data Modeler, Business Intelligence Engineer, Cloud Database Engineer, 
Machine Learning Scientist, Data Operations Analyst, Data Developer, Research Engineer, Data Science Analyst, Data Science Practitioner, 
Data Management Analyst, Analytics Engineer, Data Science Consultant, BI Data Analyst, Applied Data Scientist, Business Intelligence, 
Insight Analyst, Data Quality Engineer, BI Analyst, Data Manager, Computational Biologist, AI Research Scientist, MLOps Engineer, 
Big Data Engineer, Business Intelligence Manager, Prompt Engineer, Data Integration Engineer, Data Analytics Associate, 
Data Reporting Analyst, Business Intelligence Developer, Data Management Consultant, Data Quality Analyst, ML Engineer, 
Robotics Software Engineer, Machine Learning Researcher, Data DevOps Engineer, AI Software Engineer, Data Operations Specialist, 
Data Product Manager, Data Science Director, Data Strategist, Big Data Developer, Quantitative Research Analyst,
Lead Machine Learning Engineer, Machine Learning Research Engineer, Data Infrastructure Engineer, Data Analytics Lead, 
Data Analytics Consultant, AI Research Engineer, Data Analytics Specialist, Data Science Engineer, Business Intelligence Lead, 
AI Programmer, ETL Engineer, AI Product Manager, Data Management Specialist, Data Operations Associate, AI Developer,
Admin & Data Analyst, AI Scientist, Computer Vision Engineer, Head of Machine Learning, Data Analyst Lead, 
Machine Learning Operations Engineer, Data Lead, Data Integration Developer, ML Ops Engineer, Data Pipeline Engineer, 
Lead Data Analyst, Data Science Lead, Director of Data Science, Managing Director Data Science, Data Visualization Specialist, 
Data Quality Manager, Data Product Owner, Machine Learning Infrastructure Engineer, Business Data Analyst, NLP Engineer, 
Marketing Data Scientist, Deep Learning Engineer, Machine Learning Modeler, Business Intelligence Specialist, Decision Scientist, 
Financial Data Analyst, Data Strategy Manager, Data Visualization Engineer, Azure Data Engineer, Principal Data Scientist, 
Staff Data Analyst, Machine Learning Software Engineer, Applied Machine Learning Scientist, Data Operations Engineer, 
Machine Learning Manager, Lead Data Scientist, Principal Machine Learning Engineer, Principal Data Engineer, 
Power BI Developer, Head of Data Science, Staff Machine Learning Engineer, Staff Data Scientist, 
Consultant Data Engineer, Machine Learning Specialist, Business Intelligence Data Analyst, Data Operations Manager,
Data Modeller, Finance Data Analyst, Software Data Engineer, Compliance Data Analyst, Cloud Data Engineer, 
Analytics Engineering Manager, AWS Data Architect, Product Data Analyst, Machine Learning Developer, 
Data Visualization Analyst, Autonomous Vehicle Technician, Sales Data Analyst, Applied Machine Learning Engineer, 
BI Data Engineer, Deep Learning Researcher, Big Data Architect, Computer Vision Software Engineer, Marketing Data Engineer, 
Manager Data Management, Data Science Tech Lead, Data Scientist Lead, Marketing Data Analyst, Principal Data Architect, 
Data Analytics Engineer, Cloud Data Architect, Lead Data Engineer, and Principal Data Analyst.

| job_title               | job_count |
|-------------------------|-----------|
| Data Engineer           | 1852      |
| Data Scientist          | 1711      |
| Data Analyst            | 1266      |
| Machine Learning Engineer | 966      |
| Data Scientist          | 792       |
| Data Engineer           | 725       |
| Data Analyst            | 587       |
| Data Engineer           | 488       |
| Machine Learning Engineer | 412      |
| Data Scientist          | 404       |



| work_year | job_title                 | job_count |
|-----------|---------------------------|-----------|
| 2020      | Data Scientist            | 21        |
| 2020      | Data Engineer             | 13        |
| 2020      | Data Analyst              | 6         |
| 2020      | Machine Learning Engineer| 4         |
| 2020      | Business Data Analyst    | 3         |
| 2020      | Big Data Engineer         | 3         |
| 2020      | Staff Data Analyst        | 2         |
| 2020      | Research Scientist        | 2         |
| 2020      | Lead Data Scientist       | 2         |
| 2020      | Lead Data Engineer        | 2         |
| 2021      | Data Scientist            | 40        |
| 2021      | Data Engineer             | 37        |
| 2021      | Data Analyst              | 20        |
| 2021      | Machine Learning Engineer| 18        |
| 2021      | Research Scientist        | 10        |
| 2021      | Data Science Manager      | 6         |
| 2021      | Principal Data Scientist  | 5         |
| 2021      | Director of Data Science  | 5         |
| 2021      | Data Science Consultant   | 5         |
| 2021      | ML Engineer               | 4         |
| 2022      | Data Engineer             | 488       |
| 2022      | Data Scientist            | 404       |
| 2022      | Data Analyst              | 272       |
| 2022      | Machine Learning Engineer| 108       |
| 2022      | Analytics Engineer        | 56        |
| 2022      | Data Architect            | 46        |
| 2022      | Data Science Manager      | 30        |
| 2022      | Applied Scientist         | 18        |
| 2022      | Research Scientist        | 15        |
| 2022      | ML Engineer               | 15        |
| 2023      | Data Engineer             | 1852      |
| 2023      | Data Scientist            | 1711      |
| 2023      | Data Analyst              | 1266      |
| 2023      | Machine Learning Engineer| 966       |
| 2023      | Research Scientist        | 286       |
| 2023      | Applied Scientist         | 280       |
| 2023      | Analytics Engineer        | 222       |
| 2023      | Data Architect            | 200       |
| 2023      | Research Engineer         | 164       |
| 2023      | Business Intelligence Engineer| 164   |
| 2024      | Data Scientist            | 792       |
| 2024      | Data Engineer             | 725       |
| 2024      | Data Analyst              | 587       |
| 2024      | Machine Learning Engineer| 412       |
| 2024      | Research Scientist        | 154       |
| 2024      | Data Science              | 153       |
| 2024      | Analytics Engineer




### Year 2020                                                

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Scientist            | 21        |
| Data Engineer             | 13        |
| Data Analyst              | 6         |
| Machine Learning Engineer| 4         |
| Business Data Analyst    | 3         |
| Big Data Engineer         | 3         |
| Staff Data Analyst        | 2         |
| Research Scientist        | 2         |
| Lead Data Scientist       | 2         |
| Lead Data Engineer        | 2         |



### Year 2022

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Engineer             | 488       |
| Data Scientist            | 404       |
| Data Analyst              | 272       |
| Machine Learning Engineer| 108       |
| Analytics Engineer        | 56        |
| Data Architect            | 46        |
| Data Science Manager      | 30        |
| Applied Scientist         | 18        |
| Research Scientist        | 15        |
| ML Engineer               | 15        |

### Year 2023

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Engineer             | 1852      |
| Data Scientist            | 1711      |
| Data Analyst              | 1266      |
| Machine Learning Engineer| 966       |
| Research Scientist        | 286       |
| Applied Scientist         | 280       |
| Analytics Engineer        | 222       |
| Data Architect            | 200       |
| Research Engineer         | 164       |
| Business Intelligence Engineer| 164   |

### Year 2024

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Scientist            | 792       |
| Data Engineer             | 725       |
| Data Analyst              | 587       |
| Machine Learning Engineer| 412       |
| Research Scientist        | 154       |
| Data Science              | 153       |
| Analytics Engineer        | 119       |
| Data Architect            | 106       |
| Research Engineer         | 100       |
| Applied Scientist         | 75        |

