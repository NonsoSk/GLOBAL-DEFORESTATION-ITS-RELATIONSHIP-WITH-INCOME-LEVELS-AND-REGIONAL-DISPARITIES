# GLOBAL-DEFORESTATION-ITS-RELATIONSHIP-WITH-INCOME-LEVELS-AND-REGIONAL-DISPARITIES.


# Overview

This data analysis project aims at exploring  the relationship between income levels, and regional disparities of  countries involved in global deforestation.
This is basically an SQL- based project through which we will gain insights into the global trends of deforestation, its impact on different income groups, and regional variations in forest areas.
To properly do this, we will perform data cleaning, exploratory data analysis, aggregation. We shall explore advanced SQL functions as well as window functions. We will use statistical tools and techniques to calculate the total number of countries involved in deforestation, identify income groups based on total area, determine the total forest area in square kilometers, and identify countries with the highest forest areas in each region.


# Introduction

By analyzing the datasets related to deforestation, income groups, and regional information of countries and areas, we will answer the following questions:
 
1.	What are the total number of countries involved in deforestation? 
2.	Show the income groups of countries having total area ranging from 75,000 to 150,000 square meter?
3.	Calculate average area in square miles for countries in the 'upper middle income region'. Compare the result with the rest of the income categories.
4.	Determine the total forest area in square km for countries in the 'high income' group. Compare result with the rest of the income categories.
5.	Show countries from each region(continent) having the highest total forest areas. 

However, in the course of this analysis, I was able to ask deeper questions outside the five (5) questions above, the aim was to get detailed insights without losing sight of our primary questions.

My goal is that by the completion of this project, we must have had a detailed understanding of the global deforestation scenario, the income levels of countries related to their land area, and regional disparities in terms of forest areas and other subtle findings. The result of our analysis could provide valuable insights for policymakers, environmental organizations, and researchers to devise strategies for sustainable forest management and mitigate the adverse effects of deforestation.

## I demonstrated the following skills in this project
- Aggregate functions
- Conditional statements (top n)
- Critical thinking
- Group by and order by
- Joins
- Operators (is null, <> null, >, <)
- Problem solving
- Select distinct, from
- String functions
- Subquery and CTE
- Window functions (over, rank, dense rank)

# Data sourcing

I obtained the data by downloading the csv file from my drive and then imported into my database “PROJECT”, which I created using the syntax below:

CREATE DATABASE PROJECT;

The data is a sample data of a public available datasets on deforestation, which has three tables (Forest area, land area and Regions), and provides data of  income levels, regional information, name of countries, country codes, total area and total forest area. After the importation, I cleaned the data then proceeded to writing queries and performing my analysis.

# Data Transformation and cleaning

The dataset I worked with did not really require much cleaning, so I first checked for null values. From our fact tables, only two columns from both tables (forest area table and land area table) have numerical values that can be checked so the syntax below was used.

SELECT FOREST_AREA_SQKM FROM FOREST_AREA WHERE FOREST_AREA_SQKM IS NULL;

SELECT TOTAL_AREA_SQ_MI FROM LAND_AREA WHERE TOTAL_AREA_SQ_MI IS NULL;  

After discovering that there were null values, there was need to fill them with the average of the column affected.
Below is the syntax I employed in getting the average from the respective columns and tables.

SELECT ROUND (AVG (FOREST_AREA_SQKM), 0) FROM FOREST_AREA;

SELECT ROUND (AVG (TOTAL_AREA_SQ_MI), 0) FROM LAND_AREA;

Then I proceeded to filling the null values with the syntax below:

UPDATE FOREST_AREA
SET FOREST_AREA_SQKM = 391052 WHERE FOREST_AREA_SQKM IS NULL;

UPDATE LAND_AREA
SET TOTAL_AREA_SQ_MI= 457095 WHERE TOTAL_AREA_SQ_MI IS NULL;

After this, I proceeded to analyzing my data.

# Analysis and problem solving.
I analyzed the data step by step according to the questions and aim of my analysis. 

Outside the basic questions which my analysis covered, the following questions helped for a more insightful analysis:
- How many regions are involved in deforestation and how many countries does each region cover? 

- How many countries have total area ranging from 75,000 to 150,000 square meter?

- Find out how these countries (countries having total area ranging from 75,000 to 150,000 square meter) are spread through the different income groups.

- How many groups are there in the income group?

- Find out the specific number of countries each income group has.

- Tabulate the average area in square miles of the countries in each of income region.

- show the Top 5 countries with the greatest forest area 

Moving further, we would see how these questions were answered and how each at specific times played its role.

