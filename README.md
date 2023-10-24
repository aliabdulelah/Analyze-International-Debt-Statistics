# Analyze-International-Debt-Statistics

![Dollar](https://github.com/aliabdulelah/Analyze-International-Debt-Statistics/assets/129835709/ab2ba77b-cb12-4269-8c69-1c59ec60ea19)


<br>
<br>
<br>
<br>


## Table of contents 
- [Project Overview](#project-Overview)
- [Data Source](#Data-Source)
- [Tools](#Tools)
- [ Data Cleaning / Preparation](#Data-Cleaning-Preparation)
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
- Excel - Data Cleaning
- PostgreSQL - Data Analysis

<br>
<br>
<br>





### Data Cleaning Preparation 

 In the initial data preparation phase, we performed the following tasks:

- Data loading and inspection.
- Handling missing values.
- Data cleaning and formatting.



<br>
<br>
<br>


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




<br>
<br>
<br>



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




<br>
<br>
<br>
<br>


2- Finding out the distinct debt indicators

We can see there are a total of 124 countries present on the table. As we saw in the first section, there is a column called indicator_name that briefly specifies the purpose of taking the debt. Just beside that column, there is another column called indicator_code which symbolizes the category of these debts. Knowing about these various debt indicators will help us to understand the areas in which a country can possibly be indebted to.


```sql
SELECT DISTINCT indicator_code AS distinct_debt_indicators
FROM international_debt
ORDER BY distinct_debt_indicators

```


| distinct_debt_indicators |
| ------------------------ |
| DT.AMT.BLAT.CD           |
| DT.AMT.DLXF.CD           |
| DT.AMT.DPNG.CD           |
| DT.AMT.MLAT.CD           |
| DT.AMT.OFFT.CD           |
| DT.AMT.PBND.CD           |
| DT.AMT.PCBK.CD           |
| DT.AMT.PROP.CD           |
| DT.AMT.PRVT.CD           |
| DT.DIS.BLAT.CD           |
| DT.DIS.DLXF.CD           |
| DT.DIS.MLAT.CD           |
| DT.DIS.OFFT.CD           |
| DT.DIS.PCBK.CD           |
| DT.DIS.PROP.CD           |
| DT.DIS.PRVT.CD           |
| DT.INT.BLAT.CD           |
| DT.INT.DLXF.CD           |
| DT.INT.DPNG.CD           |
| DT.INT.MLAT.CD           |
| DT.INT.OFFT.CD           |
| DT.INT.PBND.CD           |
| DT.INT.PCBK.CD           |
| DT.INT.PROP.CD           |
| DT.INT.PRVT.CD           |




<br>
<br>
<br>
<br>

3- Totaling the amount of debt owed by the countries

As mentioned earlier, the financial debt of a particular country represents its economic state. But if we were to project this on an overall global scale, how will we approach it?

Let's switch gears from the debt indicators now and find out the total amount of debt (in USD) that is owed by the different countries. This will give us a sense of how the overall economy of the entire world is holding up.

```sql
SELECT 
    ROUND(SUM(debt)/ 1000000, 2)  AS total_debt
FROM international_debt; 
```


| total_debt |
| ---------- |
| 3079734.49 |



<br>
<br>
<br>
<br>


4- Country with the highest debt

"Human beings cannot comprehend very large or very small numbers. It would be useful for us to acknowledge that fact." - Daniel Kahneman. That is more than 3 million million USD, an amount which is really hard for us to fathom.

Now that we have the exact total of the amounts of debt owed by several countries, let's now find out the country that owns the highest amount of debt along with the amount. Note that this debt is the sum of different debts owed by a country across several categories. This will help to understand more about the country in terms of its socio-economic scenarios. We can also find out the category in which the country owns its highest debt. But we will leave that for now.



```sql
SELECT 
    country_name, 
    SUM(debt) AS total_debt
FROM international_debt
GROUP BY country_name
ORDER BY total_debt DESC
LIMIT 1
```


| country_name | total_debt       |
| ------------ | ---------------- |
| China        | 285793494734.20  |




<br>
<br>
<br>
<br>



5-Average amount of debt across indicators

We now have a brief overview of the dataset and a few of its summary statistics. We already have an idea of the different debt indicators in which the countries owe their debts. We can dig even further to find out on an average how much debt a country owes? This will give us a better sense of the distribution of the amount of debt across different indicators.



```sql

SELECT 
    indicator_code AS debt_indicator,
    indicator_name,
    AVG(debt) AS average_debt
FROM international_debt
GROUP BY debt_indicator,indicator_name
ORDER BY average_debt DESC
LIMIT 10;

```

| debt_indicator | indicator_name                                                                                                  | average_debt            |
| -------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------- |
| DT.AMT.DLXF.CD | Principal repayments on external debt, long-term (AMT, current US$)                                             | 5904868401.499193612     |
| DT.AMT.DPNG.CD | Principal repayments on external debt, private nonguaranteed (PNG) (AMT, current US$)                          | 5161194333.812658349     |
| DT.DIS.DLXF.CD | Disbursements on external debt, long-term (DIS, current US$)                                                   | 2152041216.890243888     |
| DT.DIS.OFFT.CD | PPG, official creditors (DIS, current US$)                                                                     | 1958983452.859836046     |
| DT.AMT.PRVT.CD | PPG, private creditors (AMT, current US$)                                                                      | 1803694101.963265321     |
| DT.INT.DLXF.CD | Interest payments on external debt, long-term (INT, current US$)                                               | 1644024067.650806481     |
| DT.DIS.BLAT.CD | PPG, bilateral (DIS, current US$)                                                                              | 1223139290.398230108     |
| DT.INT.DPNG.CD | Interest payments on external debt, private nonguaranteed (PNG) (INT, current US$)                             | 1220410844.421518983     |
| DT.AMT.OFFT.CD | PPG, official creditors (AMT, current US$)                                                                     | 1191187963.083064523     |
| DT.AMT.PBND.CD | PPG, bonds (AMT, current US$)                                                                                  | 1082623947.653623188     |



<br>
<br>
<br>
<br>


6-The highest amount of principal repayments

We can see that the indicator DT.AMT.DLXF.CD tops the chart of average debt. This category includes repayment of long term debts. Countries take on long-term debt to acquire immediate capital. 

An interesting observation in the above finding is that there is a huge difference in the amounts of the indicators after the second one. This indicates that the first two indicators might be the most severe categories in which the countries owe their debts.

We can investigate this a bit more so as to find out which country owes the highest amount of debt in the category of long term debts (DT.AMT.DLXF.CD). Since not all countries suffer from the same kind of economic disturbances, this finding will allow us to understand that particular country's economic condition a bit more specifically.



```sql
SELECT 
     country_name,
    indicator_name
FROM international_debt
WHERE debt = (SELECT 
                 MAX(debt)
             FROM international_debt
             WHERE indicator_code = 'DT.AMT.DLXF.CD');

```


| country_name | indicator_name                                                |
| ------------ | ------------------------------------------------------------ |
| China        | Principal repayments on external debt, long-term (AMT, current US$) |



<br>
<br>
<br>
<br>



7- The most common debt indicator
China has the highest amount of debt in the long-term debt (DT.AMT.DLXF.CD) category. It is often a good idea to verify our analyses like this since it validates that our investigations are correct.

We saw that long-term debt is the topmost category when it comes to the average amount of debt. But is it the most common indicator in which the countries owe their debt? Let's find that out.


```sql
SELECT indicator_code ,
       COUNT(indicator_code) AS indicator_count
FROM international_debt 
GROUP BY indicator_code
ORDER BY indicator_count DESC , indicator_code DESC
LIMIT 20;

```


<br>
<br>
<br>
<br>


8- Other viable debt issues and conclusion 

There are a total of six debt indicators in which all the countries listed in our dataset have taken debt. The indicator DT.AMT.DLXF.CD is also there in the list. So, this gives us a clue that all these countries are suffering from a common economic issue. But that is not the end of the story, but just a part of the story.

Let's change tracks from debt_indicators now and focus on the amount of debt again. Let's find out the maximum amount of debt that each country has. With this, we will be in a position to identify the other plausible economic issues a country might be going through.

In this project, we took a look at debt owed by countries across the globe. We extracted a few summary statistics from the data and unraveled some interesting facts and figures. We also validated our findings to make sure the investigations were correct.

```sql
SELECT country_name,
      MAX(debt) AS maximum_debt
FROM international_debt
GROUP BY country_name
ORDER BY maximum_debt DESC
LIMIT 10;
```



| country_name                                     | maximum_debt          |
| ------------------------------------------------ | --------------------- |
| China                                            | 96218620835.70        |
| Brazil                                           | 90041840304.10        |
| Russian Federation                               | 66589761833.50        |
| Turkey                                           | 51555031005.80        |
| South Asia                                       | 48756295898.20        |
| Least developed countries: UN classification     | 40160766261.60        |
| IDA only                                         | 34531188113.20        |
| India                                            | 31923507000.80        |
| Indonesia                                        | 30916112653.80        |
| Kazakhstan                                       | 27482093686.40        |








