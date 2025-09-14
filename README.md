## Netflix Movies and TV Shows Data Analysis using SQL
![Netflix logo](https://github.com/analyticsaq/Project_SQL_Netflix/blob/main/logo.webp)


## 📌 Overview

This project involves a comprehensive analysis of Netflix's movies and TV shows dataset using SQL. The goal is to extract actionable insights and answer key business questions related to content distribution, ratings, genres, and more.

This README outlines the project’s objectives, business questions, SQL solutions, and key findings.

---

## 🎯 Objectives

- Analyze the distribution of content types (Movies vs TV Shows)
- Identify the most common ratings for each type of content
- Analyze content by release years, countries, and durations
- Explore content by specific directors, actors, and keywords
- Categorize content and extract business-ready insights

---

## 📁 Dataset

- Source: Kaggle - Netflix Movies and TV Shows Dataset
- Format: CSV
- Cleaned and structured into a PostgreSQL-compatible schema

---

## 🧱 Table Schema

```sql
DROP TABLE IF EXISTS netflix;
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
```

---

## 🧠 Business Questions & SQL Solutions

### 1️⃣ Count the Number of Movies vs TV Shows

```sql
SELECT 
    type,
    COUNT(*) 
FROM netflix 
GROUP BY 1;
```

---

### 2️⃣ Most Common Rating for Each Content Type

```sql
WITH RatingCounts AS (
    SELECT type, rating, COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT *, RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
    FROM RatingCounts
)
SELECT type, rating AS most_frequent_rating
FROM RankedRatings
WHERE rank = 1;
```

---

### 3️⃣ Movies Released in a Specific Year (e.g., 2020)

```sql
SELECT * 
FROM netflix 
WHERE release_year = 2020;
```

---

### 4️⃣ Top 5 Countries with the Most Content

```sql
SELECT * 
FROM (
    SELECT 
        UNNEST(STRING_TO_ARRAY(country, ',')) AS country,
        COUNT(*) AS total_content
    FROM netflix
    GROUP BY 1
) AS t1
WHERE country IS NOT NULL
ORDER BY total_content DESC
LIMIT 5;
```

---

### 5️⃣ Longest Movie on Netflix

```sql
SELECT * 
FROM netflix 
WHERE type = 'Movie' 
ORDER BY SPLIT_PART(duration, ' ', 1)::INT DESC;
```

---

### 6️⃣ Content Added in the Last 5 Years

```sql
SELECT * 
FROM netflix 
WHERE TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years';
```

---

### 7️⃣ All Content by Director 'Rajiv Chilaka'

```sql
SELECT * 
FROM (
    SELECT *, UNNEST(STRING_TO_ARRAY(director, ',')) AS director_name
    FROM netflix
) AS t 
WHERE director_name = 'Rajiv Chilaka';
```

---

### 8️⃣ TV Shows with More Than 5 Seasons

```sql
SELECT * 
FROM netflix 
WHERE type = 'TV Show' AND SPLIT_PART(duration, ' ', 1)::INT > 5;
```

---

### 9️⃣ Number of Content Items per Genre

```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(listed_in, ',')) AS genre,
    COUNT(*) AS total_content
FROM netflix
GROUP BY 1;
```

---

### 🔟 Top 5 Years with Highest Average Content Released in India

```sql
SELECT 
    country,
    release_year,
    COUNT(show_id) AS total_release,
    ROUND(
        COUNT(show_id)::numeric /
        (SELECT COUNT(show_id) FROM netflix WHERE country = 'India')::numeric * 100, 2
    ) AS avg_release
FROM netflix
WHERE country = 'India'
GROUP BY country, release_year
ORDER BY avg_release DESC
LIMIT 5;
```

---

### 1️⃣1️⃣ Movies That Are Documentaries

```sql
SELECT * 
FROM netflix 
WHERE listed_in LIKE '%Documentaries';
```

---

### 1️⃣2️⃣ Content Without a Director

```sql
SELECT * 
FROM netflix 
WHERE director IS NULL;
```

---

### 1️⃣3️⃣ Movies Featuring 'Salman Khan' in the Last 10 Years

```sql
SELECT * 
FROM netflix 
WHERE casts LIKE '%Salman Khan%' 
  AND release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;
```

---

### 1️⃣4️⃣ Top 10 Actors in Indian Content

```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(casts, ',')) AS actor,
    COUNT(*)
FROM netflix
WHERE country = 'India'
GROUP BY actor
ORDER BY COUNT(*) DESC
LIMIT 10;
```

---

### 1️⃣5️⃣ Categorize Content Based on Keywords ('kill' or 'violence')

```sql
SELECT 
    category,
    COUNT(*) AS content_count
FROM (
    SELECT 
        CASE 
            WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Bad'
            ELSE 'Good'
        END AS category
    FROM netflix
) AS categorized_content
GROUP BY category;
```

---

## 📌 Findings & Conclusions

- **Content Type Distribution:** Netflix features a diverse mix of Movies and TV Shows.
- **Popular Ratings:** Common ratings highlight the platform’s family-oriented and adult content segments.
- **Geographical Insights:** U.S. and India dominate content production, with others following closely.
- **Keyword-Based Categorization:** Helps identify content tone and audience type.
- **Director/Actor-Based Filters:** Useful for niche recommendations and profiling.

---

## 🚀 Final Thoughts

This SQL-based exploratory data analysis offers a strategic overview of Netflix’s content library. These insights can help content strategists, analysts, and business stakeholders make informed decisions around content curation, user targeting, and market focus.
