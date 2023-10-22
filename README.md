# Analyze-International-Debt-Statistics

![Dollar](https://github.com/aliabdulelah/Analyze-International-Debt-Statistics/assets/129835709/ab2ba77b-cb12-4269-8c69-1c59ec60ea19)

## Table of contents 
- [Project Overview](#project-Overview)
- [Data Source](#Data-Source)
- [Tools](#Tools)
- [ Data Cleaning / Preparation](#Data-Cleaning-/-Preparation)
- [Exploratory Data Analysis](#Exploratory-Data-Analysis)
- [Results/Findings](#Data-Analysis-Results-Findings)



## Project Overview

It's not that we humans only take debts to manage our necessities. A country may also take debt to manage its economy. For example, infrastructure spending is one costly ingredient required for a country's citizens to lead comfortable lives. The World Bank is the organization that provides debt to countries.

In this project, you are going to analyze international debt data collected by The World Bank. The dataset contains information about the amount of debt (in USD) owed by developing countries across several categories. You are going to find the answers to questions like:

1- What is the total amount of debt that is owed by the countries listed in the dataset?
2- Which country owns the maximum amount of debt and what does that amount look like?
3- What is the average amount of debt owed by countries across different debt indicators?


### Data Source
 The World Bank is the organization that provides debt to countries. The data used in this project is provided by The World Bank. It contains both national and regional debt statistics for several countries across the globe as recorded from 1970 to 2015.
[Download here AS CSV file](https://docs.google.com/spreadsheets/d/1CHXLssj9_IiVE-nFK8NslvwJSTJ78pBPbzEkgdh1Edg/edit?usp=sharing)



### Tools 
- PostgreSQL - Data Cleaning
- PostgreSQL - Data Analysis




### Data Cleaning / Preparation 

 In the initial data preparation phase, we performed the following tasks:

- Data loading and inspection.
- Handling missing values.
- Data cleaning and formatting.




### Exploratory Data Analysis
 EDA involves exploring the American baby name data to answer key questions, such as:

1. Finding the number of distinct countries
2. Finding out the distinct debt indicators
3. Totaling the amount of debt owed by the countries
4. Country with the highest debt
5. Average amount of debt across indicators
6. The highest amount of principal repayments
7. The most common debt indicator
8. Other viable debt issues and conclusion




Firstly we need to have a look on table information :


```sql
 SELECT *
    FROM international_debt
    LIMIT 10;
```



| country_name | country_code | indicator_name                                              | indicator_code     | debt               |
| ------------ | ------------ | ---------------------------------------------------------- | ------------------ | ------------------ |
| Afghanistan  | AFG          | Disbursements on external debt, long-term (DIS, current US$) | DT.DIS.DLXF.CD      | 72894453.700000003 |
| Afghanistan  | AFG          | Interest payments on external debt, long-term (INT, current US$) | DT.INT.DLXF.CD      | 53239440.100000001 |
| Afghanistan  | AFG          | PPG, bilateral (AMT, current US$)                          | DT.AMT.BLAT.CD     | 61739336.899999999 |
| Afghanistan  | AFG          | PPG, bilateral (DIS, current US$)                           | DT.DIS.BLAT.CD     | 49114729.399999999 |
| Afghanistan  | AFG          | PPG, bilateral (INT, current US$)                           | DT.INT.BLAT.CD     | 39903620.100000001 |
| Afghanistan  | AFG          | PPG, multilateral (AMT, current US$)                       | DT.AMT.MLAT.CD      | 39107845           |
| Afghanistan  | AFG          | PPG, multilateral (DIS, current US$)                       | DT.DIS.MLAT.CD      | 23779724.300000001 |
| Afghanistan  | AFG          | PPG, multilateral (INT, current US$)                       | DT.INT.MLAT.CD      | 13335820           |
| Afghanistan  | AFG          | PPG, official creditors (AMT, current US$)                 | DT.AMT.OFFT.CD     | 100847181.900000006 |
| Afghanistan  | AFG          | PPG, official creditors (DIS, current US$)                 | DT.DIS.OFFT.CD     | 72894453.700000003 |

From the first ten rows, we can see the amount of debt owed by Afghanistan in the different debt indicators. But we do not know the number of different countries we have on the table. There are repetitions in the country names because a country is most likely to have debt in more than one debt indicator.





### Data Analysis Results Findings

1- Finding the number of distinct countries

Without a count of unique countries, we will not be able to perform our statistical analyses holistically. In this section, we are going to extract the number of unique countries present in the table.


```sql
SELECT 
    COUNT(DISTINCT country_name ) AS total_distinct_countries
FROM international_debt;
```


| total_distinct_countries |
| ------------------------ |
|           124            |





2- Finding out the distinct debt indicators







