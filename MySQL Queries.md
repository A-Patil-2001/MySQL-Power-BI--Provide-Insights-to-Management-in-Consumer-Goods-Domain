## Request 1
> 1) Provide the list of markets in which customer  "Atliq  Exclusive"  operates its business in the  APAC  region.
``` sql
SELECT 
	DISTINCT(market) as AtliQ_Exclusive_in_APAC_by_market
FROM dim_customer
WHERE 
	customer = "Atliq Exclusive" AND
	region = "APAC";
```

## Request 2
> 2) What is the percentage of unique product increase in 2021 vs. 2020? The final output contains these fields, 
```
-- unique_products_2020 
-- unique_products_2021 
-- percentage_chg
```
``` sql
-- USING CTE
WITH cte1 AS (SELECT 
					 COUNT(DISTINCT CASE WHEN fiscal_year = 2020 THEN p.product_code END) as unique_product_2020,
					 COUNT(DISTINCT CASE WHEN fiscal_year = 2021 THEN p.product_code END) as unique_product_2021
				FROM dim_product p 
				JOIN fact_sales_monthly s
				ON p.product_code = s.product_code)
SELECT 
	*,
    ROUND((unique_product_2021 - unique_product_2020)/unique_product_2020 * 100,2) as percentage_chg
 FROM cte1;
 
 -- USING SUB-QUERY
 SELECT 
	X.A AS unique_product_2020, 
	Y.B AS unique_product_2021,
    ROUND((B-A)*100 / A,2) AS percentage_chg
FROM(
	(SELECT COUNT(distinct product_code) AS A 
	FROM fact_sales_monthly
	WHERE fiscal_year = 2020) X,
	(SELECT COUNT(distinct product_code) AS B 
	FROM fact_sales_monthly
	WHERE fiscal_year = 2021) Y			
);
```
