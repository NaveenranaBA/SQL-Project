# SQL-Project
Introduction
📊 Dive into the data job market! Focusing on data analyst roles, this project explores 💰 top-paying jobs, 🔥 in-demand skills, and 📈 where high demand meets high salary in data analytics.

SQL queries? Check them out here: project_sql folder
Background
Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlining others work to find optimal jobs.

Data hails from my SQL Course. It's packed with insights on job titles, salaries, locations, and essential skills.

The questions I wanted to answer through my SQL queries were:
What are the top-paying data analyst jobs?
What skills are required for these top-paying jobs?
What skills are most in demand for data analysts?
Which skills are associated with higher salaries?
What are the most optimal skills to learn?
Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

SQL: The backbone of my analysis, allowing me to query the database and unearth critical insights.
PostgreSQL: The chosen database management system, ideal for handling the job posting data.
Visual Studio Code: My go-to for database management and executing SQL queries.
Git & GitHub: Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.
The Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here's how I approached each question:

1. Top Paying Data Analyst Jobs
To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.

SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim
    ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_location = 'Anywhere'
    AND salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
Here's the breakdown of the top data analyst jobs in 2023:

Wide Salary Range: Top 10 paying data analyst roles span from $184,000 to $650,000, indicating significant salary potential in the field.
Diverse Employers: Companies like SmartAsset, Meta, and AT&T are among those offering high salaries, showing a broad interest across different industries.
Job Title Variety: There's a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within data analytics.
2. Top Paying Job Skills
To identify the skills required for the highest-paying data analyst roles, I first identified the top 10 highest-paying Data Analyst jobs and then joined them with the skills associated with each position. This query highlights the technical skills commonly required for high-paying opportunities in data analytics.

WITH top_paying_jobs AS ( SELECT job_id, job_title, salary_year_avg, name AS company_name FROM job_postings_fact LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id WHERE job_title_short = 'Data Analyst' AND job_location = 'Anywhere' AND salary_year_avg IS NOT NULL ORDER BY salary_year_avg DESC LIMIT 10 )

SELECT
    top_paying_jobs.*,
    skills_dim.skills
FROM top_paying_jobs
INNER JOIN skills_job_dim
    ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim
    ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
Here's the breakdown of the skills required for the top-paying data analyst jobs:

Strong SQL Demand: SQL appears repeatedly across the highest-paying roles, highlighting its importance for securing well-paid opportunities in data analytics.

Python & R: Programming languages such as Python and R are frequently required, demonstrating the importance of programming and statistical analysis skills in higher-paying roles.

Cloud & Data Platforms: Technologies such as AWS, Azure, Snowflake, Databricks, and Hadoop appear among the skills required for top-paying positions, showing the growing importance of cloud and big-data technologies.

Data Visualization: Tools such as Tableau, Power BI, and Excel are also present in several high-paying roles, emphasizing the importance of communicating insights effectively.

Advanced Data Tools: Skills including Pandas, NumPy, PySpark, Jupyter, Git, and Bitbucket appear in several of the top-paying positions, indicating that employers value candidates with a broader technical toolkit.

3. Top Demanded Skills
To identify the most in-demand skills for data analysts, I analyzed job postings and counted how frequently each skill appeared in Data Analyst job listings. This query highlights the technical skills most commonly requested by employers.

SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM
    job_postings_fact
INNER JOIN skills_job_dim
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE 
        job_title_short='Data Analyst'
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
Here's the breakdown of the top 5 most demanded skills for data analysts:

SQL: SQL is the most in-demand skill, appearing in 92,628 job postings, highlighting its importance as a core skill for data analysts.

Excel: Excel ranks second with 67,031 job postings, showing that traditional spreadsheet and data-analysis skills remain highly valuable.

Python: Python appears in 57,326 job postings, demonstrating the growing importance of programming and data manipulation in analytics roles.

Tableau: Tableau is mentioned in 46,554 job postings, highlighting the demand for data visualization and dashboarding skills.

Power BI: Power BI appears in 39,468 job postings, further emphasizing the importance of business intelligence and data visualization tools.

Overall, the results show that a strong Data Analyst skill set combines SQL and Excel fundamentals with Python, Tableau, and Power BI.

4. Top Paying Skills
To identify the skills associated with the highest salaries for data analysts, I calculated the average annual salary for each skill across Data Analyst job postings with specified salaries. This query highlights the skills that are associated with the highest-paying opportunities in the field.

SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM
    job_postings_fact
INNER JOIN skills_job_dim
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
Here's the breakdown of the top 25 highest-paying skills for data analysts:

