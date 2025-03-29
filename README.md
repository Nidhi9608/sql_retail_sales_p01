-- Retail Sales Analysis SQL Project

-- Project Overview

-- Project Title: Retail Sales Analysis

-- Database: sql_query_p01

 This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. 
 The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. 
 This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

-- Objectives
* Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
* Data Cleaning: Identify and remove any records with missing or null values.
* Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
* Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

 Project Structure
 
 1. Database Setup
* Database Creation: The project starts by creating a database named p1_retail_db.
* Table Creation: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
sql'''
drop table if exists retail_sales;
create table retail_sales
(transactions_id int primary key,	
sale_date	date,
sale_time	time,
customer_id int,	
gender	varchar(15),
age	int,
category varchar(15),	
quantiy	 int,
price_per_unit	float,
cogs float,	
total_sale float
);

alter table retail_sales
change column quantiy quantity int
;

select * from retail_sales;

select * from retail_sales
where 
transactions_id is null
 or
sale_date is null 
or
sale_time is null 
or
category is null 
or
quantity is null 
or
cogs is null 
or
total_sale is null 
;
'''
-- Data Cleaning
delete from retail_sales
where 
transactions_id is null
 or
sale_date is null 
or
sale_time is null 
or
category is null 
or
quantity is null 
or
cogs is null 
or
total_sale is null 
;

-- 2. Data Exploration & Cleaning
-- Record Count: Determine the total number of records in the dataset.
-- Customer Count: Find out how many unique customers are in the dataset.
-- Category Count: Identify all unique product categories in the dataset.
-- Null Value Check: Check for any null values in the dataset and delete records with missing data.

-- Data Exploration
-- How many sales we have?
select count(*) as total_sale from retail_sales;

-- How many customers we have?
select count(customer_id) as total_sale from retail_sales;
select count(distinct customer_id) as total_sale from retail_sales;

-- How many categories we have?
select count(distinct category) as total_sale from retail_sales;
select distinct category from retail_sales;

-- Data Analysis & Business Key Problems & Answers

-- My Analysis & Findings
-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than equal to 4 in the month of Nov-2022
-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)

-- 3. Data Analysis & Findings
-- The following SQL queries were developed to answer specific business questions:

-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
select * from retail_sales
where sale_date = '2022-11-05';

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than equal to 4 in the month of Nov-2022
select 
*
from retail_sales
where category = 'Clothing'
and quantity >= 4
and date_format(sale_date, '%Y-%m') = '2022-11';

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
select
category, 
sum(total_sale) as net_sale,
count(*) as total_orders
from retail_sales
group by 1;

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
select 
round(avg(age),2) as avg_age
from retail_sales
where 
category = 'Beauty';

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
select 
transactions_id,
total_sale
from retail_sales
where total_sale > 1000;

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
select 
category,
gender,
count(*) as total_transctions
from retail_sales
group by 
category, 
gender
order by 1;

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
select 
year,
month,
avg_total_sale
from
(
select 
year(sale_date) as year,
month(sale_date) as month,
avg(total_sale) as avg_total_sale,
rank() over(partition by year(sale_date) order by avg(total_sale) desc) as rank_total_sale
from retail_sales
group by 1, 2
) as t1
where rank_total_sale = 1;
-- order by 1, 3 desc

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
select
customer_id,
sum(total_sale) total_sales
from retail_sales
group by 1
order by 2 desc
limit 5;

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
select
category,
count(distinct customer_id) as unique_customer
from
retail_sales
group by category;

-- Q.10 Write a SQL query to create each shift and number of orders 
-- (Example Morning <=12, Afternoon Between 12 & 17, Evening >17) 

with hourly_sales as (
select *,
case
when hour(sale_time) < 12 then 'morning'
when hour(sale_time) between 12 and 17 then 'afternoon'
else 'evening'
end as shift
from retail_sales
)
select 
shift,
count(*) as orders
from hourly_sales
group by shift
order by field(shift, 'Morning', 'Afternoon', 'Evening');

-- Findings
* Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
* High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
* Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
* Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.

-- Reports
* Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
* Trend Analysis: Insights into sales trends across different months and shifts.
* Customer Insights: Reports on top customers and unique customer counts per category.

-- Conclusion

* This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries.
* The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.


