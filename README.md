# Gross Sales Report for AtliQ Hardware (SQL)

Generating a financial report for AtliQ Hardware as tasked by the product owner of the company.

> **Course project.** AtliQ Hardware is a fictional company used in the Codebasics SQL course, and the dataset comes from that course. The queries, the user-defined functions and the write-up in this repo are my own work.

**Tool:** MySQL &nbsp;|&nbsp; **Techniques:** joins, user-defined functions, fiscal-year date logic

Full documentation: [Notion write-up](https://www.notion.so/Atliq-Hardware-169f74a7e49d808da9c2c85a49781edc?pvs=4)

## Business question

The product owner wants to understand sales to **Croma India** for **fiscal year 2021** (September 2020 to August 2021). The report answers two questions:

1. **Which products did Croma India buy each month, and what was their gross sales value?**
   A report of individual product sales, aggregated monthly at the product code level.
2. **How much did Croma India spend in total each month?**
   A monthly view of total gross sales to Croma India.

## Business Model

- Atliq Hardware - Let us understand the  domain before generating the report
    
    Atliq is a consumer goods company that sells hardware like PC, mouse, printers, so on to different customers. These stores than sells the hardwares to the consumers.
    
    For Atliq the customer is the stores, consumer is consuming the product.
    
    The company has its manufacturing unit from there it goes to warehouses and shipped to different countries.
    
    There are two types of customers for Atliq
    
    - Brick & Mortar : Croma, Best Buy, etc
    - E-Commerce: Amazon, Flipkart

<details>
<summary><b>Understanding Concept of Financial Statements (P&L)</b></summary>

- Gross Price - Total price of a product before any discounts or deduction.
**Example:** Atliq Hardware sells a laptop for ₹50,000.
    
    **Gross Price = ₹50,000**
    
- Pre-Invoice Deduction - These are discounts or rebates applied before the invoice is generated.
    
    **Example:** A customer gets a **10% discount** on the laptop.
    
    **Pre-Invoice Deduction = ₹5,000**
    
    **Price After Pre-Invoice Deduction = ₹50,000 - ₹5,000 = ₹45,000**
    
- Post-Invoice Deduction - These are deductions applied after the invoice, such as promotional discount or adjustments.
    
    **Example:** After the invoice, Atliq offers an additional **₹2,000 cashback** for a festive sale.
    **Post-Invoice Deduction = ₹2,000**
    
    **Price after deduction = ₹45,000 - ₹2,000 = ₹43,000**
    
- Net Sales - This is the actual amount Atliq Hardware receives after all deductions (both pre-invoice and post-invoice).
    
    **Example: Net Sales = ₹43,000**
    
- COGS (Cost of Goods Sold) - This is the cost Atliq Hardware incurs to produce or purchase the laptop. It includes material, labor, and overhead costs.
    
    **Example:** The cost to produce the laptop is **₹30,000.**
    
    **COGS = ₹30,000**
    
- Profit or Gross Margin - This is the profit made after subtracting the cost of goods sold (COGS) from Net Sales.
    
     **Example: Gross Margin = Net Sales - COGS = ₹43,000 - ₹30,000 = ₹13,000**
    
- Gross Margin Percentage - This shows how profitable Atliq Hardware is on this sale as a percentage of Net Sales.
    
    **Formula: Gross Margin % = (Gross Margin ÷ Net Sales) × 100**
    
    **Example: Gross Margin % = (₹13,000 ÷ ₹43,000) × 100 ≈ 30.23%**
</details>

 ## Task

1. Report 1- The report should have the following fields
    - Month
    - Product Name
    - Variant
    - Sold Quantity
    - Gross Price per Item
    - Gross Price Total
2. Gross monthly total sales report for Croma - The report should have the following fields,
    - Month
    - Total gross sales amount to Croma India in this month
  
## Data model

Sales link to customers and products on their codes, and to prices on product code plus fiscal year.

<img width="958" alt="Data model" src="https://github.com/user-attachments/assets/9532b31b-9a38-469c-b652-8e0c8443df72" />

 
## Techniques and Functions Used

- To focus exclusively on **Croma**, customer codes were extracted from the `dim_customer` table by applying a filter that identifies customers whose names contain "Croma" and are part of the **Indian market**. The SQL `LIKE` operator was used for pattern matching, ensuring flexibility in identifying relevant records.
- **Converting Calendar Year to Fiscal Year**
    - Atliq Hardware follows a **fiscal year starting in September** (September = Month 1 for the company).
    - To handle this, the SQL function `DATE_ADD()` was used to shift calendar dates, and the `YEAR()` function extracted fiscal years.

- Additionally I Created a User-defined function named `get_fiscal_year()` for frequent reuse.
- **Detailed Sales Analysis for FY**
    - A `User-defined` SQL function `get_fiscal_quarter()`was implemented to focus on specific fiscal quarters (e.g., Q4).

- **Joins**
    - Data from the `fact_sales_monthly` table was joined with the `dim_product` table to include product names and variants.
    - A join with the `fact_gross_price` table provided the gross price per item and calculated the total gross price for each transaction.

## Report output

- **SQL workings:** [Finance_Analytics.sql](1_sql/Finance_Analytics.sql), with all queries in run order and comments on each step
- **Final report:** [Croma_2021_AllTransactionReport.csv](2_results/Croma_2021_AllTransactionReport.csv), 3,006 rows covering every product and month for Croma India in FY2021
 
## Summary

- This project demonstrates my ability to:
    - Analyze customer-specific sales data for defined timeframes.
    - Use SQL efficiently by creating reusable **user-defined functions (UDFs)**.
    - Perform advanced data transformations and **joins** to enrich datasets.
    - Deliver comprehensive **gross sales reports** with accurate metrics.
- **Tools Used**: Joins, User-defined functions, In-built function such as round, year, date_add, etc