SVN: SVN ranks highest with an average salary of $400,000, making it the highest-paying skill in the analysis.

Solidity: Solidity follows with an average salary of $179,000, highlighting the premium associated with specialized technical expertise.

Couchbase & Databrobot: These skills are also associated with high average salaries of approximately $160,515 and $155,486, respectively.

Programming & Machine Learning Skills: Skills such as Golang, MXNet, Dplyr, Keras, PyTorch, TensorFlow, and Hugging Face appear among the highest-paying skills, indicating strong salary potential for specialized programming and machine learning expertise.

Cloud & DevOps Skills: Technologies such as VMware, Terraform, GitLab, Kafka, Puppet, and Ansible also rank highly, demonstrating the value of cloud infrastructure, DevOps, and data engineering-related skills.

Overall, the results suggest that specialized and technical skills tend to be associated with higher average salaries, although these skills may appear in fewer job postings compared with more commonly demanded skills such as SQL, Excel, and Python.

5. Most Optimal Skills
To identify the most optimal skills for data analysts, I combined skill demand with average salary for remote Data Analyst positions. I focused on skills appearing in more than 10 job postings to identify skills that offer a strong balance between market demand and earning potential.

WITH skills_demand AS (
    SELECT
        skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM
        job_postings_fact
    INNER JOIN skills_job_dim
        ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim
        ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst'
    GROUP BY
        skills_dim.skill_id,
        skills_dim.skills
),

average_salary AS (
    SELECT
        skills_dim.skill_id,
        skills_dim.skills,
        ROUND(AVG(salary_year_avg), 0) AS avg_salary
    FROM
        job_postings_fact
    INNER JOIN skills_job_dim
        ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim
        ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
    GROUP BY
        skills_dim.skill_id,
        skills_dim.skills
)

SELECT
    skills_demand.skill_id,
    skills_demand.skills,
    demand_count,
    avg_salary
FROM
    skills_demand
INNER JOIN average_salary
    ON skills_demand.skill_id = average_salary.skill_id
WHERE
    demand_count > 10
ORDER BY
    demand_count DESC,
    avg_salary DESC
LIMIT 25;
Here's the breakdown of the most optimal skills for data analysts:

Go: Go ranks highest with an average salary of $115,320 and appears in 27 job postings, showing strong earning potential despite being a relatively specialized skill.

Cloud & Data Technologies: Skills such as Hadoop, Snowflake, Azure, BigQuery, AWS, and Oracle show a strong combination of demand and high average salaries, highlighting the value of cloud computing and data infrastructure skills.

Python: Python stands out with 236 job postings and an average salary of $101,397, making it one of the strongest combinations of high demand and high pay.

Tableau: Tableau appears in 230 job postings with an average salary of $99,288, demonstrating strong demand for data visualization skills alongside competitive salaries.

R: R appears in 148 job postings with an average salary of $100,499, making it another valuable analytical skill with both significant demand and strong earning potential.

SQL Server & SAS: SQL Server and SAS also demonstrate a good balance between demand and salary, with SQL Server appearing in 35 postings and SAS in 63 postings.

Overall, the results show that the most optimal skills are not necessarily the skills with the highest salaries alone. Skills such as Python, Tableau, R, Snowflake, AWS, Azure, and Hadoop provide a stronger combination of demand and earning potential.

What I Learned
Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:

Complex Query Crafting: Mastered the art of advanced SQL, merging tables like a pro and wielding WITH clauses for ninja-level temp table maneuvers.
Data Aggregation: Got cozy with GROUP BY and turned aggregate functions like COUNT() and AVG() into my data-summarizing sidekicks.
Analytical Wizardry: Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.
Conclusions
Insights
Top-Paying Data Analyst Jobs: The highest-paying jobs for data analysts that allow remote work offer a wide range of salaries, with the highest at $650,000.

Skills for the Top-Paying Jobs: High-paying data analyst jobs require advanced proficiency in SQL, suggesting it's a critical skill for earning a top salary.

Most In-Demand Skills: SQL is also the most demanded skill in the data analyst job market, thus making it essential for job seekers.

Skills with Higher Salaries: Specialized skills, such as SVN and Solidity, are associated with the highest average salaries, indicating a premium on niche expertise.

Optimal Skills for Job Market Value: SQL leads in demand and offers a high average salary, positioning it as one of the most optimal skills for data analysts to learn to maximize their market value.

Closing Thoughts
This project enhanced my SQL skills and provided valuable insights into the data analyst job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlights the importance of continuous learning and adaptation to emerging trends in the field of data analytics.
