# SQL Review
Contents:
1. Mode Analytics Qs 
2. 50 Qs to solve
3. Optimization
4. Schema Design 
5. Interview Questions - 10

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

