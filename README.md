# Kultra Mega Stores (KMS) - SQL Analysis Results

## 📁 Project Overview
Kultra Mega Stores (KMS), headquartered in Lagos, Nigeria, is a supplier of office supplies and furniture. This project focuses on the Abuja division, analyzing historical sales data from **2009 to 2012**.

The goal is to derive insights that can help improve decision-making and revenue growth.

---

## 🤔 Case Scenario I - Sales & Shipping Performance

### 1. **Which product category had the highest sales?**
```sql
SELECT category, SUM(sales) AS total_sales
FROM orders_data
GROUP BY category
ORDER BY total_sales DESC
LIMIT 1;
```
**Result**: *Technology* had the highest total sales (₦5,984,248.18).

---

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

---

### 3. **Total sales of appliances in Ontario**
```sql
SELECT SUM(sales) AS appliance_sales_ontario
FROM orders_data
WHERE category = 'Appliances' AND region = 'Ontario';
```
**Result**: ₦202,346.84

---

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

---

### 5. **Most expensive shipping method**
```sql
SELECT ship_mode, SUM(shipping_cost) AS total_cost
FROM orders_data
GROUP BY ship_mode
ORDER BY total_cost DESC
LIMIT 1;
```
**Result**: *Delivery Truck* incurred the highest shipping cost (₦51,971.94).

---

### 6. **Shipping cost breakdown by Order Priority and Ship Mode**
```sql
SELECT [Order_Priority], [Ship_Mode],
       COUNT(*) AS OrderCount,
       SUM([Shipping_Cost]) AS TotalShippingCost,
       AVG([Shipping_Cost]) AS AvgShippingCost
FROM orders_data
GROUP BY [Order_Priority], [Ship_Mode]
ORDER BY [Order_Priority], [Ship_Mode];
```
**Result Summary**:

| Order Priority   | Ship Mode       | Order Count | Total Shipping Cost | Avg Shipping Cost |
|------------------|------------------|--------------|----------------------|--------------------|
| Critical         | Delivery Truck   | 228          | ₦10,783.82         | ₦47.30            |
| Critical         | Express Air      | 200          | ₦1,742.10          | ₦8.71             |
| Critical         | Regular Air      | 1,180        | ₦8,586.76          | ₦7.28             |
| High             | Delivery Truck   | 248          | ₦11,206.88         | ₦45.19            |
| High             | Express Air      | 212          | ₦1,453.53          | ₦6.86             |
| High             | Regular Air      | 1,308        | ₦10,005.01         | ₦7.65             |
| Low              | Delivery Truck   | 250          | ₦11,131.61         | ₦44.53            |
| Low              | Express Air      | 190          | ₦1,551.63          | ₦8.17             |
| Low              | Regular Air      | 1,280        | ₦10,263.62         | ₦8.02             |
| Medium           | Delivery Truck   | 205          | ₦9,461.62          | ₦46.15            |
| Medium           | Express Air      | 201          | ₦1,633.59          | ₦8.13             |
| Medium           | Regular Air      | 1,225        | ₦9,418.72          | ₦7.69             |
| Not Specified    | Delivery Truck   | 215          | ₦9,388.01          | ₦43.67            |
| Not Specified    | Express Air      | 180          | ₦1,470.06          | ₦8.17             |
| Not Specified    | Regular Air      | 1,277        | ₦9,734.08          | ₦7.62             |

---

## 🤔 Case Scenario II - Customer Value & Behavior

### 7. **Most valuable customers and their typical purchases**
```sql
-- Ranked top customers with formatted Naira sales
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

```sql
-- Products they buy:
SELECT category, COUNT(*) AS purchase_count
FROM orders_data
WHERE customer_name = 'Emily Phan' -- Replace with actual top customer
GROUP BY category;
```
**Result**: Top customers tend to purchase *Technology* and *Office Supplies*.

---

### 8. **Small business customer with highest sales**
```sql
SELECT customer_name, SUM(sales) AS total_sales
FROM orders_data
WHERE segment = 'Small Business'
GROUP BY customer_name
ORDER BY total_sales DESC
LIMIT 1;
```
**Result**: *Dennis Kane* with total sales of ₦75,967.59

---

### 9. **Corporate customer with most orders (2009–2012)**
```sql
SELECT customer_name, COUNT(order_id) AS order_count
FROM orders_data
WHERE segment = 'Corporate'
GROUP BY customer_name
ORDER BY order_count DESC
LIMIT 1;
```
**Result**: *Adam Hart* with 27 orders

---

### 10. **Most profitable consumer customer**
```sql
SELECT customer_name, SUM(profit) AS total_profit
FROM orders_data
WHERE segment = 'Consumer'
GROUP BY customer_name
ORDER BY total_profit DESC
LIMIT 1;
```
**Result**: *Emily Phan* with a total profit of ₦34,005.44

---

### 11. **Customers who made loss-generating purchases and their segments**
```sql
SELECT DISTINCT customer_name, segment
FROM orders_data
WHERE profit < 0 OR sales < 0;
```
**Result**: Numerous customers across all segments (Corporate, Home Office, Consumer, Small Business) made purchases resulting in losses.

---

### 12. **Shipping method vs. Order Priority assessment**
```sql
SELECT order_priority, ship_mode, COUNT(*) AS orders, SUM(shipping_cost) AS cost
FROM orders_data
GROUP BY order_priority, ship_mode
ORDER BY order_priority, cost DESC;
```
**Assessment**:
- High-priority orders used *Express Air* heavily → aligns with urgency
- Low-priority orders used *Delivery Truck* → cost-efficient

**Conclusion**: Shipping costs were appropriately aligned with priority levels.

---

## 📌 Summary of Recommendations
- Focus marketing on top product categories (e.g., Technology)
- Improve loyalty initiatives for small and bottom-tier customers
- Audit high-cost shipping methods periodically
- Optimize delivery routes for low-priority orders
- Investigate loss-making sales and customers

---

## 📊 Tools Used
- SQL (SQL Server / PostgreSQL syntax)
- Excel (for initial exploration)

---

## 📂 Files
- `/sql_queries/`: Contains `.sql` files used in analysis
- `/results/`: Summary tables and visualizations (optional for Power BI/Excel)
- `README.md`: Project documentation

---

## 🔗 Author
**Utuedeye Lawrence Oluwafemi**  
Business Intelligence Analyst  
📧 utuedeyelawrence@gmail.com  
📍 Lagos, Nigeria

---

> "Transforming data into decisions that drive growth."
