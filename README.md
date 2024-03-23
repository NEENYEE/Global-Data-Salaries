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

**6. Update job titles in the 'salaries' table based on specific conditions:**
Updated job titles in the 'salaries' table, mapping various job titles to standardized ones for consistency.


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
    
-- Updating the 'employment_type' column in the 'salaries' table to expand abbreviated employment types

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

-- Update job titles in the 'salaries' table based on specific conditions
UPDATE salaries
SET job_title =
    CASE
        WHEN job_title IN ('Data science', 'Data science practitioner', 'Staff data scientist') THEN 'Data Scientist'
        WHEN job_title IN ('BI Analyst', 'BI Data Analyst', 'Business Intelligence Data Analyst') THEN 'Business Intelligence Analyst'
        WHEN job_title IN ('Data scientist Lead','Data Science Tech Lead', 'Head of data', 'Data manager', 'head of data science') THEN 'Data science manager'
        WHEN job_title IN ('ML engineer', 'Machine learning research engineer', 'Data science engineer')  THEN 'Machine learning engineer' 
        WHEN job_title IN ('Data Lead', 'Data analytics manager', 'Principal data analyst') THEN 'Data Analyst Lead'
        WHEN job_title IN ('Staff data analyst', 'Data science analyst', 'Admin & data analyst') THEN 'Data analyst' 
        ELSE job_title
    END;

    
```

## Data Exploration

Following the completion of the data cleaning process, a comprehensive exploration of the dataset was conducted to gain insights and understand key trends. Here's an overview of the data exploration journey:

**1. How many job titles are we looking at?**
```sql
SELECT COUNT(DISTINCT(job_title))
FROM salaries;
```
There are **129** distinct job titles.

**2. what are these job titles?**
```sql
   SELECT DISTINCT(job_title)
FROM salaries;
```
The dataset includes a diverse range of job titles. They include:

Data Scientist, Data Engineer, Data Analyst, ETL Developer, Data Analyst Lead, Data Specialist, AI Architect, AI Engineer, Data Architect, Data Integration Specialist, Robotics Engineer, Research Analyst, Applied Scientist, BI Developer, Business Intelligence Analyst, Machine Learning Engineer, Research Scientist, Data science manager, Data Modeler, Business Intelligence Engineer, Cloud Database Engineer, Machine Learning Scientist, Data Operations Analyst, Data Developer, Research Engineer, Data Management Analyst, Analytics Engineer, Data Science Consultant, Applied Data Scientist, Business Intelligence, Insight Analyst, Data Quality Engineer, Computational Biologist, AI Research Scientist, MLOps Engineer, Big Data Engineer, Business Intelligence Manager, Prompt Engineer, Data Integration Engineer, Data Analytics Associate, Data Reporting Analyst, Business Intelligence Developer, Data Management Consultant, Data Quality Analyst, Robotics Software Engineer, Machine Learning Researcher, Data DevOps Engineer, AI Software Engineer, Data Operations Specialist, Data Product Manager, Data Science Director, Data Strategist, Big Data Developer, Quantitative Research Analyst, Lead Machine Learning Engineer, Data Infrastructure Engineer, Data Analytics Lead, Data Analytics Consultant, AI Research Engineer, Data Analytics Specialist, Business Intelligence Lead, AI Programmer, ETL Engineer, AI Product Manager, Data Management Specialist, Data Operations Associate, AI Developer, AI Scientist, Computer Vision Engineer, Head of Machine Learning, Machine Learning Operations Engineer, Data Integration Developer, ML Ops Engineer, Data Pipeline Engineer, Lead Data Analyst, Director of Data Science, Managing Director Data Science, Data Visualization Specialist, Data Quality Manager, Data Product Owner, Machine Learning Infrastructure Engineer, Business Data Analyst, NLP Engineer, Marketing Data Scientist, Deep Learning Engineer, Machine Learning Modeler, Business Intelligence Specialist, Decision Scientist, Financial Data Analyst, Data Strategy Manager, Data Visualization Engineer, Azure Data Engineer, Principal Data Scientist, Machine Learning Software Engineer, Applied Machine Learning Scientist, Data Operations Engineer, Machine Learning Manager, Lead Data Scientist, Principal Machine Learning Engineer, Principal Data Engineer, Power BI Developer, Staff Machine Learning Engineer, Consultant Data Engineer, Machine Learning Specialist, Data Operations Manager, Data Modeller, Finance Data Analyst, Software Data Engineer, Compliance Data Analyst, Cloud Data Engineer, Analytics Engineering Manager, AWS Data Architect, Product Data Analyst, Machine Learning Developer, Data Visualization Analyst, Autonomous Vehicle Technician, Sales Data Analyst, Applied Machine Learning Engineer, BI Data Engineer, Deep Learning Researcher, Big Data Architect, Computer Vision Software Engineer, Marketing Data Engineer, Manager Data Management, Marketing Data Analyst, Principal Data Architect, Data Analytics Engineer, Cloud Data Architect, Lead Data Engineer

**3. What are the 10 most popular roles in this field?**

```sql
SELECT job_title, COUNT(job_title) AS job_count
FROM salaries
GROUP BY job_title, work_year
ORDER BY job_count DESC
LIMIT 10;

