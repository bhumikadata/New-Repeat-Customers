
# NamasteKart E-commerce Business Metrics

## Overview

NamasteKart, an e-commerce company, aims to track daily business metrics to understand the dynamics of their customer base. One crucial metric they want to monitor is the number of new and repeat customers making purchases on their website each day.

This project provides a SQL solution to calculate the daily count of new and repeat customers based on their purchase history.

## Table Structure

The solution utilizes a table named `customer_orders` with the following columns:

- `order_id`: INT (Primary key) - Represents the unique identifier for each order.
- `order_date`: DATE - Indicates the date when the order was placed.
- `customer_id`: INT - Represents the unique identifier for each customer.
- `order_amount`: INT - Indicates the amount of the order.

![Day16sql](https://github.com/bhumikadata/New-Repeat-Customers/assets/131578649/23c1ca47-3094-4140-98af-f44e51efd6c4)


## SQL Solution

This SQL query calculates the daily count of new and repeat customers based on their purchase history.

```sql
WITH First_order AS (
  SELECT
    customer_id,
    MIN(order_date) AS First_order_date
  FROM 
    customer_orders
  GROUP BY 
    customer_id  
)
SELECT
  o.order_date,
  SUM(CASE WHEN o.order_date = f.First_order_date THEN 1 ELSE 0 END) AS new_customers, 
  SUM(CASE WHEN o.order_date > f.First_order_date THEN 1 ELSE 0 END) AS repeat_customers
FROM 
  customer_orders o
JOIN
  First_order f ON o.customer_id = f.customer_id   
GROUP BY 
  o.order_date;


