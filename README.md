# Overview

Welcome to my analysis of the data job market in Poland, focusing on data analyst roles. This is a Capstone project for a python Data Analytics course I took - [Luke Barousse's Python Course](https://lukebarousse.com/python). This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts. 

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter for Jobs in Poland

To focus my analysis on the polish job market, I apply filters to the dataset, narrowing down to roles based in Poland, the dataset contains data on 160 countries.

```python
df_PL = df[df['job_country'] == 'Poland']
```

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2_Skills_Count](2_Skills_Count.ipynb).

### Visualize Data

fig, ax = plt.subplots(len(job_titles), 1)

```python
for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)

    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Skills Requested in the Polish Job Postings', fontsize=15)
fig.tight_layout(h_pad=.8)
plt.show()
```

### Results

![Likelihood of Skills Requested in the Polish Job Postings](images/likelihood_of_skills_1st_question.png)

*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights:

- For Data Engineers and Data Scientists, Python is the top-requested skill, appearing in 63.28% and 60.48% of their respective job postings.

- For Data Analysts, SQL is the most important skill, required in nearly half (49.56%) of all postings.

- Data Engineers require the most highly specialized, infrastructure-related technologies. Cloud platforms like Azure (41.05%) and AWS (35.34%), along with the big data processing tool Spark (35.31%), all feature prominently in over a third of their postings.

- Data Analysts are primarily expected to be proficient in general business intelligence and data visualization tools, with Excel (37.45%) and Tableau (21.66%) being top requirements, reflecting a focus on reporting and accessibility.


- Python serves as the central programming language for the data industry, though its required frequency varies significantly by role: It is most critically required by Data Engineers (63.28%) and Data Scientists (60.48%), where it is the leading skill used for building models and data pipelines. While it is less frequent for Data Analysts, it remains the third most requested skill (29.89%), highlighting its growing utility in data manipulation and scripting for analysis within that role.

## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](3_Skills_Trend.ipynb).

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_PL_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove top and right spines

plt.title('Trending Top Skills for Data Analysts in Poland')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

# annotate the plot with the top 5 skills using plt.text()
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.show()
```

### Results

![Trending Top Skills for Data Analysts in Poland](images/Trending_Top_Skills_for_Data_Analysts_in_Poland.png)  
*Bar graph visualizing the trending top skills for data analysts in Poland in 2023.*

### Insights:

- SQL was the most consistently demanded skill throughout 2023, often appearing in over 70% of job postings and peaking near 85% in July and October. However, its demand was highly volatile, dropping sharply at the end of the year to below 60%.
- Tableau experienced a massive mid-year surge, climbing from around 45% to a high of nearly 75% in September before sharply declining to about 30% by December. Python maintained strong, consistent relevance in the mid-range (fluctuating between 30% and 62%), also ending the year around 30% demand, tying with Tableau.
- Power BI was consistently the least requested skill, generally remaining below 35% and showing the sharpest drop at year-end, falling to below 10% likelihood in December job postings.
- Core Insight: While SQL is the fundamental requirement, Python and Tableau defined the competitive middle ground for Data Analyst skills in 2023, though all five skills saw a notable decrease in demand by year's end.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I got jobs in Poland and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

View my notebook with detailed steps here: [4_Salary_Analysis](4_Salary_Analysis.ipynb).

#### Visualize Data 

```python
sns.boxplot(data=df_PL_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

# this is all the same
plt.title('Salary Distributions of Data Jobs in Poland')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

![Salary Distributions of Data Jobs in Poland](images/Salary_Distributions_of_Data_Jobs_Poland.png)  
*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights

- All data roles, particularly the Senior and Scientist/Engineer tracks, show a large number of outliers (individual circles) extending far past the upper quartile.

- The highest salaries are concentrated in the Data Scientist and Senior Data Scientist roles, which feature outliers extending all the way to $600K USD, indicating exceptional earning potential at the top end of the market.

- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.


### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualize Data

```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_PL_top_pay, x='median', y=df_PL_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
# original code:
# df_PL_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
ax[0].set_title('Highest Paid Skills for Data Analysts in Poland')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_PL_skills, x='median', y=df_PL_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# original code:
# df_PL_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)
ax[1].set_title('Most In-Demand Skills for Data Analysts in Poland')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.show()

```

#### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in Poland:

![The Highest Paid & Most In-Demand Skills for Data Analysts in Poland](images/Highest_Paid_and_Most_In_Demand_Skills_for_Data_Analysts_in_Poland.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in Poland.*

#### Insights:

-  High-paying skills are niche, technical, and infrastructure-related.
Skills like dplyr, bitbucket, gitlab, solidity, hugging face, couchbase, ansible, cassandra, mxnet have the highest salaries but are not common general-purpose data-analysis tools. These reflect either
	-	specialized ecosystems (Solidity → blockchain, HuggingFace → NLP), or
	-	infrastructure/devops/ML tooling (Ansible, Couchbase, GitLab).

They pay well because few candidates have them, not because most analysts use them.

- The most in-demand skills are general-purpose analytics tools.
Employers most frequently request Python, Tableau, R, SQL Server, SQL, SAS, Power BI, Excel, Word, PowerPoint.
These are bread-and-butter analyst skills, widely required and broadly used across industries.

- There is a clear split between “high-pay” and “high-demand.”
	-	High-pay skills reward specialization and scarcity.
	-	High-demand skills reward versatility and day-to-day business analytics.

- Python dominates demand, not salary.
Python tops the demand chart, meaning it’s essential, but not rare enough to command extreme salaries.

# What I Learned

Working on this project gave me a clearer understanding of the data analyst job market in Poland and strengthened my practical skills in Python. Several points stood out:

- **Advanced Python Workflows**: I improved at using Pandas for data manipulation and Matplotlib/Seaborn for creating clear, informative visualizations.
- **Critical Role of Data Cleaning**: I saw firsthand how essential it is to clean and structure data properly before any analysis. The quality of insights depends directly on the quality of the dataset.
- **Skill Landscape Awareness**: Investigating how different skills relate to pay and job frequency highlighted how unevenly the market values various technical competencies.

# Insights

The analysis revealed several patterns within the Polish data analyst job market:

- **Mismatch Between Pay and Popularity**: The skills with the highest salaries in Poland tend to be niche, technical, or infrastructure-focused, whereas the skills most commonly requested by employers are general-purpose analytical tools.
- **Specialization Pays, General Skills Secure Jobs**: Broad analytical tools like Python, SQL, and Tableau dominate demand, while more specialized or less common tools are associated with higher compensation.
- **Dynamic Market Structure**: The distribution of skill demand across tools and technologies shows that the field is continuously shifting, which reinforces the need to monitor emerging trends.

# Challenges I Faced

Several aspects of the project required careful problem-solving:

- **Inconsistent or Sparse Data**: Some variables contained unexpected distributions or missing categories, which required additional checking and cleaning to avoid misleading results.
- **Visualizing Uneven Patterns**: Presenting skewed or highly imbalanced data in a clear way demanded more deliberate choices in visualization design.
- **Scope Management**: Deciding how much detail to include in each part of the analysis while keeping the project coherent required constant adjustment.

# Conclusion

This project provided a grounded look at the data analyst landscape in Poland and helped clarify which skills appear most valuable from both salary and demand perspectives. It strengthened my technical workflow and offered a structured approach to evaluating job-market data. The experience forms a useful foundation for future analytical work and reinforces the importance of continuous exploration in a rapidly changing field.
