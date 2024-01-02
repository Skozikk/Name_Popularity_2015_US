# Population analysis for 2015 US

### Poject Overview

This project aims to analyze and explore the unique baby name dataset provided by the Social Security Administration, covering the years 1980 to 2015. The analysis will focus on several key aspects of baby names, including ranking within specific years, the most popular names for each gender, dynamics of name changes over the years, and names that have consistently held high rankings over an extended period.

### Data Source:

The dataset used in this project is sourced from the Social Security Administration Baby Names dataset, covering the years 1980 to 2015. It includes information recorded for Social Security Number applications, categorized by sex, for each state and nationwide.

### Tools

- SQLite - Data Analysis [Download here](https://www.sqlite.org/index.html)

### Features and Analysis

#### Ranking of Unique Names in Specific Years:
```
WITH RankedNames AS (
  SELECT
    year,
    sex,
    name,
    RANK() OVER (PARTITION BY year, sex ORDER BY count DESC) AS name_rank
  FROM
    babynames
  WHERE
    year BETWEEN 1980 AND 2015
)
SELECT
  year,
  sex,
  name
FROM
  RankedNames
WHERE
  name_rank = 1;
```
### Trends in Changes in Name Popularity:

``` WITH NameTrends AS (
  SELECT
    name,
    year,
    RANK() OVER (PARTITION BY name ORDER BY year) AS name_rank
  FROM
    babynames
)

SELECT
  name,
  MIN(year) AS start_year,
  MAX(year) AS end_year,
  MAX(name_rank) - MIN(name_rank) AS rank_change
FROM
  NameTrends
GROUP BY
  name
HAVING
  MAX(name_rank) - MIN(name_rank) > 0
ORDER BY
  rank_change DESC;```
```
#### Comparison Popularity of Names for Both Sexes:
```
WITH CommonNames AS (
  SELECT
    name
  FROM
    babynames
  WHERE
    name IS NOT NULL
  GROUP BY
    name
  HAVING
    COUNT(DISTINCT sex) = 2
)

SELECT
  name
FROM
  CommonNames
ORDER BY
  name;
  ```

### ranking of names by gender

```
WITH HighRankingNames AS (
  SELECT
    name,
    sex,
    AVG(rank_within_sex) AS avg_rank
  FROM
    babynames
  GROUP BY
    name, sex
  HAVING
    COUNT(DISTINCT year) >= 5 
)

SELECT
  name,
  sex,
  AVG(avg_rank) AS overall_avg_rank
FROM
  HighRankingNames
GROUP BY
  name, sex
ORDER BY
  overall_avg_rank ASC;
```

| Name         | Sex | Overall Avg Rank |
|--------------|-----|------------------|
| Michael      | M   | 2.25             |
| Matthew      | M   | 5.72             |
| Joshua       | M   | 6.94             |
| Christopher  | M   | 7.67             |
| Daniel       | M   | 8.22             |
| Andrew       | M   | 11.28            |
| David        | M   | 11.44            |
| Joseph       | M   | 12.22            |
| William      | M   | 12.50            |
| Jacob        | M   | 12.67            |

| Name       | Sex | Overall Avg Rank |
|------------|-----|------------------|
| Emily      | F   | 9.22             |
| Elizabeth  | F   | 9.97             |
| Sarah      | F   | 13.64            |
| Ashley     | F   | 16.61            |
| Samantha   | F   | 16.97            |
| Lauren     | F   | 29.33            |
| Victoria   | F   | 34.22            |
| Jessica    | F   | 34.78            |
| Anna       | F   | 37.83            |
| Natalie    | F   | 39.81            |

For female names, there is greater variability in rankings, which may indicate a more diverse range of choices among girls. Female names seem to exhibit more fluctuation in rankings than male names, reflecting changing trends in name choices for girls. These are the most popular male and female names during the period 1980-2015 in the USA
### Annual Trends
```
WITH YearlyTrends AS (
  SELECT
    year,
    name,
    AVG(count) AS avg_count
  FROM
    babynames
  GROUP BY
    year, name
)

SELECT
  year,
  name,
  avg_count
FROM
  YearlyTrends
ORDER BY
  year, avg_count DESC;
```
| Year | Top 1      | Top 2      | Top 3        | Top 4      | Top 5      | Top 6       | Top 7    | Top 8    | Top 9    | Top 10      |
|------|------------|------------|--------------|------------|------------|-------------|----------|----------|----------|-------------|
| 1980 | Michael    | Jennifer   | Christopher  | Jason      | David      | James       | Matthew  | Joshua   | Amanda   | John        |
| 1981 | Michael    | Jennifer   | Christopher  | Matthew    | Jessica    | Jason       | David    | Joshua   | James    | John        |
| 1982 | Michael    | Christopher | Jennifer   | Matthew    | Jessica    | Jason       | David    | James    | Joshua   | John        |
| 1983 | Michael    | Christopher | Jennifer   | Matthew    | Jessica    | David       | Joshua   | James    | Jason   | Daniel      |
| 1984 | Michael    | Christopher | Jennifer   | Matthew    | Jessica    | Joshua      | Ashley   | David    | Daniel   | James       |
| 1985 | Michael    | Christopher | Jessica     | Ashley     | Matthew    | Jennifer    | Joshua   | Amanda   | Daniel   | David       |
| 1986 | Michael    | Christopher | Jessica     | Ashley     | Matthew    | Amanda      | Joshua   | David    | Daniel   | Jennifer    |
| 1987 | Michael    | Jessica    | Ashley       | Christopher | Matthew    | Amanda      | Joshua   | David    | Andrew   | Daniel      |
| 1988 | Michael    | Christopher | Jessica     | Ashley     | Matthew    | Joshua      | Amanda   | Andrew   | David    | Justin      |
| 1989 | Michael    | Christopher | Jessica     | Ashley     | Matthew    | Joshua      | Brittany | Amanda   | David    | Daniel      |
| 1990 | Michael    | Christopher | Jessica     | Ashley     | Matthew    | Joshua      | Brittany | Amanda   | Daniel   | David       |
| 1991 | Michael    | Christopher | Ashley      | Jessica    | Matthew    | Joshua      | Andrew  | Daniel   | James   | David       |
| 1992 | Michael    | Christopher | Ashley      | Jessica    | Matthew    | Joshua      | Andrew  | Tyler    | Brandon  | Daniel      |
| 1993 | Michael    | Christopher | Matthew     | Jessica    | Ashley     | Joshua      | Tyler   | Taylor   | Brandon  | Daniel      |
| 1994 | Michael    | Christopher | Matthew     | Jessica    | Tyler      | Joshua      | Ashley  | Brandon  | Jacob    | Daniel      |
| 1995 | Michael    | Matthew    | Christopher  | Jacob      | Joshua     | Tyler       | Nicholas | Jessica  | Brandon  | Daniel      |
| 1996 | Michael    | Matthew    | Jacob       | Christopher | Joshua    | Tyler       | Nicholas | Brandon  | Austin  | Andrew      |
| 1997 | Michael    | Jacob      | Matthew      | Christopher | Joshua    | Nicholas    | Emily    | Tyler   | Brandon  | Austin      |
| 1998 | Michael    | Jacob      | Matthew      | Joshua     | Christopher | Nicholas   | Emily    | Tyler   | Brandon  | Andrew      |
| 1999 | Jacob      | Michael    | Matthew      | Joshua     | Emily      | Nicholas    | Christopher | Andrew | Tyler   | Joseph      |
| 2000 | Jacob      | Michael    | Matthew      | Joshua     | Emily      | Christopher | Nicholas | Andrew  | Hannah  | Joseph      |
| 2001 | Jacob      | Michael    | Matthew      | Joshua     | Emily      | Christopher | Nicholas | Andrew  | Madison | Joseph      |
| 2002 | Jacob      | Michael    | Joshua       | Matthew    | Emily      | Ethan       | Andrew  | Joseph  | Madison | Christopher |
| 2003 | Jacob      | Michael    | Emily        | Joshua     | Matthew    | Emma        | Andrew  | Ethan   | Joseph  | Daniel      |
| 2004 | Jacob      | Michael    | Emily        | Joshua     | Matthew    | Ethan       | Andrew  | Emma    | Daniel  | Madison     |
| 2005 | Jacob      | Emily      | Michael      | Joshua     | Matthew    | Ethan       | Andrew  | Emma    | Daniel  | Madison     |
| 2006 | Jacob      | Michael    | Joshua       | Emily      | Ethan      | Matthew     | Daniel  | Andrew  | Christopher | Anthony   |
| 2007 | Jacob      | Michael    | Ethan        | Joshua     | Daniel     | Christopher | Anthony | Emily   | Isabella | William    |
| 2008 | Jacob      | Michael    | Ethan        | Joshua     | Daniel     | Jayden      | Emma    | Alexander | Isabella | William    |
| 2009 | Isabella   | Jacob      | Ethan        | Jayden     | Michael    | Alexander   | William | Emma    | Joshua  | Daniel      |
| 2010 | Isabella   | Jacob      | Sophia       | Jayden     | Ethan      | Michael     | Emma    | William | Olivia  | Alexander   |
| 2011 | Sophia     | Jacob      | Isabella     | Mason      | Emma       | Jayden      | William | Olivia  | Noah    | Michael     |
| 2012 | Sophia     | Emma       | Isabella     | Jacob      | Mason      | Ethan       | Noah    | Olivia  | Jayden  | William     |
| 2013 | Sophia     | Emma       | Olivia       | Noah       | Liam       | Jacob       | Mason   | Isabella | William | Ethan       |
| 2014 | Emma       | Olivia     | Noah         | Sophia     | Liam       | Mason       | Isabella | Jacob   | William | Ethan       |
| 2015 | Emma       | Noah       | Amelia       | Olivia     | Liam       | Sophia      | Mason   | Ava     | Jacob   | William     |

Consistent Popularity of Certain Names: Names such as "Michael" for boys and "Jennifer" for girls maintained high positions on the list for many years.

Variable Trends: Other names, such as "Jessica," "Ashley," "Christopher," or "Emma," seem to exhibit variable trends over the years, gaining higher or lower positions on the list at different times.

Changes in Recent Years: In recent years (2010-2015), names like "Sophia," "Olivia," or "Noah" gained popularity, aligning with the prevailing trends during that period.

Universality of Certain Names: Some names, such as "William," "Emily," or "Alexander," consistently hold high positions on the list for both boys and girls.

## Inspiration

