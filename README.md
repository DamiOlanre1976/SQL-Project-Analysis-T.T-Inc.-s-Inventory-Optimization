
### SQL Queries and Codes

```markdown
# SQL Capstone Project: T.T Inc.'s Inventory Optimization Journey

## Overview
This project focuses on optimizing inventory management for T.T Inc., a leading company in the consumer electronics sector. As a Data Analyst, I was tasked with analyzing sales data to provide insights and strategies aimed at minimizing overstock and understock situations, understanding seasonal trends, and improving customer satisfaction through enhanced product availability.

## Objectives
- Optimize inventory levels to minimize overstock and understock situations.
- Understand seasonal trends of sales for different products.
- Improve customer satisfaction by ensuring product availability.

## SQL Queries
The following SQL queries were executed to extract meaningful insights from the datasets:

### 1. Total Units Sold per Product SKU
```sql
SELECT productid, SUM(inventoryquantity) AS Total_Units_Sold
FROM sales
GROUP BY productid
ORDER BY Total_Units_Sold DESC;
```

### 2. Product Category with Highest Sales Volume Last Month
```sql
SELECT p.productcategory, SUM(s.inventoryquantity) AS Highest_Sales_Volume
FROM sales s
JOIN product p ON p.productid = s.productid
WHERE s.sales_year = '2021' AND s.sales_month = '11'
GROUP BY p.productcategory
ORDER BY Highest_Sales_Volume DESC
LIMIT 1;
```

### 3. Correlation of Inflation Rate with Sales Volume
```sql
SELECT s.sales_year, s.sales_month, AVG(f.inflationrate) AS Avg_InflationRate, SUM(s.inventoryquantity) AS Sales_Volume
FROM sales s
JOIN factors f ON f.salesdate = s.salesdate
GROUP BY s.sales_year, s.sales_month;
```

### 4. Monthly Correlation of Inflation Rate and Sales Quantity
```sql
SELECT s.sales_year, s.sales_month, AVG(f.inflationrate) AS Avg_InflationRate, SUM(s.inventoryquantity) AS Sales_Volume
FROM sales s
JOIN factors f ON f.salesdate = s.salesdate
WHERE s.salesdate >= (CURRENT_DATE - INTERVAL '1 year')
GROUP BY s.sales_year, s.sales_month
ORDER BY s.sales_year, s.sales_month;
```

### 5. Impact of Promotions on Sales Quantity
```sql
SELECT p.productcategory, ROUND(AVG(s.inventoryquantity)) AS Avg_Sales_WithoutPromotion, p.promotions
FROM sales s
JOIN product p ON p.productid = s.productid
WHERE p.promotions = 'No'
GROUP BY p.productcategory, p.promotions
UNION ALL
SELECT p.productcategory, ROUND(AVG(s.inventoryquantity)) AS Avg_Sales_WithPromotion, p.promotions
FROM sales s
JOIN product p ON p.productid = s.productid
WHERE p.promotions = 'Yes'
GROUP BY p.productcategory, p.promotions;
```

### 6. Average Sales Quantity per Product Category
```sql
SELECT p.productcategory, ROUND(AVG(s.inventoryquantity)) AS Avg_Sales
FROM sales s
JOIN product p ON p.productid = s.productid
GROUP BY p.productcategory
ORDER BY Avg_Sales DESC;
```

### 7. Effect of GDP on Total Sales Volume
```sql
SELECT s.sales_year, SUM(f.gdp), SUM(s.inventoryquantity) AS Total_Sales_Volume
FROM sales s
JOIN factors f ON f.salesdate = s.salesdate
GROUP BY s.sales_year
ORDER BY Total_Sales_Volume DESC;
```

### 8. Top 10 Best-Selling Product SKUs
```sql
SELECT productid, SUM(inventoryquantity) AS Total_Sales
FROM sales
GROUP BY productid
ORDER BY Total_Sales DESC
LIMIT 10;
```

### 9. Seasonal Factors Influence on Sales Quantities
```sql
SELECT p.productcategory, SUM(s.inventoryquantity) AS Total_Sales, f.seasonalfactor
FROM sales s
JOIN factors f ON f.salesdate = s.salesdate
JOIN product p ON p.productid = s.productid
GROUP BY p.productcategory, f.seasonalfactor;
```

### Insights and Recommendations

```markdown
# Insights and Recommendations for T.T Inc.'s Inventory Optimization Journey

## Insights
The following insights were derived from the SQL queries executed on the sales and product datasets:

1. **Total Units Sold per Product SKU**: Identified the best-selling products, which helps focus inventory on high-demand items.
  
2. **Product Category with Highest Sales Volume Last Month**: Understanding which product category had the highest sales volume can guide marketing and inventory decisions.

3. **Correlation of Inflation Rate with Sales Volume**: Analyzing the correlation between inflation and sales volume provides insight into how economic factors influence sales.

4. **Monthly Correlation of Inflation Rate and Sales Quantity**: This analysis helps in understanding trends over the past year, allowing for better forecasting.

5. **Impact of Promotions on Sales Quantity**: Provides a clear comparison of sales performance with and without promotions, highlighting the effectiveness of marketing strategies.

6. **Average Sales Quantity per Product Category**: Identifying average sales per category can inform inventory distribution strategies.

7. **Effect of GDP on Total Sales Volume**: Understanding the relationship between GDP and sales volume can guide business strategy.

8. **Top 10 Best-Selling Product SKUs**: Helps identify which products are driving the most revenue.

9. **Seasonal Factors Influence on Sales Quantities**: Analyzing seasonal effects allows for better inventory planning during peak periods.

## Recommendations
1. **Focus on High-Demand Products**: Prioritize inventory for top-selling SKUs to meet customer demand and reduce stockouts.

2. **Seasonal Planning**: Adjust inventory levels based on seasonal trends identified in the analysis to optimize stock levels.

3. **Promotional Strategies**: Implement targeted promotions for underperforming product categories to boost sales.

4. **Monitor Economic Indicators**: Keep track of inflation and GDP data to adjust pricing strategies and inventory levels accordingly.

5. **Customer Satisfaction**: Ensure that popular products are always in stock to enhance customer experience and loyalty.

## Conclusion
This project provided valuable insights into T.T Inc.'s inventory management challenges and opportunities. By leveraging data-driven strategies, the company can optimize inventory levels, improve sales performance, and enhance customer satisfaction.

```

