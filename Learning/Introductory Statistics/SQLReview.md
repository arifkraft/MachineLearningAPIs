# SQL Review
Contents:
1. Mode Analytics Qs 
1. Optimizing SQL Queries 

## 1. Mode Analytics Problems
1.1 Write a query that returns songs that ranked between 10 and 20 (inclusive) in 1993, 2003, or 2013. Order the results by year and rank, and leave a comment on each line of the WHERE clause to indicate what that line does

```sql
SELECT year, year_rank, artist, 
       row_number() over(partition by year_rank order by year_rank) as row_num
  FROM tutorial.billboard_top_100_year_end
  where year in (1993, 2003, 2013)
  and (year_rank>= 10 and year_rank<= 20)
  LIMIT 100;

```
1.2 Write a query that shows the number of players at schools with names that start with A through M, and the number at schools with names starting with N - Z.

```sql 
SELECT case when lower(school_name) < 'n' then 'a-m'
            when lower(school_name) >= 'n' then 'n-z'
            else NULL end as school_alpha_start,
            count(player_name)
  FROM benn.college_football_players
  group by 1
  order by 1;
```

1.3 Count the number of unique companies (don't double-count companies) and unique acquired companies by state. Do not include results for which there is no state data, and order by the number of acquired companies from highest to lowest.

```sql
SELECT companies.state_code,
       COUNT(DISTINCT companies.permalink) AS unique_companies,
       COUNT(DISTINCT acquisitions.company_permalink) AS unique_companies_acquired
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
    ON companies.permalink = acquisitions.company_permalink
 WHERE companies.state_code IS NOT NULL
 GROUP BY 1
 ORDER BY 3 DESC;
```

1.4 Write a query that counts the number of companies acquired within 3 years, 5 years, and 10 years of being founded (in 3 separate columns). Include a column for total companies acquired as well.

```sql
select acq.acquirer_city,
       count(acq.company_permalink) as total,
       count(case when acq.acquired_at_cleaned <= com.founded_at_clean::timestamp 
             + INTERVAL '3 years' then 1 else NULL end) as before_3,
       count(case when acq.acquired_at_cleaned <= com.founded_at_clean::timestamp
             + INTERVAL '5 years' then 1 else NULL end) as before_5,
       count(case when acq.acquired_at_cleaned <= com.founded_at_clean::timestamp
             + INTERVAL '10 years' then 1 else NULL end) as before_10
from  tutorial.crunchbase_acquisitions_clean_date as acq
left join tutorial.crunchbase_companies_clean_date as com
on acq.company_permalink = com.permalink
group by 1
having acquirer_city is not null
order by 2 desc;
```

1.5 You can use EXTRACT to pull the pieces apart one-by-one

```sql
SELECT cleaned_date,
       EXTRACT('year' FROM NOW()) AS years,
       EXTRACT('year'   FROM cleaned_date) AS year,
       EXTRACT('month'  FROM cleaned_date) AS month,
       EXTRACT('day'    FROM cleaned_date) AS day,
       EXTRACT('hour'   FROM cleaned_date) AS hour,
       EXTRACT('minute' FROM cleaned_date) AS minute,
       EXTRACT('second' FROM cleaned_date) AS second,
       EXTRACT('decade' FROM cleaned_date) AS decade,
       EXTRACT('dow'    FROM cleaned_date) AS day_of_week
  FROM tutorial.sf_crime_incidents_cleandate;

```


1.6 Write a query that shows a running total of the duration of bike rides (similar to the last example), but grouped by end_terminal, and with ride duration sorted in descending order.

```sql
select end_terminal,
       sum(duration_seconds) over(partition by end_terminal order by duration_seconds desc) as running_total
from tutorial.dc_bikeshare_q1_2012;
```

1.7 Add row number without having to use partition by.

```sql
SELECT start_terminal,
       start_time,
       duration_seconds,
       ROW_NUMBER() over(order by (select 1))as row
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'

```

1.8 Write a query that shows the 5 longest rides from each starting terminal, ordered by terminal, and longest to shortest rides within each terminal. Limit to rides that occurred before Jan. 8, 2012.

```sql
select start_terminal,
       duration_seconds,
       rank
from
(
select start_terminal,
       duration_seconds,
       dense_rank() over(partition by start_terminal order by duration_seconds desc) as rank,
       row_number() over(partition by start_terminal order by duration_seconds desc) as rownum
from tutorial.dc_bikeshare_q1_2012
where start_time < '2012-01-08'
) a
where rownum<6
order by start_terminal;
```

1.9 Write a query that shows only the duration of the trip and the percentile into which that duration falls (across the entire dataset—not partitioned by terminal).

```sql
select duration_seconds,
       ntile(100) over(order by duration_seconds) as Percentile
from tutorial.dc_bikeshare_q1_2012;

```

## 2. Optimizing SQL queries

1. Define Business Requirements before Beginning
1. Define SELECT Fields instead of SELECT *
1. Select More Fields to Avoid SELECT DISTINCT
1. Create Joins with INNER JOIN Rather than WHERE
1. Use WHERE instead of HAVING to Define Filters
1. Use Wildcards at the End of a Phrase Only
1. Avoid using functions in predicates when possible
1. Use inner join, instead of outer join if possible.
1. DISTINCT and UNION should be used only if it is necessary.
1. Push predicates into the OUTER JOIN clause whenever possible
1. Duplicate constant condition for different tables whenever possible

## 3. Interview Questions

1. Find the 5th highest weight.

```sql
select top 1 weight
from
(
select distinct top 2 weight
from benn.college_football_players 
order by weight desc;
)a
order by weight;
```

2.Query to find duplicate rows in table?(click here for explanation)

```sql
Select a,b,c,d
From table
Group by 1,2,3,4
Having count(1)>1;
```

3. What is the Query to fetch first record from Employee table?
```
Select top 1
From employee;
```

4. What is the Query to fetch last record from the table?

```sql
Select * 
From employee 
Where rowid = max(rowid)
```

5. How to fetch 3rd highest salary using Rank Function?

```sql
select * from
(
Select *, dense_rank() over(order by salary) as rank
)a
Where rank=3;
```

6. Display first 50% records from Employee table?

```sql
select * from
(
(Select *, row_number() over(order by (select 1) as row_num
)a
Where rownum>(count(*)/2)
```

7. How do i fetch only common records between 2 tables.

```sql
Select * from Employee;

Intersect

Select * from Employee1;
```