In the first analysis, we set out to answer the question:
**What are the total number of countries involved in deforestation?**
To avoid repetition of countries since I was working with **5886** rows of data, I had to find distinctively the countries involved before counting them. The syntax below helped me achieve this:

SELECT DISTINCT(COUNTRY_NAME) FROM REGIONS;

SELECT COUNT (DISTINCT(COUNTRY_NAME)) FROM REGIONS; 

I was able to discover that there are **219** countries involved In deforestation. 

It could be possible that all **219** countries came from a particular region which could hamper economic growth and uneven distribution of resources. To be sure of this, I sought out to check how many regions were involved in deforestation and how many countries each region covered.
Below are the regions involved in deforestation.
![REGION](https://github.com/NonsoSk/GLOBAL-DEFORESTATION-ITS-RELATIONSHIP-WITH-INCOME-LEVELS-AND-REGIONAL-DISPARITIES/assets/147613828/d8ae6225-6fb3-4e8a-b830-ad94d003e408)

The syntax below helped me arrive at this:
SELECT DISTINCT (REGION) FROM REGIONS;

I then counted these regions, using the below syntax, and discovered  that the regions involved in deforestation are (8)

SELECT COUNT (DISTINCT (REGION)) AS COUNT_OF_REGION FROM REGIONS;

It was thus easier at this point to tell how many countries each of this eight (8) regions were made of.
Using : 

SELECT REGION, COUNT (DISTINCT(COUNTRY_NAME)) AS NUMBER_OF_COUNTRIES FROM REGIONS GROUP BY REGION ORDER BY NUMBER_OF_COUNTRIES DESC;
We Can see that from the image below, 
![REGION BY COUNTRY SQL 2](https://github.com/NonsoSk/GLOBAL-DEFORESTATION-ITS-RELATIONSHIP-WITH-INCOME-LEVELS-AND-REGIONAL-DISPARITIES/assets/147613828/f277ed21-b675-42f5-8f6e-22b519ac07e7)

All 219 countries were spread across the regions with Europe having the highest number of countries practicing deforestation and then the world has the least (just one).

Since our analysis was based on establishing the relationship between income group and total area, we proceeded to finding the highest range of total area and found out it was between 75,000 sq_mi to 150,00 sq_mi. This thus served as the base for answering our second question.

**Show the income groups of countries having total area ranging from 75,000 to 150,000 square meter?**

To get a very presentable and neat analysis, I had to round up the total_area_sq_mi to no decimal place, added a table “TOTAL_AREA_SQUARE_MI” where our new data was entered, then completed it by updating the table. 
The following syntax helped me arrive at this.


 SELECT ROUND (TOTAL_AREA_SQ_MI, 0) TOTAL_AREA_SQUARE_MI FROM LAND_AREA;

 ALTER TABLE LAND_AREA ADD TOTAL_AREA_SQUARE_MI INT;

UPDATE LAND_AREA
SET TOTAL_AREA_SQUARE_MI= (ROUND (TOTAL_AREA_SQ_MI, 0)) FROM LAND_AREA;

It was necessary that I use only one column for “total area”, throughout the remaining process of our analysis so I continued with the newly created column and so dropped the old column “total_area_sq_mi” with the syntax below:

 ALTER TABLE LAND_AREA DROP COLUMN TOTAL_ARE_SQ_MI;

Having rounded up the entire column to be used, I proceeded to answering the question by first seeing the actual countries and the exact number of countries that have “total_area_sq_mi” between 75000 to 15000.

![SQL 3](https://github.com/NonsoSk/GLOBAL-DEFORESTATION-ITS-RELATIONSHIP-WITH-INCOME-LEVELS-AND-REGIONAL-DISPARITIES/assets/147613828/f8aec5d5-3318-45b2-9056-d0a0d79653f2)

I arrived at the result above using the syntax:

SELECT DISTINCT (COUNTRY_NAME) FROM
(SELECT R.COUNTRY_NAME,TOTAL_AREA_SQUARE_MI,INCOME_GROUP FROM REGIONS R INNER JOIN LAND_AREA L ON R.COUNTRY_NAME=L.COUNTRY_NAME
 WHERE TOTAL_AREA_SQUARE_MI BETWEEN 75000 AND 150000) SETIG;
 
Thus, using the count function, I was able to find out that there are exactly **25** countries out of the **219** countries that have total area ranging from 75,000 to 150,000 square meter.

I was able to solve this problem with the syntax below:

 select count (DISTINCT (COUNTRY_NAME)) FROM
(SELECT R.COUNTRY_NAME,TOTAL_AREA_SQUARE_MI,INCOME_GROUP FROM REGIONS R INNER JOIN LAND_AREA L ON R.COUNTRY_NAME=L.COUNTRY_NAME
 WHERE TOTAL_AREA_SQUARE_MI BETWEEN 75000 AND 150000) SETIG;

At this point, it was easier then to show the “income group” of these **25** countries. 

So, using:

SELECT INCOME_GROUP, count (DISTINCT (COUNTRY_NAME)) AS COUNTRIES FROM
(SELECT R.COUNTRY_NAME,TOTAL_AREA_SQUARE_MI,INCOME_GROUP FROM REGIONS R INNER JOIN LAND_AREA L ON R.COUNTRY_NAME=L.COUNTRY_NAME
WHERE TOTAL_AREA_SQUARE_MI BETWEEN 75000 AND 150000) SETIG GROUP BY INCOME_GROUP ORDER BY COUNTRIES DESC; 

I discovered that 10 countries had high income, 6 countries had upper middle income, 5 were in the lower middle income group and 4 were of low income as shown by the table below:
![NUMBER 4](https://github.com/NonsoSk/GLOBAL-DEFORESTATION-ITS-RELATIONSHIP-WITH-INCOME-LEVELS-AND-REGIONAL-DISPARITIES/assets/147613828/4edea6c7-e69e-4c23-b9fe-2f176beb1af7)

Progressively, the third question to be answered was: Calculate average area in square miles for countries in the 'upper middle-income region'. Compare the result with the rest of the income categories.

It was necessary before answering any question, to know how many income groups were in our data. So I used the syntax:

SELECT DISTINCT (INCOME_GROUP) FROM REGIONS;

SELECT COUNT (DISTINCT (INCOME_GROUP)) FROM REGIONS;

And here we discovered that there are **five (5)** income groups. This means that our concern would be to compare the result of 'upper middle-income region' with the result of the other 4 income groups.

The question to be answered, implied that I get the average area in square miles for countries in all 8 regions for proper comparison.
my first concern was with calculating the average area in square miles for countries in the 'upper middle-income region'. 
To this end, I used the syntax below:

SELECT AVG (TOTAL_AREA_SQUARE_MI) AVERAGE_OF_UPPER_MIDDLE_INCOME FROM (SELECT L.COUNTRY_NAME,INCOME_GROUP,TOTAL_AREA_SQUARE_MI
FROM REGIONS R JOIN LAND_AREA L ON R.COUNTRY_NAME = L.COUNTRY_NAME 
WHERE INCOME_GROUP = 'UPPER MIDDLE INCOME') DATUM;

Which resulted in: 

![AVERAGE 5](https://github.com/NonsoSk/GLOBAL-DEFORESTATION-ITS-RELATIONSHIP-WITH-INCOME-LEVELS-AND-REGIONAL-DISPARITIES/assets/147613828/7f24aa72-28d3-4828-8558-838e27cf730c)


I repeated same action only changing the information in the “where” clause, replacing it with the particular income group I intend to calculate.
Moving further, I shall tabulate this for proper understanding.


It was proper to see how many countries fell under the “upper middle income group”; “high income group”; “lower income group”, and of course all other income groups.

I calculated that of the upper middle-income group (i.e., the number of countries that have upper middle income) using:

SELECT COUNT (DISTINCT (COUNTRY_NAME)) 
FROM (SELECT L.COUNTRY_NAME,INCOME_GROUP,TOTAL_AREA_SQUARE_MI FROM REGIONS R JOIN LAND_AREA L ON R.COUNTRY_NAME = L.COUNTRY_NAME WHERE INCOME_GROUP = 'UPPER MIDDLE INCOME')NUMBER;

I repeated the action to find out the number of countries in the income groups only changing the “where” clause to fill in the appropriate group at each point.

The various results of our query showed that **79** have high income; **55** countries have upper middle income; **33** countries have low income and **44** countries have lower middle income 

We can see that a greater number of countries generate high income from deforestation and **33** countries generate low income.
the aim of this analysis covers also how to optimize the income of countries involved in deforestation. 
 
Having established that the average area in square miles for countries in the 'upper middle-income region as 390072.33988,  and having  seen the total number of countries that generate 'upper middle-income as 55,  we can now proceed with comparing our result with the results of other income groups.

Using this syntax:


WITH COMPARE AS
(SELECT L.COUNTRY_NAME, INCOME_GROUP, TOTAL_AREA_SQUARE_MI 
FROM REGIONS R   JOIN LAND_AREA L ON R.COUNTRY_NAME = L.COUNTRY_NAME)
SELECT COUNT (DISTINCT (COUNTRY_NAME)) AS NO_OF_COUNTRIES, INCOME_GROUP, AVG (TOTAL_AREA_SQUARE_MI) AVERAGE 
FROM COMPARE GROUP BY INCOME_GROUP ORDER BY AVERAGE DESC;

The result of our query is
IMAGE…………………………..


Very noticeable is the fact that countries with upper middle income, has a fair area in sq miles on average when compared to other income groups. It is second to “NULL”.
However, it is worrisome that these countries (in the “Null income group” as well as the “upper middle income”) do not generate high income despite having the highest and second highest average respectively. 

I also observed that countries that generate the least income have the least area in square miles on average.

The answers provided and the analysis carried on the third question has made it a lot easier for us to face the fourth question which is:

Determine the total forest area in square km for countries in the 'high income' group. Compare result with the rest of the income categories.

From our previous analysis, we discovered that precisely 79 countries involved in deforestation have high income.
We also saw that on average in terms of area in square miles, they (high income countries) were third when compared to countries in the “Null” and “Upper middle” income group.
 
Moving straight to the question at hand, we used the syntax :

SELECT ROUND (SUM (FOREST_AREA_SQKM),0) SUM_OF_HIGH_INCOME_GROUP FROM (SELECT F.COUNTRY_NAME,INCOME_GROUP,FOREST_AREA_SQKM
FROM FOREST_AREA F JOIN REGIONS R ON F.COUNTRY_NAME = R.COUNTRY_NAME 
WHERE INCOME_GROUP = 'HIGH INCOME') GAD;

to discover that the total forest area in square km for countries in the 'high income' group
is IMAGE……….

The table below, would help us compare the total forest area in square km for countries in the 'high income' with those in other income groups.
IMAGE…..
  
We were able to get the above table with this syntax:
WITH EMMAS AS
(SELECT F.COUNTRY_NAME,INCOME_GROUP,FOREST_AREA_SQKM
FROM FOREST_AREA F JOIN REGIONS R ON F.COUNTRY_NAME = R.COUNTRY_NAME )
SELECT INCOME_GROUP, ROUND (SUM (FOREST_AREA_SQKM), 0) TOTAL FROM EMMAS GROUP BY INCOME_GROUP  ORDER BY TOTAL DESC;


From the table above, We can observe that 79 countries despite having total forest area less than the countries ranking first and second in total forest  area, they (these 79 countries) still found their way to generate the highest income.

Generally therefore, countries in the high income group do not have as much total forest area as the countries in the upper middle income group and the null income group.

As a way of gaining further insights, it is proper to know at least the top 5 countries with highest total forest area.
This is to help us ascertain if actually countries with the highest total forest area are majorly countries in the upper middle income groups.

IMAGE.

We were able to get this using the syntax below:

SELECT * FROM (SELECT TOP 5 R.COUNTRY_NAME, SUM(FOREST_AREA_SQKM) AS TOTAL_FOREST_AREA, 
RANK () OVER(ORDER BY SUM(FOREST_AREA_SQKM) DESC) AS TOP_FOREST_RANKING_COUNTRIES, INCOME_GROUP
FROM REGIONS R JOIN FOREST_AREA F ON R.COUNTRY_NAME = F.COUNTRY_NAME
GROUP BY R.COUNTRY_NAME, INCOME_GROUP ) TOP_FOREST_RANKING_COUNTRIES
ORDER BY TOTAL_FOREST_AREA DESC; 

It is evident from the table that the country “world” which is of the “Null” income group has the highest forest area in square miles and the second largest country (Russian Federation) as well as the third (Brazil) by total forest area, are of the upper middle income group. While other countries in the high income group followed suit.


Finally, the last analysis had to do with providing an answer to the question:

Show countries from each region(continent) having the highest total forest areas.

here I applied windows function and below is the syntax I used:

WITH FINAL AS(SELECT  R.COUNTRY_NAME, REGION, FOREST_AREA_SQKM AS TOTAL_FOREST_AREA,ROW_NUMBER () OVER
(PARTITION BY REGION ORDER BY FOREST_AREA_SQKM  DESC) RANKING
FROM REGIONS R JOIN FOREST_AREA F ON R.COUNTRY_NAME = F.COUNTRY_NAME)
SELECT COUNTRY_NAME, REGION, TOTAL_FOREST_AREA FROM FINAL WHERE RANKING = '1' ORDER BY TOTAL_FOREST_AREA DESC;

The result of our query is displayed in the image below
IMAGE


From this, we can observe that the region with the highest total forest area is the “world”.









