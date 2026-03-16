# AtliQ Hardware – SQL Business Insights Challenge
## Project Overview

- AtliQ Hardwares is one of the leading computer hardware producers in India and has expanded globally. However, the management team faced challenges in making quick, data-informed  decisions due to the lack of actionable insights.

- To address this, the Data Analytics Director Tony Sharma designed a SQL Challenge to evaluate candidates' technical and analytical abilities.

- In this project, I analyzed the company's sales and product data using SQL and created insights that can help top-level management make strategic decisions.

## Objective

- The goal of this project is to:

- Solve 10 business ad-hoc requests using SQL

- Extract meaningful business insights from raw data

- Present findings in a clear and visually engaging presentation

- Demonstrate both technical SQL skills and analytical thinking

## Tools & Technologies

- SQL (MySQL)

- Power BI (for visualization)

- Power-Query (for Data Transformarion)

- GitHub (project documentation)

## Dataset Description

- The dataset contains multiple tables related to the business operations of AtliQ Hardwares.

- Main tables include:
  
| Table Name | Type | Description |
|------------|------|-------------|
| dim_customer | Dimension Table | Stores customer details |
| dim_product | Dimension Table | Contains product information |
| fact_sales_monthly | Fact Table | Monthly sales transaction data |
| fact_manufacturing_cost | Fact Table | Manufacturing cost per product |
| fact_pre_invoice_deductions | Fact Table | Discounts before invoice |
| fact_gross_price | Fact Table | Product gross pricing |

- These datasets help analyze:

- [X] Product performance

- [X] Sales trends

- [X] Market performance

- [X] Manufacturing costs

- [X] Profitability insights
      

## Business Requests

- The management requested insights for the following 10 ad-hoc questions:

> 1. List markets in which "Atliq Exclusive" operates in the APAC region.

> 2. Percentage of unique product increase in 2021 vs 2020.

> 3. Report of unique product counts for each product segment.

> 4. Segment with the highest increase in unique products in 2021 vs 2020.

> 5. Products with the highest and lowest manufacturing costs.

> 6. Top 5 customers who received high average pre-invoice discounts in 2021 in India.

> 7. Monthly gross sales for customer "Atliq Exclusive".

> 8. Quarter with the maximum total sold quantity in 2020.
 
> 9. Channel contributing maximum gross sales in 2021 and percentage contribution.

> 10. Top 3 products in each division by sold quantity in 2021.

- All queries used to answer these questions are available in the SQL folder.

## Key Insights

- Some important findings from the analysis include:

> 1. Significant growth in unique products between 2020 and 2021

> 2. Certain product segments expanded more aggressively

> 3. A small number of customers receive higher discounts, impacting margins

> 4. Specific sales channels contribute the majority of revenue

> 5. Some products show high manufacturing cost but lower sales performance

- These insights can help management improve:

> 1. Pricing strategy

> 2. Product portfolio planning

> 3. Customer discount policies

> 4. Channel performance optimization
