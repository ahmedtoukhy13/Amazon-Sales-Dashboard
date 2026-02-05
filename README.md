# 🛍️ Amazon Sales Dashboard – Power BI Project

## 📌 Overview
This project analyzes Amazon sales data using Power BI and DAX, focusing on customer segments, product categories, and time-based trends. It includes dynamic KPIs, interactive visuals, and custom measures to extract actionable insights.

---

## 🧹 Data Preparation

Performed in **Power Query Editor** to ensure clean and analysis-ready data:

- **Split Column**  
  Separated `order_date` into Date and Time using delimiter and `Time.From()`.

- **Transform → Date Only**  
  Extracted pure date values for time-based grouping.

- **Add Column → Custom Column**  
  Created derived fields like `Year`, `Month Name`, `Quarter`.

- **Change Type**  
  Set appropriate data types:
  - `Date` → Date  
  - `Time` → Time  
  - `Year` → Whole Number

### 📅 Year Extraction Logic

To extract the year from `order_date`, I used:

```powerquery
Date.Year([order_date])
This method was chosen over Split Column because the date format (yyyy-mm-dd) doesn’t isolate the year cleanly.
Using Date.Year ensures:
- Accurate numeric output
- Easier use in time-based analysis
- Cleaner visuals and filters
✅ This reflects a deliberate choice for precision and best practices in Power BI..

---

## 🧠 DAX Measures

Here are the key measures used in the dashboard:

1. **Total Revenue**  
   `SUM(Table2[TotalPrice])`

2. **Total Quantity**  
   `SUM(Table2[TotalQuantity])`

3. **Average Order Value**  
   ` DIVIDE([Total Revenue],DISTINCTCOUNT(Table2[order_id]))`

4. **Customer Count**  
   `DISTINCTCOUNT(Table2[user_id])`

5. **High Value Orders**  
   ```
CALCULATE(
    COUNT(Table2[order_id]),
    Table2[Order_segment] = "High-Value"
   ```

6. **Average Items per Order**  
   `DIVIDE([Total Quantity],DISTINCTCOUNT(Table2[order_id]))`

7. **Revenue_Percentage**  
   ```DAX
  DIVIDE(
    SUM(Table2[TotalPrice]),
    CALCULATE(SUM(Table2[TotalPrice]), ALL(Table2[Category]))
) * 100
   ```
✅ Project Improvements (What We Changed Together)

1) Order vs Customer Segmentation (Important Fix)
- The raw Excel column customer_segment is an order-level label (based on order value), not a true customer segmentation.
- In Power BI, it was renamed to: order_segment to avoid confusion.
  
2) Built True Customer Segmentation (Customer-Level)
- Created a new table CustomerSummary (one row per user_id) to aggregate:
- TotalRevenue per customer
- Orders per customer
- TotalQty per customer
- Calculated Customer AOV at the customer level:
- CustomerAOV = TotalRevenue / Orders
- Classified customers into: Low / Mid / High using CustomerAOV thresholds.
✅ Result: Each customer now has one consistent segment (does not change per order).

3) Relationship (Model Fix)
- Created the correct relationship to apply customer segments across visuals:
- CustomerSummary[user_id] (1) → Table2[user_id] (*)
  
4) Dashboard Update (Using the Correct Segment)
- All segment slicers/visuals now use: CustomerSummary[CustomerSegment]
- Table2[order_segment] is kept only for order-level analysis.
  
5) Month Order Fix (Jan → Dec)
- Created MonthNumber and used Sort by column so Month Name sorts chronologically.
- Updated the visual sorting to Month Name (Ascending) instead of sorting by quantity.
---
## 📊 Business Analysis Summary

### 📆 Time-Based Analysis
- **Monthly Quantity Distribution**: Identified seasonal peaks in sales volume.  
- **Year-over-Year Performance**: Measured growth and performance consistency over time.  
- **Revenue vs Price Trends**: Analyzed how pricing impacts total revenue across years.  
📍**Goal**: Understand sales evolution, highlight strong periods, and detect growth patterns.

### 🏷️ Category Breakdown
- **Revenue % by Category: Shows which categories contribute the most to total revenue.

- **- Revenue by Category: Identifies the highest-revenue categories.  
- **Total Quantity by Category: Shows which categories lead in units sold.
📍**Goal**: Identify high-value categories and those with strong sales potential.

### 👥 Customer Segment Analysis
- **High-Value Orders Count**:Counts customers in the High customer segment based on Customer AOV.
- **Segment-Wise Performance**: Evaluates how each customer segment performs.
- **Dynamic Filters**: Year, Quarter, Segment, Country, and Category for interactive analysis.  
📍**Goal**:Gain insight into customer behavior and segment-specific performance

---

## 📈 Key Business Insights
- Electronics dominate revenue share (≈40%).  
- Clothing leads in total quantity sold.  
- 2023 was the strongest year in terms of performance.  
- Average order size is relatively low, suggesting mostly individual purchases.  
- Books & Toys underperform, indicating potential for marketing improvements.  
📍**Takeaway**: Focus on maintaining electronics and clothing growth while boosting weaker categories through targeted campaigns.

---

## 🛠 Tools Used
- **Power BI** – for data visualization and dashboard creation  
- **Power Query** – for data cleaning and transformation  
- **DAX** – for creating custom analytical measures  
- **Excel** – for initial data review and validation

---

## 📎 Repository Contents
- `Amazon_Sales_Dashboard.pbix` – Power BI file  
- `Amazon_Sales_Data.xlsx` – Raw data  
- `README.md` – Project documentation  
- `/Power BI Screenshots/` – Dashboard visuals
