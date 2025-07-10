# sales-insights-dashboard
Interactive Power BI dashboard providing detailed sales insights, performance trends, and key business KPIs.
# Sales Insights Data Analysis Project

This project demonstrates how raw sales data from a MySQL database can be transformed and visualised with Power BI.  
It contains the original database dump, a cleaned version, example SQL analyses, and an interactive Power BI dashboard.

---

## 🔧 Setup Instructions

1. **Install MySQL** on your local machine.  
2. **Import the database**  
   - For the untouched dataset use `raw_sales_data_dump.sql`.  
   - For the prepared dataset (cleaned & normalised) use `sales_db_cleaned.sql`.  
3. **Connect Power BI Desktop** to your local MySQL instance or run the SQL queries below for ad‑hoc exploration.

---

## 🧮 SQL Analysis Examples

| # | Purpose | Query |
|---|---------|-------|
| 1 | All customers | `SELECT * FROM customers;` |
| 2 | Total customers | `SELECT COUNT(*) FROM customers;` |
| 3 | Transactions – Chennai (`Mark001`) | `SELECT * FROM transactions WHERE market_code = 'Mark001';` |
| 4 | Distinct products sold in Chennai | `SELECT DISTINCT product_code FROM transactions WHERE market_code = 'Mark001';` |
| 5 | Transactions in USD | `SELECT * FROM transactions WHERE currency = 'USD';` |
| 6 | 2020 transactions (join on date) | `SELECT t.*, d.* FROM transactions t JOIN date d ON t.order_date = d.date WHERE d.year = 2020;` |
| 7 | Total revenue 2020 | `SELECT SUM(t.sales_amount) FROM transactions t JOIN date d ON t.order_date = d.date WHERE d.year = 2020 AND t.currency IN ('INR','USD');` |
| 8 | Revenue January 2020 | `SELECT SUM(t.sales_amount) FROM transactions t JOIN date d ON t.order_date = d.date WHERE d.year = 2020 AND d.month_name = 'January' AND t.currency IN ('INR','USD');` |
| 9 | 2020 revenue – Chennai | `SELECT SUM(t.sales_amount) FROM transactions t JOIN date d ON t.order_date = d.date WHERE d.year = 2020 AND t.market_code = 'Mark001';` |

---

## 📊 Power BI Dashboard Highlights

- **Revenue trends** by year, market and currency  
- **Product & customer segmentation** with dynamic slicers  
- **Currency normalisation** for INR ↔ USD comparison  

### Example Power Query step (currency normalisation)

```powerquery
= Table.AddColumn(#"Filtered Rows", "norm_amount",
    each if [currency] = "USD" or [currency] = "USD#(cr)"
         then [sales_amount] * 75        // FX rate placeholder
         else [sales_amount],
    type number)

