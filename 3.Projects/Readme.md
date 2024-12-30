# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles, with an emphasis on ASEAN countries. This project was created out of a desire to navigate and understand the job market more effectively as well as an opportunity to practice and enhance my python skills. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

The following questions are the focus of the project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)?

# Tools I Used

For my deep dive into the data analyst job market, I utilized the power of several key tools:

- **Python:** The primary tool of this analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** The basis for visualizing the data.
    - **Seaborn Library:** The tool that allowed me a higher customization of the visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, allowing tracking, backing up my data and potential collaboration & peer review.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import ast
from datasets import load_dataset

dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter ASEAN jobs

I chose to focus my analysis on the job market of the following ASEAN members, so this filter was applied to the dataset, narrowing it down to roles based in the following countries.

```python
ASEAN = ['Brunei', 'Cambodia', 'Indonesia', 'Laos', 'Malaysia', 'Myanmar', 'Philippines', 'Singapore', 'Thailand', 'Vietnam']
df_SEA = df[df['job_country'].isin(ASEAN)].copy()
```

# The Analysis

Each Jupyter notebook of this project aimed to investigate specific aspects of the data job market. Here's how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2.Skills_Demand_ASEAN](2.Skills_Demand_ASEAN.ipynb).

### Visualizing the Data

```Python
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_per[df_skills_per['job_title_short'] == job_title].head(5)
    sns.barplot(
        data=df_plot, 
        y='job_skills', 
        x='skills_percent', 
        ax=ax[i], hue='skills_percent', 
        palette='dark:b_r'
        )

plt.show()
```

### Results

![Likelihood of Skills Requested in the ASEAN Job Postings](<Images/Likelihood of Skills Requested in the ASEAN Job Postings.png>)  
*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights
- SQL is the most requested skill for Data Analysts and Data Engineer, at almost half for Data Analysts and more than half for Data Engineers. For Data Scientist, Python is the most sought-after skill, appearing in 59% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (59%) and Data Engineers (53%).

## 2. How are in-demand skills trending for Data Analysts?
To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3.Skills_Trend_ASEAN](3.Skills_Trend_ASEAN.ipynb)

### Visualizing the Data

```Python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_ASEAN_per.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```
### Results

![Trending Top Skills for Data Analysts in ASEAN Countries](<Images/Trending Top Skills for Data Analysts in ASEAN Countries.png>)  
*Bar graph visualizing the trending top skills for data analysts in ASEAN countries in 2023.*

### Insights:
- SQL remains the most consistently demanded skill throughout the year, although it showed a noticeable massive decrease in September before exhibiting a massive spike in demand.
- Tableu's demand fluctates massively but stays consistently above certain levels.
- Both Python and Excel show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a decent upward trend starting from the latter half of the year but tapered off back to its initial numbers

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in ASEAN countries and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

View my notebook with detailed steps here: [4.Skills_Analysis_ASEAN](4.Skills_Analysis_ASEAN.ipynb)

### Visualizing the Data

```Python
sns.boxplot(data=df_jobs_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks = plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks)
plt.show()
```
### Results
![Salary Distributions of Data Jobs in ASEAN countries](<Images/Salary Distributions of Data Jobs in ASEAN countries.png>)  
*Box plot visualizing the salary distributions for the top 6 data job titles.*

### Insights

- There's a significant variation in salary ranges across different job titles. Data Scientist positions tend to have the highest salary potential, with up to $200K, indicating the high value placed on advanced data skills in the industry.

- Data Analyst is the only job position that has a significant outlier, indicating that the demands for these positions are relatively stable and salaries between companies are generally competitive.

- Specialized roles have higher upper quartile ranges but lower median salaries compare to less specialized ones. Both senior roles (Senior Data Engineer, Senior Data Analyst) have higher median salaries, but of the two roles, Senior Data Analyst remains stable, possibly occupying similar responsibilities and roles within the field across companies while Senior Data Engineer shows a larger interquartile, indicating variance in compensation as responsibilities increase.

## Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.


### Visualizing the Data

```python
fig, ax = plt.subplots(2,1)

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_ASEAN_top_pay, x='median', y='job_skills', ax=ax[0], hue='median', palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr'
sns.barplot(data=df_DA_ASEAN_top_skills, x='median', y='job_skills', ax=ax[1], hue='median', palette='dark:b_r')

plt.show()
```

### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in ASEAN countries:

![Highest Paid and Most In-Demand skills for Data Analysts in ASEAN countries](<Images/Highest Paid and Most In-Demand skills for Data Analysts in ASEAN countries.png>)    
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in ASEAN countries*

### Insights
- The top graph shows specialized technical skills like `golang`, `redshift`, and `kafka` are associated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.

- The bottom graph highlights that foundational skills like `Excel`, `Word`, and `SQL` are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.

- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn (the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn. 

View my notebook with detailed steps here: [5.Optimal_Skills_ASEAN](5.Optimal_Skills_ASEAN.ipynb)

### Visualizing the Data

```Python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```
### Results

![Most Optimal Skill for Data Analyst in ASEAN countries](<Images/Most Optimal Skill for Data Analyst in ASEAN countries.png>)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in ASEAN countries.*

### Insights

- The skill `Spark` appears to have the highest median salary of just over $130k, despite being less common in job postings. 

- The skill `SQL` and `Python` are the two most common listed in job postings followed by `Excel` and `Tableu`, all four also have similar median salary levels, indicating that a need for both specialized and common skills are generally required in the region.

## Visualizing Different Technologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

### Visualizing the Data

```Python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skillstech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()
```

### Results

![Most Optimal Skills for Data Analysts in ASEAN countries with Coloring by Technology .png](<Images/Most Optimal Skills for Data Analysts in ASEAN countries with Coloring by Technology .png>)    
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology.*

### Insights:

- The scatter plot shows that most of the `programming` skills (colored blue) tend to cluster at similar salary levels compared to other categories while also being the most prevalent, indicating that programming expertise could offer more stable salary benefits and employement within the data analytics field.

- The database skills (colored red), such as `SQL Server`, are associated with some of the lowest salaries among data analyst tools. This indicates a lack of demand and valuation for data management and manipulation expertise in the industry within the region.

- Analyst tools (colored orange), including `Tableau` and `Power BI`, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Spark can lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.