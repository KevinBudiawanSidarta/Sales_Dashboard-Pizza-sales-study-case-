# 🍕 Pizza Sales Dashboard — Power BI

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Data Analytics](https://img.shields.io/badge/Data%20Analytics-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)

> An end-to-end business intelligence dashboard analyzing pizza sales performance across daily trends, monthly patterns, product categories, and size distribution — built in Power BI with data extracted from an SQL database.

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Problem Statement](#problem-statement)
3. [Data Source](#data-source)
4. [Data Preprocessing & Analysis Workflow](#data-preprocessing--analysis-workflow)
5. [KPI Metrics](#kpi-metrics)
6. [Dashboard Visualizations](#dashboard-visualizations)
7. [Key Insights](#key-insights)
8. [Recommendations](#recommendations)
9. [Conclusion](#conclusion)

---

## Introduction

This project presents a comprehensive **Pizza Sales Performance Dashboard** developed using Microsoft Power BI. The dashboard was designed to provide business stakeholders with a centralized, interactive view of sales data spanning the full calendar year of 2015 (January–December). By consolidating transactional data into intuitive visualizations, this project enables data-driven decision-making across sales strategy, inventory planning, and marketing operations.

The dashboard is structured around five core KPI cards and six analytical visualizations, covering everything from daily order fluctuations to the best and worst performing products by category. The end goal is to surface actionable insights that can directly inform business strategy and operational improvements.

---

## Problem Statement

The pizza business generates a large volume of transactional data daily. Without a structured analytical framework, identifying meaningful trends, evaluating product performance, or understanding customer ordering behavior becomes difficult and time-consuming.

This project addresses the following key business questions:

- What is the overall revenue performance and average order value across the year?
- On which days of the week and months of the year does order volume peak?
- Which pizza categories and sizes contribute most to total sales revenue?
- Which specific products are top performers, and which are consistently underperforming?
- How can the business leverage these insights to optimize its product portfolio and operational strategy?

---

## Data Source

| Attribute | Details |
|---|---|
| **Source Type** | SQL Relational Database |
| **Table** | `pizza_sales` |
| **Time Period** | January 2015 – December 2015 |
| **Extraction Method** | Direct SQL query export to Power BI via the native SQL Server connector |

The raw dataset contains transactional-level pizza order records. Each row represents an individual line item within an order and includes the following key fields:

| Column | Description |
|---|---|
| `order_id` | Unique identifier for each customer order |
| `order_date` | Date the order was placed |
| `pizza_name` | Name of the specific pizza ordered |
| `pizza_category` | Category classification (e.g., Classic, Supreme, Chicken, Veggie) |
| `pizza_size` | Size of the pizza ordered (S, M, L, XL, XXL) |
| `quantity` | Number of pizzas ordered per line item |
| `unit_price` | Price per individual pizza |
| `total_price` | Computed revenue for the line item (`quantity × unit_price`) |

---

## Data Preprocessing & Analysis Workflow

The data pipeline followed a structured Extract–Transform–Load (ETL) approach before dashboard development.

### 1. Data Extraction
Transactional records were extracted from the source SQL database using structured queries. The complete `pizza_sales` table was loaded directly into Power BI Desktop using the **SQL Server data connector**, ensuring a live and reproducible data pipeline.

### 2. Data Transformation (Power Query Editor)
Inside Power BI's Power Query Editor, the following transformations were applied:

- **Data Type Enforcement:** All columns were assigned correct data types — `order_date` as Date, `quantity` and pricing fields as numeric, and categorical fields as Text.
- **Calculated Date Columns:** Two derived columns were added to support time-series analysis:
  - `Order Day` — extracted the day-of-week name from `order_date` (e.g., Monday, Friday)
  - `Order Month` — extracted the month name from `order_date` (e.g., January, July)
- **Null & Duplicate Handling:** Rows with missing values in key fields (`order_id`, `total_price`, `pizza_name`) were identified and removed. Duplicate transaction records were de-duplicated.
- **Column Standardization:** Category and size labels were standardized for consistent grouping in visuals.

### 3. Data Modelling
Since all data resided in a single flat table (`pizza_sales`), no complex relational joins were required. The table was used directly as the primary fact table for all DAX measure calculations.

### 4. Measure Development (DAX)
All KPI metrics were created as explicit **DAX measures** within Power BI, ensuring accurate aggregation across all filter contexts (slicers, visual cross-filtering, drill-downs). Measures were organized in a dedicated measures table for maintainability.

### 5. Dashboard Design
Visualizations were built with an emphasis on clarity and usability. The layout was organized into logical sections — KPI summary cards at the top, trend charts in the center, and distribution/ranking visuals below — following standard BI dashboard design conventions.

---

## KPI Metrics

Five business-critical KPIs were defined and implemented as Power BI DAX measures. These metrics form the foundation of the dashboard's summary panel.

| KPI | DAX Measure Name | Description |
|---|---|---|
| **Total Revenue** | `Total Revenue` | Sum of all `total_price` values across all orders |
| **Average Order Value (AOV)** | `Avg Order value` | Total revenue divided by the number of distinct orders |
| **Total Items Sold** | `Total Pizzas Sold` | Sum of the `quantity` column across all line items |
| **Total Orders** | `Total Orders` | Count of distinct `order_id` values |
| **Average Items per Order** | `AVG Pizzas Per Order` | Total pizzas sold divided by total distinct orders |

These measures are displayed as interactive **KPI card visuals** at the top of the dashboard, providing stakeholders with an at-a-glance performance summary. All cards respond dynamically to any filter or slicer selections applied on the dashboard.

---

## Dashboard Visualizations

The dashboard is composed of six analytical visualizations, each designed to answer a specific business question.

### 1. Daily Trend for Total Orders
**Visual Type:** Clustered Column Chart  
**Fields:** `Order Day` (X-axis) × `Total Orders` (Y-axis)

Displays the total number of orders aggregated by day of the week. This visualization reveals intra-week ordering patterns and helps identify the busiest and quietest trading days for staffing and inventory planning.

---

### 2. Monthly Trend for Total Orders
**Visual Type:** Area Chart  
**Fields:** `Order Month` (X-axis) × `Total Orders` (Y-axis)

Tracks how order volume evolves across the twelve months of the year. The area chart format makes it easy to spot seasonal peaks, growth periods, and demand slowdowns at a glance.

---

### 3. % of Sales by Pizza Category
**Visual Type:** Donut Chart  
**Fields:** `pizza_category` (Legend) × `Total Revenue` (Values)

Shows the revenue share contributed by each pizza category (Classic, Supreme, Chicken, Veggie). This helps the business understand which categories are driving the most income and whether the product portfolio is well-balanced.

---

### 4. % of Sales by Pizza Size
**Visual Type:** Donut Chart  
**Fields:** `pizza_size` (Legend) × `Total Revenue` (Values)

Breaks down total revenue by pizza size (S, M, L, XL, XXL), providing insight into customer size preferences and guiding decisions on portion sizing and pricing strategies.

---

### 5. Top 5 Best-Selling Products by Category
**Visual Type:** Bar Chart (filtered to Top 5)  
**Fields:** `pizza_name` × `Total Pizzas Sold`, segmented by `pizza_category`

Highlights the five highest-performing pizzas by total units sold within each category. This view is critical for understanding which products should be promoted, featured, or used in combo deals.

---

### 6. Bottom 5 Worst-Selling Products by Category
**Visual Type:** Bar Chart (filtered to Bottom 5)  
**Fields:** `pizza_name` × `Total Pizzas Sold`, segmented by `pizza_category`

Surfaces the five lowest-performing pizzas by category. This visualization supports portfolio rationalization decisions — identifying products that may need reformulation, repricing, or discontinuation.

---

### 7. Sales by Pizza Category (Volume)
**Visual Type:** Funnel Chart  
**Fields:** `pizza_category` (Category) × `Total Pizzas Sold` (Values)

Visualizes the relative volume of pizzas sold across each category in a hierarchical funnel format, making the contribution gap between categories immediately apparent.

---

## Key Insights

### 📅 Busiest Days & Times
- **Weekends (Friday and Saturday)** record the highest order volumes across the week, with Friday in particular emerging as the single busiest day. This is consistent with consumer dining behavior where end-of-week meals are more likely to involve takeaway or delivery.
- Mid-week days (Tuesday–Wednesday) show noticeably lower order volumes, representing potential opportunities for targeted promotions.

### 📆 Monthly Order Trends
- Order volumes are highest in the **first half of the year**, peaking around **January through July**.
- A clear **decline in orders is observed from August onward**, with the second half of the year consistently underperforming relative to the first. This seasonal pattern may reflect back-to-school periods, changing consumer habits, or reduced promotional activity.

### 🍕 Sales Performance by Category
- **Classic pizzas** represent the most significant contributor to both total revenue and total units sold. This category's broad appeal across age groups and taste profiles makes it the anchor of the product portfolio.
- **Large-size pizzas** dominate sales by pizza size, accounting for the largest share of total revenue — suggesting that customers frequently order for groups or families rather than as individuals.

### 🏆 Top Performers
- The top 5 best-selling pizzas are concentrated within the **Classic and Supreme categories**, indicating strong customer loyalty to familiar flavor profiles.
- High-volume products tend to have straightforward ingredient combinations and are positioned at accessible price points.

### ⚠️ Underperformers
- The bottom 5 worst-selling pizzas span multiple categories, with some **specialty or premium variants** appearing consistently low in order counts.
- These products may suffer from lower visibility on menus, higher price points relative to perceived value, or limited awareness among the customer base.

---

## Recommendations

Based on the insights generated from the dashboard, the following strategic and operational recommendations are proposed:

**1. Capitalize on Peak Days with Targeted Promotions**  
Launch Friday and Saturday-specific promotions — such as combo deals, loyalty rewards, or limited-time offers — to maximize revenue during already high-traffic periods.

**2. Drive Mid-Week Demand**  
Introduce weekday deals (e.g., "Tuesday Two-for-One" or "Wednesday Family Bundle") to flatten the demand curve and improve kitchen utilization during slower periods.

**3. Address the Second-Half Sales Decline**  
Investigate the root cause of declining orders from August onward. Consider seasonal menu refreshes, targeted advertising campaigns, or back-to-school family meal bundles to sustain demand through Q3 and Q4.

**4. Double Down on Classic and Large-Size Offerings**  
Given that Classic pizzas and Large sizes dominate both volume and revenue, ensure these are prominently featured on the menu, in marketing materials, and in any bundled offerings. Explore flavor extensions within the Classic category to deepen engagement.

**5. Evaluate and Act on Underperforming Products**  
Conduct a cost-benefit analysis on the bottom 5 products. Options include:
  - Repricing or reformulating to improve perceived value
  - Increasing visibility through in-store promotions or digital menu placement
  - Discontinuing items with consistently low demand to reduce operational complexity and ingredient waste

**6. Leverage Category Data for Inventory Planning**  
Use the category and size distribution data to optimize ingredient purchasing and reduce waste. Classic and Large-format products should have higher stock priority given their sales dominance.

---

## Conclusion

This Pizza Sales Dashboard delivers a complete, data-driven view of one year of pizza business performance. By combining robust SQL-sourced data with carefully designed Power BI measures and visualizations, the project translates raw transactional records into clear, actionable business intelligence.

The analysis confirms that while the business demonstrates strong performance during weekends and the first half of the year, there is meaningful headroom to improve mid-week engagement and second-half revenue. The product portfolio is anchored by a reliable Classic category, but optimization of underperforming items and strategic sizing focus on Large pizzas present clear paths to incremental growth.

This dashboard is designed to serve as a living analytical tool — one that can be refreshed with new data and extended with additional filtering dimensions (e.g., by location, promotion type, or customer segment) as the business scales.

---