```
**Output:**
| Job Title               | Job Count |
|-------------------------|-----------|
| Data Scientist          | 3177      |
| Data Engineer           | 3115      |
| Data Analyst            | 2161      |
| Machine Learning Engineer | 1692    |
| Research Scientist      | 467       |
| Analytics Engineer      | 397       |
| Data science manager   | 386       |
| Applied Scientist       | 373       |
| Data Architect          | 355       |
| Research Engineer       | 272       |

The above table shows the 10 most popular entries among the survey responses. These findings provide valuable insights into the prevalent job roles within the data field and highlight areas of significant interest among survey participants. However, it's important to note that the popularity of these job titles may not directly correlate with the actual employment numbers or workforce distribution. Further investigation is necessary to understand the underlying factors driving the prominence of these roles.

**4. Has these job titles reamained popular over the years?**

```sql
SELECT work_year, job_title, job_count
FROM (
    SELECT 
        work_year,
        job_title,
        COUNT(job_title) AS job_count,
        ROW_NUMBER() OVER (PARTITION BY work_year ORDER BY COUNT(job_title) DESC) AS ranks
    FROM salaries
    GROUP BY work_year, job_title
) AS RankedRoles
WHERE ranks <= 10;

```
**Output:**
### Year 2020

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Scientist            | 22        |
| Data Engineer             | 13        |
| Data Analyst              | 8         |
| Machine Learning Engineer | 5         |
| Business Data Analyst     | 3         |
| Big Data Engineer         | 3         |
| Research Scientist        | 2         |
| Lead Data Scientist       | 2         |
| Lead Data Engineer        | 2         |
| Azure Data Engineer       | 1         |

### Year 2021

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Scientist            | 41        |
| Data Engineer             | 37        |
| Machine Learning Engineer | 25        |
| Data Analyst              | 20        |
| Data Science Manager      | 12        |
| Research Scientist        | 10        |
| Principal Data Scientist  | 5         |
| Director of Data Science  | 5         |
| Data Science Consultant   | 5         |
| Big Data Engineer         | 4         |

### Year 2022

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Engineer             | 488       |
| Data Scientist            | 404       |
| Data Analyst              | 273       |
| Machine Learning Engineer | 125       |
| Analytics Engineer        | 56        |
| Data Science Manager      | 47        |
| Data Architect            | 46        |
| Applied Scientist         | 18        |
| Research Scientist        | 15        |
| Machine Learning Scientist| 14        |

### Year 2023

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Engineer             | 1852      |
| Data Scientist            | 1759      |
| Data Analyst              | 1268      |
| Machine Learning Engineer | 1083      |
| Research Scientist        | 286       |
| Applied Scientist         | 280       |
| Data Science Manager      | 254       |
| Analytics Engineer        | 222       |
| Data Architect            | 200       |
| Research Engineer         | 164       |

### Year 2024

| Job Title                 | Job Count |
|---------------------------|-----------|
| Data Scientist            | 951       |
| Data Engineer             | 725       |
| Data Analyst              | 592       |
| Machine Learning Engineer | 454       |
| Research Scientist        | 154       |
| Analytics Engineer        | 119       |
| Data Architect            | 106       |
| Research Engineer         | 100       |
| Applied Scientist         | 75        |
| Data Science Manager      | 72        |

The trend analysis shows that the Top 10 popular job titles continue to dominate the data science and analytics landscape, reflecting their essential contributions to organizations in various industries.
Data Scientist: The demand for Data Scientists has been consistently high throughout the years, showing a steady increase from 2020 to 2023, with a slight decrease in 2024.
- Data Engineer: Data Engineer positions have also maintained a high demand, particularly in 2023, with a slight decrease in 2024 compared to the previous year.
- Data Analyst: The demand for Data Analysts has remained relatively stable over the years, showing a slight increase from 2020 to 2021 and then maintaining a similar level in subsequent years.
- Machine Learning Engineer: The demand for Machine Learning Engineers has seen fluctuations, with a peak in 2023 followed by a slight decrease in 2024.
- Other Roles: Other roles such as Research Scientist, Analytics Engineer, and Applied Scientist have shown varying levels of demand across the years, with some experiencing peaks in specific years.

*Note: 2024 has just three months( Jan to Mar).*

Overall, the trend suggests a growing demand for data-related roles, particularly for Data Scientists and Data Engineers, indicating the importance of data-driven decision-making in various industries. However, fluctuations in demand across different roles also reflect the evolving nature of the field and the changing requirements of organizations over time.

**4. Which job titles tend to command higher salaries within the data field?**

```SQL
SELECT 
    job_title, 
    ROUND(AVG(salary_in_usd), 0) AS avg_salary,
    MIN(salary_in_usd) AS min_salary,
    MAX(salary_in_usd) AS max_salary
