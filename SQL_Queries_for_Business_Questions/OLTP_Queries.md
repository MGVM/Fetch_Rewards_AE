# SQL Queries for Business Questions Based on OLTP Data Model

This document contains SQL queries written against the **OLTP data model** to answer six key business questions. The queries leverage the normalized structure of the OLTP model, demonstrating how the design supports analytics despite being primarily designed for transactional purposes.

---

## **1. What are the top 5 brands by receipts scanned for the most recent month?**

### **Query**
```sql
WITH RecentMonth AS (
    SELECT MAX(MONTH(purchase_date)) AS recent_month, YEAR(purchase_date) AS recent_year
    FROM Receipts
)
SELECT 
    b.name AS brand_name,
    COUNT(r.receipt_id) AS receipts_scanned
FROM Receipts r
JOIN ReceiptItems ri ON r.receipt_id = ri.receipt_id
JOIN Brands b ON ri.brand_id = b.brand_id
JOIN RecentMonth rm ON MONTH(r.purchase_date) = rm.recent_month AND YEAR(r.purchase_date) = rm.recent_year
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
        COUNT(r.receipt_id) AS receipts_scanned,
        MONTH(r.purchase_date) AS month,
        YEAR(r.purchase_date) AS year,
        RANK() OVER (PARTITION BY MONTH(r.purchase_date), YEAR(r.purchase_date) ORDER BY COUNT(r.receipt_id) DESC) AS rank
    FROM Receipts r
    JOIN ReceiptItems ri ON r.receipt_id = ri.receipt_id
    JOIN Brands b ON ri.brand_id = b.brand_id
    WHERE MONTH(r.purchase_date) IN (MONTH(CURRENT_DATE), MONTH(CURRENT_DATE) - 1)
    GROUP BY b.name, MONTH(r.purchase_date), YEAR(r.purchase_date)
)
SELECT 
    recent.brand_name,
    recent.rank AS recent_month_rank,
    previous.rank AS previous_month_rank
FROM RankedBrands recent
JOIN RankedBrands previous 
    ON recent.brand_name = previous.brand_name AND recent.month = MONTH(CURRENT_DATE) AND previous.month = MONTH(CURRENT_DATE) - 1;
```

## **3. When considering average spend from receipts with 'Accepted' or 'Rejected' status, which is greater?**

### **Query**
```sql
SELECT 
    rewards_receipt_status,
    AVG(total_spent) AS average_spent
FROM Receipts
WHERE rewards_receipt_status IN ('Finished', 'Rejected')
GROUP BY rewards_receipt_status;
```

## **4. When considering the total number of items purchased from receipts with 'Accepted' or 'Rejected' status, which is greater?**

### **Query**
```sql
SELECT 
    r.rewards_receipt_status,
    SUM(ri.quantity) AS total_items
FROM Receipts r
JOIN ReceiptItems ri ON r.receipt_id = ri.receipt_id
WHERE r.rewards_receipt_status IN ('Finished', 'Rejected')
GROUP BY r.rewards_receipt_status;
```

## **5. Which brand has the most spend among users who were created within the past 6 months?**

### **Query**
```sql
WITH RecentUsers AS (
    SELECT user_id
    FROM Users
    WHERE created_date >= DATEADD(MONTH, -6, CURRENT_DATE)
)
SELECT 
    b.name AS brand_name,
    SUM(r.total_spent) AS total_spend
FROM Receipts r
JOIN ReceiptItems ri ON r.receipt_id = ri.receipt_id
JOIN Brands b ON ri.brand_id = b.brand_id
JOIN RecentUsers ru ON r.user_id = ru.user_id
GROUP BY b.name
ORDER BY total_spend DESC
LIMIT 1;
```

## **6. Which brand has the most transactions among users who were created within the past 6 months?**

### **Query**
```sql
WITH RecentUsers AS (
    SELECT user_id
    FROM Users
    WHERE created_date >= DATEADD(MONTH, -6, CURRENT_DATE)
)
SELECT 
    b.name AS brand_name,
    COUNT(r.receipt_id) AS total_transactions
FROM Receipts r
JOIN ReceiptItems ri ON r.receipt_id = ri.receipt_id
JOIN Brands b ON ri.brand_id = b.brand_id
JOIN RecentUsers ru ON r.user_id = ru.user_id
GROUP BY b.name
ORDER BY total_transactions DESC
LIMIT 1;
```
---

### Key Features:
1. **SQL Queries**:
   - Includes complete SQL queries for all six business questions, written specifically for the OLTP schema.

2. **Explanations**:
   - Describes how each query leverages the OLTP model’s structure to answer the question.

3. **Limitations Mentioned**:
   - Highlights the additional joins and aggregations required in OLTP, contrasting with the streamlined OLAP approach.
