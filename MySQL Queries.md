## Request 1
- 1) Provide the list of markets in which customer  "Atliq  Exclusive"  operates its business in the  APAC  region.
``` sql
SELECT 
	DISTINCT(market) as AtliQ_Exclusive_in_APAC_by_market
FROM dim_customer
WHERE 
	customer = "Atliq Exclusive" AND
	region = "APAC";
```