FROM
    salaries
GROUP BY job_title
ORDER BY avg_salary DESC
LIMIT 10;

```

**Output:**
| Job Title                      | Avg Salary | Min Salary | Max Salary |
|--------------------------------|------------|------------|------------|
| Analytics Engineering Manager | $399,880   | $399,880   | $399,880   |
| Head of Machine Learning      | $299,758   | $76,309    | $448,000   |
| Managing Director Data Science| $280,000   | $260,000   | $300,000   |
| AWS Data Architect            | $258,000   | $258,000   | $258,000   |
| AI Architect                  | $255,142   | $99,750    | $800,000   |
| Cloud Data Architect          | $250,000   | $250,000   | $250,000   |
| Director of Data Science      | $218,775   | $57,786    | $375,500   |
| Data Infrastructure Engineer  | $207,333   | $135,920   | $385,000   |
| Prompt Engineer               | $206,075   | $60,462    | $600,000   |
| Data Analytics Lead           | $198,242   | $17,511    | $405,000   |

**5. Are there factors Contributing to Salary Disparities amongst job titles?**

```sql
-- Based on experience level
SELECT job_title, 
       experience_level,
       ROUND(AVG(salary_in_usd),1) AS avg_salary
FROM salaries
GROUP BY job_title, experience_level
ORDER BY job_title ASC;

-- Based on employment type
SELECT job_title, 
       employment_type,
       ROUND(AVG(salary_in_usd),1) AS avg_salary
FROM salaries
GROUP BY job_title, employment_type
ORDER BY job_title ASC;

-- Based on company size
SELECT job_title, 
       company_size,
       ROUND(AVG(salary_in_usd),1) AS avg_salary
FROM salaries
GROUP BY job_title, company_size
ORDER BY job_title ASC;
```
**Company Size Impact:** Across all roles, employees in large companies tend to earn higher salaries compared to those in smaller companies. This trend suggests that larger companies may have more resources and financial capacity to offer competitive compensation packages to their employees.

**Experience Level Impact:** Directors consistently earn higher salaries across different roles compared to other experience levels. This finding highlights the value placed on experience and seniority within organizations, where individuals in leadership positions command higher compensation due to their expertise and responsibilities.

**Employment Type Impact:** Full-time employees tend to earn higher salaries compared to those in other employment types. This observation underscores the traditional employment model where full-time positions often come with additional benefits and stability, leading to higher overall compensation for employees.

**6. Distribution of average salaries by country**

```sql
-- TOP 10
SELECT country, CONCAT('$', FORMAT(avg_salary, 0)) AS avrg_salary FROM
(SELECT  c.country, AVG(s.salary_in_usd) AS avg_salary
FROM salaries s
INNER JOIN countries c
ON s.company_location = c.code
GROUP BY c.country
ORDER BY avg_salary DESC
LIMIT 10) AS Top;

-- BOTTOM 10
SELECT country, CONCAT('$', FORMAT(avg_salary, 0)) AS avrg_salary FROM
(SELECT  c.country, AVG(s.salary_in_usd) AS avg_salary
FROM salaries s
INNER JOIN countries c
ON s.company_location = c.code
GROUP BY c.country
ORDER BY avg_salary 
LIMIT 10) AS Bottom;

