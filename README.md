# Pizza-Sales-Dashboard-End-to-End-Project
## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis (EDA)](#my-custom-anchor-point)
- [Data Analysis](#data-analysis)
- [Results/Findings](#my-custom-anchor-point2)
- [Recommendations](#recommendations)
  
### Project Overview
For a pizza restaurant chain, understanding sales performance, customer preferences, and product popularity is essential for maximizing revenue and optimizing menu offerings. In this project, I analysed sales data across multiple dimensions including revenue, order volume, pizza categories, sizes, and time-based trends to identify best and worst performing products, peak business periods, and customer purchasing patterns.

<img width="755" height="415" alt="pizza sales1" src="https://github.com/user-attachments/assets/db5c7d89-88b6-4ade-ba92-15412e7f6790" />

<img width="754" height="415" alt="pizza sales2" src="https://github.com/user-attachments/assets/64786c83-6296-4a52-9a53-36bf85128934" />

### Data Sources
The primary dataset used for this analysis contains pizza sales transaction records from the year 2015, including order details: pizza types, categories, sizes, quantities, and revenue figures across 21,350 total orders.

### Tools
- **SQL** - Exploratory Data Analysis
- **Power BI** - Data Visualization and Dashboard Design

### Data Cleaning and Preparation
In the data preparation phase, I performed the following tasks;
- Data loading and inspection
- Handling missing values and errors
- Data formatting and normalization
- Creating calculated measures (Total Revenue, Avg Order Value,Total Pizzas Sold, Avg Pizzas per Order)
- Categorizing pizzas by type, size, and category
  
<a name="my-custom-anchor-point"></a>
### Exploratory Data Analysis (EDA)
This involved exploring the data to provide the following key imnsights;
1.Daily Trend for Total Orders
2.Monthly Trend for Total Orders
3.Percentage of Sales by Pizza Category
4.Percentage of Sales by Pizza Size
5.Total Pizzas Sold by Pizza Category
6.Top 5 Best Sellers by Revenue, Total Quantity and Total Orders
7. Bottom 5 Best Sellers by Revenue, Total Quantity and Total Orders

## Data Analysis
This is the code I used to answer the questions.
### A. KPI’s
1. Total Revenue:

```sql
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;
```
2. Average Order Value

```sql
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;
```
3. Total Pizzas Sold
```sql
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;
```
4. Total Orders
```sql
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;
```
5. Average Pizzas Per Order
```sql
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_per_order
FROM pizza_sales;
```
### B. Daily Trend for Total Orders
```sql
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date);
```
### C. Monthly Trend for Orders
```sql
select DATENAME(MONTH, order_date) as Month_Name, COUNT(DISTINCT order_id) as Total_Orders
from pizza_sales
GROUP BY DATENAME(MONTH, order_date);
```
### D. % of Sales by Pizza Category
```sql
SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category;
```
### E. % of Sales by Pizza Size
```sql
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size;
```
### F. Total Pizzas Sold by Pizza Category
```sql
SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
FROM pizza_sales
GROUP BY pizza_category
ORDER BY Total_Quantity_Sold DESC;
```
### G. Top 5 Pizzas by Revenue
```sql
SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC;
```
### H. Bottom 5 Pizzas by Revenue
```sql
SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC;
```
### I. Top 5 Pizzas by Quantity
```sql
SELECT Top 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC;
```
### J. Bottom 5 Pizzas by Quantity
```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC;
```
### K. Top 5 Pizzas by Total Orders
```sql
SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC;
```
### L. Bottom 5 Pizzas by Total Orders
```sql
SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders ASC;
```
<a name="my-custom-anchor-point2"></a>
### Results/Findings
Some of the Analysis results are summarised as follows;
- **The Thai Chicken** pizza generates the highest revenue, while **The Brie Carre** pizza records the lowest revenue, quantity sold, and total orders.
- **The Classic Deluxe** pizza leads in both purchase quantity and total order frequency.
- **Large size** pizzas generate the highest sales, contributing significantly to overall revenue.
- **Classic category** pizzas record the highest overall sales volume among all categories.
- Weekend days **(Friday and Saturday)** see the highest order volumes, indicating peak customer traffic.
- **July and May** are the peak months for pizza orders, suggesting seasonal demand patterns.
- The **Chicken category** leads in revenue contribution at 26.91%, closely followed by Supreme and Classic categories.
- **Medium sized** pizzas account for the largest revenue share.

### Recommendations
- **Promote top performers** — Feature The Thai Chicken and The Classic Deluxe in promotions and bundle offers to capitalize on their popularity.
- **Re-evaluate low performers** — Consider removing or reformulating The Brie Carre and other bottom-ranking pizzas to optimize menu efficiency.
- **Weekend-focused marketing** — Increase targeted campaigns and staffing on Fridays and Saturdays to maximize high-traffic periods.
- **Seasonal campaigns** — Launch summer promotions in July and May to align with peak ordering months.
- **Size-based pricing strategy** — Leverage the popularity of Large and Medium pizzas through value meal deals and upsizing incentives.
- **Category expansion** — Explore new offerings in the high-performing Chicken and Classic categories to drive further revenue growth.
