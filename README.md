# Coffee Sales with MySQL and Power BI

<br>

### Project Overview

This project aims to analyze a coffee shop's sales data using MySQL for data cleaning and processing, and Power BI for visualization. The objective is to extract business insights such as KPIs, sales trends, top-performing products, store performance, and sales patterns over time.

<BR>

### Tools & Technologies

- Database: MySQL
- Query Language: SQL
- Visualization: Power BI (for reporting and dashboards)

<br>

### Dataset Description

- Dataset: **Coffee Sales Data**
- Raw File: [coffee sales](https://docs.google.com/spreadsheets/d/1KzHFcrgreK3FQhRaEkt9V-29lo_GL7WZ/edit?usp=sharing&ouid=100554781607807743501&rtpof=true&sd=true)
- No of Rows: 149117 rows.
- No of Columns: 11 columns.
  - transaction_id (Unique ID for each sale)
  - transaction_date
  - transaction_time
  - transaction_qty
  - store_id
  - store_location
  - product_id
  - unit_price
  - product_category 
  - product_type
  - product_details


<br>
<br>

### Business Questions
#### SECTION A: KPI'S REQUIREMENTS

1. Total Sales Analysis:
    - Calculate the total sales for each respective month
    - determine the month-on-month increase or decrease in sales
    - calculate the percentage difference in sales between the selected month and the previous month


2. Total Order Analysis:
    - calculate the total number of orders for each respective month.
    - determine the month-on-month increase or decrease in the number of orders.
    - calcuate the % difference in the number of orders between the slected month and the previous month.

3. Total Quantity Sold Analysis:
    - calculate the total quantity sold for each respective month.
    - determine the month-on-month increase or decrease in the total quantity sold.
    - calculate the % difference in the total quantity sold between the selected month and the previous month.


#### SECTION B: CHARTS REQUIREMENTS

1. Calender Heat Map:
    - implement a calendar heat map that dynamically adjusts based on the selected month from a slicer.
    - each day on the calender will be color-coded to represent sales volume, with darker shades indicating higher sales
    - implement tooltips to display detailed metrics (Sales, Orders, Quantity) when hovering over a specific day.

2. Sales Analysis by Weekdays and Weekends:
    - segment sales data into weekdays and weekends to analyze performance variations.
    - provide insights into whether sales patterns differ significantly between weekdays and weekends.

3. Sales Analysis by Store Location:
    - visualize sales data by different store locations.
    - include month-over-month(MoM) difference metrics based on the selected month in the slicer.
    - Highlight MoM sales increase or decrease for each store location to identify trends.

4. Daily Sales Analysis with Average Line:
    - display daily sales for the selected month with a line chart.
    - incorporate an average line on the chart to represent the average daily sales
    - show whether it exceeding or falling below the average sales to identify exceptional sales days

5. Sales Analysis by Product Category:
    - analyze sales perfomance across differenct product categories.
    - provide insight into which product categories contribute the most to overall sales.

6. Top 10 Products by Sales:
    - identify and display the top 10 products based on sales volume.
    - allow users to quickly visualize the best-performing products in terms of sales.

7. Sales Analysis by Days and Hours:
    - Utilize a heat map to visualize sales patterns by days and hours.
    - implement tooltips to display detailed metrics (Sales, Orders, Quantity) when hovering over a specific day-hour.

<br>
<br>


### Solution Sections
The solution section is categorized into SQL Solution Section and Power BI Solution Section.

#### The SQL Solution Section is splitted into 2 Parts:
- Data Extraction and Cleaning
- Exploratory Data Analysis on Business Questions:
	- Section A: KPI'S Requirements
 	- Section B: Chart Requirements

#### Power BI Solution Sectionis splitted into
- Data Importation and Canvas Configuration
- Business Questions
	- Section A: KPI'S Requirements
 	- Section B: Chart Requirements


#### View Solution Section files for step by step documentation

<br>
<br>

