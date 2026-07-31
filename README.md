# 📊 Sales Performance Dashboard — SQL to Power BI

An end-to-end data analytics project that extracts sales data from SQL Server, cleans and transforms it in Power Query, models it using a star schema, and visualizes it in an interactive Power BI dashboard.

---

## 🧭 Overview

This project analyzes sales performance across regions, products, and customers from 2017–2020, covering the full analytics pipeline — from raw relational data to a decision-ready dashboard.

**Key metrics tracked:**
- Total Revenue: $984.81M
- Sales Quantity: 2M units
- Average Order Value: $1.7M

---

## 🛠️ Tech Stack

| Layer | Tool |
|---|---|
| Data Source | SQL Server |
| Extraction | SQL |
| Transformation & Cleaning | Power Query (M) |
| Data Modeling | Power BI (Star Schema) |
| Calculations | DAX |
| Visualization | Power BI Desktop |

---

## 🔄 Step-by-Step Process

### Step 1: Data Extraction (SQL)
- Connected Power BI to SQL Server using **Get Data → SQL Server**
- Wrote SQL queries to pull relevant tables: sales transactions, products, customers, regions, and date data
- Filtered to the relevant date range (2017–2020) at the source to reduce load size

```sql
SELECT SaleID, ProductID, CustomerID, RegionID, OrderDate, Revenue, Quantity
FROM Sales
WHERE OrderDate BETWEEN '2017-01-01' AND '2020-12-31';
```

### Step 2: Data Cleaning (Power Query)
- **Removed duplicates** across all variable and dimension tables.
- **Standardized text fields**: trimmed whitespace, fixed inconsistent casing in region and product names
- **Fixed data types**: corrected date columns stored as text, numeric fields stored as strings
- **Removed unnecessary columns** not used in the model to reduce file size
- **Renamed columns** for clarity and consistency (e.g., `Qty` → `Quantity`)

### Step 3: Data Transformation (Power Query)
- Split combined columns (e.g., Region + City) into separate fields
- Created a dedicated **Date table** with Year, Month, Quarter columns for time intelligence
- Merged lookup tables (Product, Customer, Region) into clean dimension tables
- Applied conditional columns where needed (e.g., categorizing order size)

### Step 4: Data Modeling (Star Schema)
Structured the model with one central fact table and surrounding dimension tables to keep it fast, scalable, and simple to maintain:

```
                sales_Date
                    │
Sales_Products ── Sales_Transactions ── Sales_Customers
                    │
                Sales_Markets
```

- **Sales_Transaction**: transactional records (Revenue, Quantity, foreign keys)
- **Sales_Date / Sales_Products / Sales_Customers / Sales_Markets**: descriptive attributes
- Relationships set to **one-to-many**, single-direction filter flow from dimensions to fact
- Verified relationship cardinality and cross-filter direction to avoid ambiguous filtering

### Step 5: DAX Measures
Built explicit measures rather than relying on implicit aggregations:

```DAX
Total Revenue = SUM(Sales[Revenue])
Total Quantity = SUM(Sales[Quantity])
Avg Order Value = DIVIDE([Total Revenue], [Total Quantity])
YoY Revenue Growth = 
    DIVIDE(
        [Total Revenue] - CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Dim_Date[Date])),
        CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Dim_Date[Date]))
    )
```

### Step 6: Dashboard Design
- Added **Year** and **Region** slicers for dynamic filtering
- Built KPI cards: Revenue, Sales Quantity, Avg Order Value
- Created a Revenue Trend line chart (2018–2020)
- Built Revenue & Sales Quantity by Region bar charts
- Added Top 5 Products and Top 5 Customers rankings
- Applied a consistent dark theme with green accents for readability

---

## 📌 Key Insights

- **Revenue decline**: Revenue peaked at $43M in early 2018 and trended down to $15M by early 2020.
- **Regional concentration**: Delhi NCR alone drives over half of total revenue and quantity.
- **Customer concentration**: A single customer, Electricalsara Stores, accounts for $0.41bn in revenue.
- **Data quality fix**: Resolved a broken fact-to-dimension join that had been inflating an unmapped `(Blank)` product category.

---

## ✅ Recommendations

1. Investigate the root cause of the 2018–2020 revenue decline.
2. Diversify regional focus to reduce dependency on Delhi NCR.
3. Reduce customer concentration risk by growing the secondary customer base.
4. Establish ongoing data quality checks on fact-dimension relationships.


---

## 📷 Dashboard Preview

<img width="565" height="334" alt="image" src="https://github.com/user-attachments/assets/5da7a8d3-c0af-40ad-93b2-f5ab28e02b1a" />


<img width="599" height="341" alt="Screenshot 2026-07-24 183323" src="https://github.com/user-attachments/assets/fe2bc24a-d822-4bda-b559-2b26abfdda0d" />


