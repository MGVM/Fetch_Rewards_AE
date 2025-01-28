# Fetch Rewards Coding Exercise - Analytics Engineer

## **1. Data Model**
This folder contains the data models for:
- **Transactional Database (OLTP)**: Includes the schema design for efficient day-to-day operations with detailed documentation.
- **Analytical Database (OLAP)**: A star schema optimized for reporting and analytical queries. The folder includes explanations on how the schema addresses business questions effectively.

[Data Model Folder](Data_Models)

---

## **2. SQL Queries**
This folder contains SQL scripts to solve key business questions, including:

1. **Top Brands Analysis**:
   - Identify the top 5 brands by receipts scanned for the most recent month.
   - Compare the rankings of the top 5 brands between the recent and previous months.

2. **Rewards Receipt Status Analysis**:
   - Compare the average spend for receipts with a `rewardsReceiptStatus` of 'Accepted' vs. 'Rejected'.
   - Determine whether the total number of items purchased differs between 'Accepted' and 'Rejected' receipts.

3. **Brand Analysis for Recent Users**:
   - Identify the brand with the highest spend among users created within the past 6 months.
   - Find the brand with the most transactions for the same user segment.

[SQL Queries](https://github.com/MGVM/Fetch_Rewards_AE/SQL_Queries)

---

## **3. Exploratory Data Analysis (EDA)**
This section contains detailed scripts and visualizations that analyze the datasets to uncover:
- Missing values, duplicates, and inconsistencies.
- Outliers and their alignment with business expectations.
- Distribution and trends across key numerical and categorical columns.

For the full EDA script and usage instructions, visit:
[EDA Folder](https://github.com/MGVM/Fetch_Rewards_AE/EDA)

---

## **4. Stakeholder Email**
This section includes a concise, business-friendly summary of the data quality review and next steps. Key findings include:
- Missing data and duplicates in critical columns (e.g., `role`, `state`, `barcode`).
- Outliers in numerical fields that require validation with business rules.
- Recommendations for addressing inconsistencies and scaling concerns.

---

This repository provides a comprehensive approach to analyzing, modeling, and querying data to solve business problems effectively. Please explore the respective sections for detailed insights.
