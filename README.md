## Netflix Movies and TV Shows Data Analysis using SQL
![Netflix logo](https://github.com/analyticsaq/Project_SQL_Netflix/blob/main/logo.webp)


## 📌 Overview

This project involves a comprehensive analysis of Netflix's movies and TV shows dataset using SQL. The goal is to extract actionable insights and answer key business questions related to content distribution, ratings, genres, and more.

## 🎯 Objectives

- Analyze the distribution of content types (Movies vs TV Shows)
- Identify the most common ratings for each type of content
- Analyze content by release years, countries, and durations
- Explore content by specific directors, actors, and keywords
- Categorize content and extract business-ready insights

## 📁 Dataset

- **Source:** [Kaggle - Netflix Movies and TV Shows Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows)
- **Format:** CSV
- **Size:** [Specify if known - e.g., "Contains X movies and Y TV shows"]
- **Note:** Data was cleaned and structured into a PostgreSQL-compatible schema

## 🧱 Database Schema

```sql
CREATE TABLE netflix (
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
🛠️ Installation & Setup
Clone the repository:

bash
git clone https://github.com/your-username/netflix-sql-analysis.git
cd netflix-sql-analysis
Import the dataset into PostgreSQL:

bash
psql -d your_database -f schema.sql
\copy netflix FROM 'netflix_titles.csv' DELIMITER ',' CSV HEADER;
🔍 SQL Analysis
The project includes analysis of various aspects of Netflix content:

Content Distribution
sql
## 📌 Overview

This project involves a comprehensive analysis of Netflix's movies and TV shows dataset using SQL. The goal is to extract actionable insights and answer key business questions related to content distribution, ratings, genres, and more.

## 🎯 Objectives

- Analyze the distribution of content types (Movies vs TV Shows)
- Identify the most common ratings for each type of content
- Analyze content by release years, countries, and durations
- Explore content by specific directors, actors, and keywords
- Categorize content and extract business-ready insights

## 📁 Dataset

- **Source:** [Kaggle - Netflix Movies and TV Shows Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows)
- **Format:** CSV
- **Size:** [Specify if known - e.g., "Contains X movies and Y TV shows"]
- **Note:** Data was cleaned and structured into a PostgreSQL-compatible schema

## 🧱 Database Schema

```sql
CREATE TABLE netflix (
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
🛠️ Installation & Setup
Clone the repository:

bash
git clone https://github.com/your-username/netflix-sql-analysis.git
cd netflix-sql-analysis
Import the dataset into PostgreSQL:

bash
psql -d your_database -f schema.sql
\copy netflix FROM 'netflix_titles.csv' DELIMITER ',' CSV HEADER;
🔍 SQL Analysis
The project includes analysis of various aspects of Netflix content:

Content Distribution
sql
-- Count of Movies vs TV Shows
SELECT type, COUNT(*) 
FROM netflix 
GROUP BY 1;
Rating Analysis
sql
-- Most common rating for each content type
WITH RatingCounts AS (
    SELECT type, rating, COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
)
-- [Full query in queries.sql]
Geographical Analysis
sql
-- Top 5 countries with the most content
SELECT * 
FROM (
    SELECT UNNEST(STRING_TO_ARRAY(country, ',')) AS country,
    COUNT(*) AS total_content
    FROM netflix
    GROUP BY 1
) AS t1
WHERE country IS NOT NULL
ORDER BY total_content DESC
LIMIT 5;
[Additional query categories...]
For all queries, see the queries.sql file.

📊 Key Findings
Content Distribution: [Brief summary of findings]

Rating Patterns: [Brief summary of findings]

Geographical Insights: The United States, India, and the United Kingdom produce the most content on Netflix

Temporal Trends: [Brief summary of release patterns over time]

Content Categories: Documentaries and International content dominate certain markets

📈 Visualizations
[Optional: Describe or link to any visualizations created from this data]

🚀 How to Use This Project
Examine the SQL queries in the queries.sql file

Run the queries in your PostgreSQL environment

Modify parameters (years, countries, etc.) to explore different aspects

Use the insights for:

Content strategy analysis

Market research

Academic learning of SQL

💡 Skills Demonstrated
SQL query writing and optimization

Data analysis and interpretation

PostgreSQL database management

Data cleaning and preparation

Business intelligence reporting

📚 Resources
PostgreSQL Documentation

Kaggle Netflix Dataset

SQL Style Guide
