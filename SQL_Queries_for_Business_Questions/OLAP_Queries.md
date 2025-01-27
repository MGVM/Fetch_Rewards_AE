# SQL Queries for Business Questions Based on OLAP Data Model

This document contains SQL queries written against the **OLAP Star Schema model** to answer six key business questions. The star schema is optimized for analytical queries, enabling efficient and intuitive data retrieval.

---

## **1. What are the top 5 brands by receipts scanned for the most recent month?**

### **Query**
```sql
WITH RecentMonth AS (
    SELECT MAX(date_id) AS recent_date_id
    FROM Date_Dim
    WHERE full_date <= CURRENT_DATE
),
RecentMonthData AS (
    SELECT DISTINCT year, month
    FROM Date_Dim
    WHERE date_id = (SELECT recent_date_id FROM RecentMonth)
)
SELECT 
    b.name AS brand_name,
    COUNT(f.receipt_id) AS receipts_scanned
FROM Receipts_Fact f
JOIN Brands_Dim b ON f.brand_id = b.brand_id
JOIN Date_Dim d ON f.date_id = d.date_id
WHERE d.year = (SELECT year FROM RecentMonthData)
  AND d.month = (SELECT month FROM RecentMonthData)
GROUP BY b.name
ORDER BY receipts_scanned DESC
LIMIT 5;
```

## **2. How does the ranking of the top 5 brands by receipts scanned for the recent month compare to the ranking for the previous month?**

### **Query**
```sql
WITH RankedBrands AS (
    SELECT 
        b.name AS brand_name,
        COUNT(f.receipt_id) AS receipts_scanned,
        d.month,
        d.year,
        RANK() OVER (PARTITION BY d.year, d.month ORDER BY COUNT(f.receipt_id) DESC) AS rank
    FROM Receipts_Fact f
    JOIN Brands_Dim b ON f.brand_id = b.brand_id
    JOIN Date_Dim d ON f.date_id = d.date_id
    WHERE d.month IN (MONTH(CURRENT_DATE), MONTH(CURRENT_DATE) - 1)
    GROUP BY b.name, d.month, d.year
)
SELECT 
    recent.brand_name,
    recent.rank AS recent_month_rank,
    previous.rank AS previous_month_rank
FROM RankedBrands recent
JOIN RankedBrands previous 
    ON recent.brand_name = previous.brand_name 
   AND recent.month = MONTH(CURRENT_DATE) 
   AND previous.month = MONTH(CURRENT_DATE) - 1;
```

## **3. When considering average spend from receipts with 'Accepted' or 'Rejected' status, which is greater?**

### **Query**
```sql
SELECT 
    f.rewards_receipt_status,
    AVG(f.total_spent) AS average_spent
FROM Receipts_Fact f
WHERE f.rewards_receipt_status IN ('Finished', 'Rejected')
GROUP BY f.rewards_receipt_status;
```

## **4. When considering the total number of items purchased from receipts with 'Accepted' or 'Rejected' status, which is greater?**

### **Query**
```sql
SELECT 
    f.rewards_receipt_status,
    SUM(f.purchased_item_count) AS total_items
FROM Receipts_Fact f
WHERE f.rewards_receipt_status IN ('Finished', 'Rejected')
GROUP BY f.rewards_receipt_status;
```

## **5. Which brand has the most spend among users who were created within the past 6 months?**

### **Query**
```sql
WITH RecentUsers AS (
    SELECT user_id
    FROM Users_Dim
    WHERE created_date >= DATEADD(MONTH, -6, CURRENT_DATE)
)
SELECT 
    b.name AS brand_name,
    SUM(f.total_spent) AS total_spend
FROM Receipts_Fact f
JOIN Users_Dim u ON f.user_id = u.user_id
JOIN Brands_Dim b ON f.brand_id = b.brand_id
WHERE f.user_id IN (SELECT user_id FROM RecentUsers)
GROUP BY b.name
ORDER BY total_spend DESC
LIMIT 1;
```

## **6. Which brand has the most transactions among users who were created within the past 6 months?**

### **Query**
```sql
WITH RecentUsers AS (
    SELECT user_id
    FROM Users_Dim
    WHERE created_date >= DATEADD(MONTH, -6, CURRENT_DATE)
)
SELECT 
    b.name AS brand_name,
    COUNT(f.receipt_id) AS total_transactions
FROM Receipts_Fact f
JOIN Users_Dim u ON f.user_id = u.user_id
JOIN Brands_Dim b ON f.brand_id = b.brand_id
WHERE f.user_id IN (SELECT user_id FROM RecentUsers)
GROUP BY b.name
ORDER BY total_transactions DESC
LIMIT 1;
```
---

### Key Features:
---

### Key Features of the README:
1. **SQL Queries**:
   - Provides complete, efficient queries written for the OLAP star schema.

2. **Explanations**:
   - Clearly describes how the OLAP model answers each business question.

3. **Advantages of the Star Schema**:
   - Highlights how the denormalized structure enhances query performance and usability.