```
**Output: Top 10**
| Country                  | Avg_Salary    |
|--------------------------|---------------|
| Qatar                    | $300,000      |
| Israel                   | $217,332      |
| Puerto Rico              | $167,500      |
| United States of America | $157,449      |
| New Zealand              | $147,682      |
| Canada                   | $145,845      |
| Egypt                    | $140,869      |
| Saudi Arabia             | $139,999      |
| Australia                | $131,205      |
| Mexico                   | $129,241      |


**Output: Bottom 10**
| Country        | Avg_Salary   |
|----------------|--------------|
| Ecuador        | $16,000.00   |
| Moldova        | $18,000.00   |
| Honduras       | $20,000.00   |
| Thailand       | $22,971.00   |
| Turkey         | $23,095.00   |
| Ghana          | $27,000.00   |
| Pakistan       | $30,000.00   |
| American Samoa | $31,684.00   |
| Indonesia      | $34,208.00   |
| Hungary        | $39,938.00   |

**7. Salary ranges for entry level positions**

```sql
-- This view filters data from the salaries table to include only entry-level positions and joins it with the countries table to include the country information.
-- It selects relevant columns such as work_year, employment_type, job_title, salary_in_usd, remote_status, company_size, and country for analysis.
-- The view provides a subset of data focusing on entry-level positions, suitable for further analysis.

CREATE VIEW entry AS
    SELECT 
        s.work_year,
        s.employment_type,
        s.job_title,
        s.salary_in_usd,
        s.remote_status,
        s.company_size,
        c.country
    FROM
        salaries s
            INNER JOIN
        countries c ON s.company_location = c.code
    WHERE
        s.experience_level = 'Entry-level';
        
-- Salary ranges for entry level positions
SELECT 
    job_title,
    MIN(salary_in_usd) AS min_salary,
    MAX(salary_in_usd) AS max_salary,
    ROUND(AVG(salary_in_usd),0) AS avg_salary
FROM 
    entry
GROUP BY 
    job_title;
```

**8. Average salaries for entry level positions by country**

```sql
SELECT 
    country,
    FORMAT(AVG(salary_in_usd),0) AS avg_entry_salary
FROM 
    entry
GROUP BY 
    country
ORDER BY 
    avg_entry_salary DESC
    LIMIT 10;
```

**9. what is the average pay for entry level data analysts in UK, Nigeria, Australia, US and Canada?**

```sql
SELECT 
    FORMAT(AVG(salary_in_usd), 1) AS avg_salary,
    country
FROM
    entry
WHERE
    country IN ('United kingdom' , 'Nigeria',
        'United states of America',
        'Canada',
        'Australia')
        AND job_title = 'data analyst' 
GROUP BY job_title , country
ORDER BY avg_salary DESC;

```

**10. Are there specific countries where entry level jobs tend to earn higher than average?**

```SQL
SELECT 
    country,
    ROUND(AVG(salary_in_usd), 1) AS avg_entry_salary,
    COUNT(job_title) AS employee_count
FROM
    entry
GROUP BY country
HAVING AVG(salary_in_usd) > 91897.8
ORDER BY avg_entry_salary DESC;
```
**11. what roles are entry_levels most hired for?**

```SQL
SELECT 
    job_title, COUNT(job_title) AS employee_count
FROM
    entry
GROUP BY job_title
ORDER BY employee_count DESC;
```

**12. Any growth or decline of entry-level positions over time?**
```SQL
SELECT 
    work_year,
    COUNT(*) AS entry_level_count
FROM 
    entry
GROUP BY 
    work_year
ORDER BY 
   entry_level_count DESC;
```

**13. And for data analysts?**
```SQL
SELECT 
    work_year, job_title,
    COUNT(*) AS entry_level_count
FROM 
    entry
    WHERE job_title = 'data analyst'
GROUP BY 
    work_year
ORDER BY 
   entry_level_count DESC;
```

**14. Impact of company size and remote work availability on entry-level employment**
```SQL
SELECT 
    company_size,remote_status, 
    AVG(salary_in_usd) AS avg_salary,
    COUNT(*) AS entry_level_count
FROM 
    entry
GROUP BY 
    company_size, remote_status
ORDER BY 
    company_size, avg_salary DESC;
```

**15. How much are fully remote entry level employees paid by country?**
```SQL
SELECT country, ROUND(avg(salary_in_usd),0) AS avg_salary
from entry
where remote_status LIKE '%remote'   
GROUP BY country
ORDER BY avg_salary DESC;
```

**16. The average salary for different experience levels across different years to analyze year-over-year (YoY) salary growth.**
```SQL
SELECT 
    work_year,
    experience_level,
    FORMAT(AVG(salary_in_usd),0) AS avg_salary
FROM 
    salaries
GROUP BY 
    work_year, experience_level
ORDER BY 
    experience_level DESC, work_year;

```



