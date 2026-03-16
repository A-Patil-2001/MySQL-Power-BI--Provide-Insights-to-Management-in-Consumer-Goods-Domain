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
- unique_products_2020 
- unique_products_2021 
- percentage_chg
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

## Request 3
> 3) Provide a report with all the unique product counts for each  segment  and sort them in descending order of product counts. 
The final output contains 2 fields, 
- segment 
- product_count
``` sql
SELECT 
	segment,
	COUNT(DISTINCT product_code) AS product
FROM dim_product 
GROUP BY segment
ORDER BY product DESC;
```

## Request 4
> 4. Follow-up: Which segment had the most increase in unique products in 2021 vs 2020? The final output contains these fields, 
- segment 
- product_count_2020 
- product_count_2021 
- difference
``` sql
WITH cte2 AS (SELECT 
					segment,
					COUNT(DISTINCT CASE WHEN fiscal_year = 2020 THEN p.product_code END) as unique_product_2020,
					COUNT(DISTINCT CASE WHEN fiscal_year = 2021 THEN p.product_code END) as unique_product_2021
				FROM dim_product p
				JOIN fact_sales_monthly s
				ON p.product_code = s.product_code
				GROUP BY segment)
SELECT 
	*,
    unique_product_2021 - unique_product_2020 AS difference
FROM cte2;
```

# Request 5
> 5. Get the products that have the highest and lowest manufacturing costs. The final output should contain these fields, 
- product_code 
- product 
- manufacturing_cost
``` sql
-- USING CTE
WITH cte3 AS (SELECT 
					p.product_code,
					p.product,
					SUM(m.manufacturing_cost) AS manufacturing_cost 
				FROM dim_product p
				JOIN fact_manufacturing_cost m
				ON p.product_code = m.product_code
                WHERE m.cost_year = 2021
				GROUP BY p.product_code, p.product)
SELECT 
	*
FROM cte3
WHERE manufacturing_cost = (SELECT 
								MAX(manufacturing_cost)
							FROM cte3)
                            OR
	   manufacturing_cost = (SELECT 
                                MIN(manufacturing_cost)
							FROM cte3)
ORDER BY manufacturing_cost DESC;

-- USING SUB-QUERY
SELECT 
	p.product_code,
	p.product,
	SUM(m.manufacturing_cost) AS manufacturing_cost
FROM dim_product p
JOIN fact_manufacturing_cost m
ON p.product_code = m.product_code
WHERE manufacturing_cost IN (SELECT MAX(manufacturing_cost) FROM fact_manufacturing_cost
							 UNION
                             SELECT MIN(manufacturing_cost) FROM fact_manufacturing_cost)
GROUP BY p.product_code, p.product
ORDER BY manufacturing_cost DESC;
```

# Request 6
> 6. Generate a report which contains the top 5 customers who received an average high pre_invoice_discount_pct  for the  fiscal  year 2021  and in the Indian  market. The final output contains these fields, 
- customer_code 
- customer 
- average_discount_percentage
``` sql
SELECT 
	c.customer_code,
    c.customer,
    ROUND(AVG(pre.pre_invoice_discount_pct),2) AS average_discount_percentage 
FROM dim_customer c
JOIN fact_pre_invoice_deductions pre
ON c.customer_code = pre.customer_code
WHERE fiscal_year = 2021 AND market = "India"
GROUP BY c.customer_code, c.customer
ORDER BY average_discount_percentage DESC
LIMIT 5;
```

# Request 7
> 7. Get the complete report of the Gross sales amount for the customer  “Atliq Exclusive”  for each month . This analysis helps to  get an idea of low and high-performing months and take strategic decisions. 
> The final report contains these columns: 
- Month 
- Year 
- Gross sales Amount 
*/
 SELECT 
	CONCAT(MONTHNAME(s.date), ' (', YEAR(s.date), ')') AS month,
    s.fiscal_year,
    ROUND(SUM(s.sold_quantity * g.gross_price),2) AS gross_sales_mln
FROM fact_sales_monthly s
JOIN fact_gross_price g
ON 	
	s.product_code = g.product_code AND
    s.fiscal_year = g.fiscal_year
JOIN dim_customer c
ON 
	s.customer_code = c.customer_code
WHERE c.customer = "Atliq Exclusive"
GROUP BY month,s.fiscal_year
ORDER BY s.fiscal_year;
