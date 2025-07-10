# 📊 Sales Insights Data Analysis Project
This project shows how raw sales data from a MySQL database can be transformed and visualised with Power BI.  
It contains the original database dump, a cleaned version, curated SQL analyses, and an interactive dashboard.

---

## 📁 Project Structure

| File | Description |
|------|-------------|
| `sales_dashboard.pbix` | Power BI dashboard file containing visuals and insights |
| `raw_sales_data_dump.sql` | Original unprocessed sales database dump |
| `sales_db_cleaned.sql` | Cleaned version of the sales database for Power BI |
| `README.md` | Documentation of the project setup, SQL logic, and dashboard features |

---

## ▶️ How to Use the Power BI Report
1. **Open** `sales_dashboard.pbix` in Power BI Desktop.  
2. **Update** the data‑source settings to point to your MySQL server.  
3. **Refresh** the data and explore visuals, filters, and KPIs.

---

## 🔧 Setup Instructions
1. **Install MySQL** on your local machine.  
2. **Import the database**  
   - Use **`raw_sales_data_dump.sql`** for the untouched dataset.  
   - Use **`sales_db_cleaned.sql`** for the cleaned and normalised dataset.  
3. **Connect Power BI Desktop** to your local MySQL instance *or* run the SQL queries below for ad‑hoc exploration.

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
- **Revenue trends** by year, market, and currency  
- **Product & customer segmentation** with dynamic slicers  
- **Currency normalisation** for INR ↔ USD comparison  

### 💱 Example Power Query Step (Currency Normalisation)
```powerquery
= Table.AddColumn(#"Filtered Rows", "norm_amount",
    each if [currency] = "USD" or [currency] = "USD#(cr)"
         then [sales_amount] * 75   // FX‑rate placeholder
         else [sales_amount],
    type number)
