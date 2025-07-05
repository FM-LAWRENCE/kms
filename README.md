# growth-insights-kms-palmoria
# Kultra Mega Stores (KMS) - SQL & Excel Analysis Results

## 📁 Project Overview
This project combines insights from two major business intelligence analyses:

1. **Kultra Mega Stores (KMS)** – Office supply sales in Abuja (2009–2012)
2. **Palmoria Group** – HR data analysis across departments and regions in Nigeria

These analyses were conducted using SQL and Excel respectively to support data-driven decisions and guide organizational strategy and growth.

---

## 🤔 Case Scenario I - KMS Sales & Shipping Performance (SQL)

### 1. **Which product category had the highest sales?**
```sql
SELECT category, SUM(sales) AS total_sales
FROM orders_data
GROUP BY category
ORDER BY total_sales DESC
LIMIT 1;
```
**Result**: *Technology* had the highest total sales (₦5,984,248.18).

### 2. **Top 3 and Bottom 3 regions in terms of sales**
```sql
SELECT region, SUM(sales) AS total_sales
FROM orders_data
GROUP BY region
ORDER BY total_sales DESC;
```
**Result**:
- **Top 3**: West (₦3,597,549.27), Ontario (₦3,063,212.48), Prarie (₦2,837,304.61)
- **Bottom 3**: Nunavut (₦116,376.48), Northwest Territories (₦800,847.33), Yukon (₦975,867.38)

### 3. **Total sales of appliances in Ontario**
```sql
SELECT SUM(sales) AS appliance_sales_ontario
FROM orders_data
WHERE category = 'Appliances' AND region = 'Ontario';
```
**Result**: ₦202,346.84

### 4. **Advice to increase revenue from bottom 10 customers**
```sql
SELECT customer_id, customer_name, SUM(sales) AS total_sales
FROM orders_data
GROUP BY customer_id, customer_name
ORDER BY total_sales ASC
LIMIT 10;
```
**Recommendation**:
- Personalize promotions & discounts
- Follow-up with satisfaction surveys
- Assign relationship managers to drive upsells

### 5. **Most expensive shipping method**
```sql
SELECT ship_mode, SUM(shipping_cost) AS total_cost
FROM orders_data
GROUP BY ship_mode
ORDER BY total_cost DESC
LIMIT 1;
```
**Result**: *Delivery Truck* incurred the highest shipping cost (₦51,971.94).

### 6. **Most valuable customers and their typical purchases**
```sql
SELECT 
  RANK() OVER (ORDER BY SUM(Sales) DESC) AS [Rank],
  Customer_Name,
  '₦' + FORMAT(SUM(Sales), 'N2') AS [Total Sales (₦)]
FROM 
  orders_data
GROUP BY 
  Customer_Name
ORDER BY 
  SUM(Sales) DESC;
```
**Top 5 Customers**:
| Rank | Customer Name         | Total Sales (₦) |
| ---- | --------------------- | ---------------- |
| 1    | Emily Phan            | ₦117,124.44      |
| 2    | Deborah Brumfield     | ₦97,433.13       |
| 3    | Roy Skaria            | ₦92,542.15       |
| 4    | Sylvia Foulston       | ₦88,875.76       |
| 5    | Grant Carroll         | ₦88,417.00       |

### 7. **Small business customer with highest sales**
```sql
SELECT customer_name, SUM(sales) AS total_sales
FROM orders_data
WHERE segment = 'Small Business'
GROUP BY customer_name
ORDER BY total_sales DESC
LIMIT 1;
```
**Result**: *Dennis Kane* with total sales of ₦75,967.59

### 8. **Corporate customer with most orders (2009–2012)**
```sql
SELECT customer_name, COUNT(order_id) AS order_count
FROM orders_data
WHERE segment = 'Corporate'
GROUP BY customer_name
ORDER BY order_count DESC
LIMIT 1;
```
**Result**: *Adam Hart* with 27 orders

### 9. **Most profitable consumer customer**
```sql
SELECT customer_name, SUM(profit) AS total_profit
FROM orders_data
WHERE segment = 'Consumer'
GROUP BY customer_name
ORDER BY total_profit DESC
LIMIT 1;
```
**Result**: *Emily Phan* with a total profit of ₦34,005.44

### 10. **Customers who made loss-generating purchases and their segments**
```sql
SELECT DISTINCT customer_name, segment
FROM orders_data
WHERE profit < 0 OR sales < 0;
```
**Result**: Customers from all segments made loss-generating purchases.

### 11. **Shipping method vs. Order Priority assessment**
```sql
SELECT order_priority, ship_mode, COUNT(*) AS orders, SUM(shipping_cost) AS cost
FROM orders_data
GROUP BY order_priority, ship_mode
ORDER BY order_priority, cost DESC;
```
**Assessment**:
- High-priority orders used *Express Air* → aligns with urgency
- Low-priority orders used *Delivery Truck* → cost-efficient

**Growth Insight**:
- Consider optimizing logistics for Critical and High priority orders.
- Build loyalty programs around top-performing customers and segments.
- Expand successful product categories into underperforming regions.

---

## 🤔 Case Scenario II - Palmoria Group HR Analysis (Excel)

**File**: `Palmoria_HR.xlsx`  
[🔗 View the file (OneDrive)](https://1drv.ms/x/c/86640da3c29c30b7/EYJnZ38DathDjcSv8mN16FIBCJ2-b6pvt9nzzDxiX4fNEA?e=EedBHu)

### 🔍 Objectives
- Explore gender balance across departments
- Analyze regional workforce distribution
- Visualize employee segmentation using dashboards

### 📊 Tools Used
- Microsoft Excel
  - Functions: `IF`, `COUNTIF`, `VLOOKUP`
  - PivotTables
  - Bar, Pie, and Clustered Charts

### 📈 Key Insights
- Notable gender gaps exist in the Technical and Manufacturing departments.
- Female representation is stronger in Human Resources and Administration.
- Northern and Eastern zones show uneven departmental spread.

### 📌 Recommendations
- Promote inclusive hiring policies in male-dominated roles.
- Balance departmental staffing across zones for operational efficiency.
- Implement diversity dashboards to monitor progress.

**Growth-Driven Outlook**:
- Data shows readiness for strategic HR transformation.
- Integrating Excel dashboards into monthly reviews can highlight trends early.
- Use insights for employee retention and equitable promotions.

---

## 📌 Summary of Recommendations

### From KMS (Sales & Shipping):
- Focus marketing on high-performing categories (e.g., Technology)
- Target bottom-tier customers for engagement
- Monitor high shipping costs periodically
- Align delivery modes with order priority
- **Assessment**:
- High-priority orders used *Express Air* heavily → aligns with urgency
- Low-priority orders used *Delivery Truck* → cost-efficient

**Conclusion**: Shipping costs were appropriately aligned with priority levels.

---


### From Palmoria Group (HR):
- Improve gender diversity in skewed departments
- Use Excel dashboards for real-time HR tracking
- Inform policy with regional and departmental workforce trends

---

## 📂 Tools & Files
- **SQL & Excel**: Used for querying and data visualization
- `Palmoria_HR.xlsx`: Excel-based HR dashboard
- `/sql_queries/`: SQL scripts from KMS analysis

---

## 🔗 Author
**Utuedeye Lawrence Oluwafemi**  
Business Intelligence Analyst  
📧 utuedeyelawrence@gmail.com  
📍 Lagos, Nigeria

> "Transforming data into decisions that drive growth."
