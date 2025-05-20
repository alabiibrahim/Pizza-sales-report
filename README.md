# SQL to Power BI - Data Portfolio


# Table of Contents


- [Project brief](#Project-brief)
- [Objective](#Objective)
- [Questions to Answer](#Questions-to-Answer)
- [Data source](#Data-source)
- [Tools](#Tools)
- [Dashboard design charts](#Dashboard-design-charts)
- [Development](#Developments)
- [Data processing / Transformation](#Data-processing-Transformation)
- [Findings](#Findings)
- [Recommendations](#Recommendations)


# Project brief
Pizza Sales Performance Analysis

## Objective
The goal is to analyze the dataset to gain insights into the effectiveness of pizza sales, optimize best performing pizza category and size to improve future market strategies.

## Questions to answer

1.	What is the total revenue generated from Pizza sales?
2.	What is the average order value?
3.	What is the total quantity of pizza sold?
4.	What is the total amount of orders?
5.	What is the average pizza price per order?
6.	How is the performance on daily and monthly basis?
7.	What is the Percentage of sales by pizza category?
8.	What is the Percentage of sales by pizza size? 
9.	Compute the Quantity sold by each pizzas category?
10.	Identify Top performing pizzas that generated the most in revenue?
11.	Identify the least performing pizzas that generated the least revenue?
12.	Find the pizza with the second highest quantity sold.

## Data source
Here are the Data needed to achieve project goal

1.  Pizza name
2.  Pizza category
3.  Pizza size
4.  Unit price
5.  Total price
6.  Quanity sold
7.  Order date

## Tools

| Tools | Purpose | 
| --- | --- | 
| SQL Server |  Cleaning, Transformation, Processing. |
| POWER BI | DAX Measures, Visualizations. | 

## Dashboard design

Here is a list of appropriate chart visuals used answering key questions.
1.	Score cards
2.	Column chart
3.	Donut chart
4.	Area chart
5.	Waterfall chart
6.	Bar chart
7.	Slicers

# Development

Here’s a step by step guide on how the data was approached 

1.	Getting the data
2.	Load data into SQL
3.	Clean and test with SQL
4.	Visualizations using POWER BI
5.	Generate Insights/ Findings
6.	Give recommendations 

## Data Processing / Transformation

The goal is to refine the dataset to ensure its clean and ready for analysis.

- Only relevant columns should be retained.
- All data types should be appropriate for the contents of each column.
- No column should contain null values, indicating complete data for all records

```sql

--1.	What is the total revenue generated from Pizza sales?

SELECT SUM(quantity * Unit_price) AS Revenue --Total Revenue
FROM pizza_sales; 

```

```sql

--2.	What is the average order value?

SELECT SUM(total_price) / COUNT(DISTINCT(order_id)) AS AvgOrderValue –- AO-Value
FROM pizza_sales;

```
```sql

--3.	What is the total quantity of pizza sold?

SELECT SUM(quantity) AS TotalPizzaOrder -- Total Pizza sold
FROM pizza_sales;

```
```sql

--4.	What is the total amount of orders?

SELECT COUNT(DISTINCT(order_id)) AS TotalOrders -- Total Orders
FROM pizza_sales;

```

```sql

--5.	What is the average pizza price per order?

SELECT CAST(SUM(quantity) AS DECIMAL (10,2)) / COUNT(DISTINCT(order_id)) AS APPO  --AveragePizzasPerOrder
FROM pizza_sales;

```


```sql

--6.	How is the performance on daily and monthly basis?
  
SELECT DATENAME(DW, order_date) AS OrderDay, COUNT(order_id) AS DayTotalOrders 
FROM pizza_sales 
GROUP BY DATENAME(DW, order_date);		-- Daily trend total orders

SELECT DATENAME(MONTH, order_date) AS OrderMonth, SUM(quantity * unit_price) AS MntRevenue 
FROM pizza_sales 
GROUP BY DATENAME(MONTH, order_date);		-- Monthly trend total orders

```

```sql
--7.	What is the Percentage of sales by pizza category?

SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL (10,2)) AS Revenue,   
CAST(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales)
AS DECIMAL (10,2)) AS 'Percentage'
FROM pizza_sales 
GROUP BY pizza_category;  --% of sales by Pizza Category

```

```sql

--8.	What is the Percentage of sales by pizza size? 

SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL (10,2)) AS Rev, 
CAST(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales)
AS DECIMAL(10,2)) AS 'Percentage'
FROM pizza_sales 
GROUP BY pizza_size   --% of sales by pizza size

```

```sql

--9.	Compute the quantity sold by each pizzas category?

SELECT SUM(quantity) AS PizzaSoldCategory, pizza_category --Total pizzas sold by pizzas category
FROM pizza_sales
GROUP BY pizza_category;

```

```sql

--10.	Top performing pizzas that generated the most in revenue?

SELECT TOP 5 pizza_name, SUM(total_price) AS Top5Revenue --Top 5 revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Top5Revenue DESC;

```sql

--11.	Least performing pizzas that generated the least revenue?

SELECT TOP 5 pizza_name, SUM(total_price) AS Top5Rev enue --Bottom 5 revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Top5Revenue ASC;

```

```sql

--12.	Find the pizza name with the second highest quantity sold

SELECT TOP 2  pIZZA_NAME,  SUM(quantity) AS RankQty FROM Pizza_sales WHERE quantity < 
(SELECT MAX(QUANTITY) FROM pizza_sales )  
GROUP BY pizza_name
ORDER BY RankQty DESC; --Second with the highest qty sold


```

## Findings

1.	Classic pizza outshines every other pizzas with the highest order and revenue. Supreme made the second spot, while chicken and veggie have a slight difference between them.

2.	Pizza with the “L” size performs significantly higher than other size whilst pizza with the “xxl” size performs very low compare to others.

3.	The month of February & December generated low revenue but September & October are lowest in both revenue and quantity sold.

4.	Pizzas are mostly ordered by weekends than midweek and at dinner, lunch and evening with dinner topping the list.


5.	Classic deluxe performs best, with Barbecue chicken, Hawaii pizza, Pepperoni pizza and Thai chicken having slight or no difference between them. 

6.	Soppresata, Spinach, Calabrese, Mediterranean and Brie-carrie pizza performs the lowest.


## Recommendations

1.	Increase in production of Pizza with “L” size should be made priority as it performs the best.

2.	Pizzas categories are doing well but the strategy used for the Classic pizza can be applied to others in the same category as there’s slight difference between them.

3.	Increase in production should be prioritize by mid-year starting from March-August and towards ending and new year (November-January). As sales seems to skyrocket at midyear.

4.	Plan for the increase in weekends pizza production than midweek.

5.	Least selling pizza production needs to be halted and invest more on top performing ones. 




