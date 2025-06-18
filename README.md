# sql_bacis_commands
CREATE DATABASE ecommerce;
USE ecommerce;

CREATE TABLE sale (
  InvoiceNo    VARCHAR(20),
  StockCode    VARCHAR(20),
  Description  VARCHAR(255),
  Quantity     INT,
  InvoiceDate  DATETIME,
  UnitPrice    DECIMAL(10,2),
  CustomerID   INT,
  Country      VARCHAR(50)
);
-- THIS PART IS FOR CLEANING THE DATA --
SET SQL_SAFE_UPDATES = 0;

DELETE FROM sale
WHERE Description IS NULL;

DELETE FROM sale 
WHERE Quantity <=0 OR UnitPrice <= 0;

--  NOW CORE SQL EXPORATORY QUERRIES --

SELECT COUNT(*) AS totle_orders,
COUNT(DISTINCT CustomerID) AS totle_customers
FROM sale;

SELECT Country,ROUND(SUM(Quantity * UnitPrice), 2) AS totle_revenue
FROM sale
GROUP BY Country
ORDER BY totle_revenue DESC
LIMIT 5;

-- NOW WE WILL CALCULATE MONTHLY REVENUE TRENDS--

SELECT MONTH(InvoiceDate) AS month,
ROUND(SUM(Quantity * UnitPrice),2) AS monthly_revenue
FROM sale
GROUP BY month
ORDER BY monthly_revenue DESC;

-- now we will calculate which are the most sale or popular products--

SELECT StockCode,
Description,
SUM(Quantity) AS totle_sold
FROM sale
GROUP BY StockCode,Description
ORDER BY totle_sold DESC
LIMIT 10;

-- NOW WE WILL CALCULATE ORDERS WHICH HAVE QUANTITY > 100--

SELECT InvoiceNo,
Country,
SUM(Quantity) AS items_in_order
FROM sale
GROUP BY InvoiceNo, Country
HAVING items_in_order > 100;








