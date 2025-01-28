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

[SQL Queries](SQL_Queries_for_Business_Questions)

---

## **3. Exploratory Data Analysis (EDA)**
This section contains detailed scripts and visualizations that analyze the datasets to uncover:
- Missing values, duplicates, and inconsistencies.
- Outliers and their alignment with business expectations.
- Distribution and trends across key numerical and categorical columns.

For the full EDA script and usage instructions, visit:
[EDA Folder](EDA)

---

## **4. Stakeholder Email**
This section includes a concise, business-friendly summary of the data quality review and next steps. Key findings include:
- Missing data and duplicates in critical columns (e.g., `role`, `state`, `barcode`).
- Outliers in numerical fields that require validation with business rules.
- Recommendations for addressing inconsistencies and scaling concerns.

---
## Usage

To replicate the analysis:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/MGVM/Fetch_Rewards_AE.git
   cd Fetch_Rewards_AE
   ```

2. **Install Dependencies: Ensure you have Python installed. Then, install the required packages**
   ```bash
   pip install pandas numpy matplotlib seaborn
   ```

3. **Python Libraries**
   - **pandas**: Used for data manipulation and analysis, including handling missing values, transforming data, and generating summary statistics.
   
   - **numpy**: Provides support for numerical operations and handling arrays, aiding in efficient computation.
   
   - **matplotlib**: A powerful library for creating static, animated, and interactive visualizations.
   
   - **seaborn**: Built on top of matplotlib, seaborn is used for creating attractive and informative statistical graphics.

---

This repository provides a comprehensive approach to analyzing, modeling, and querying data to solve business problems effectively. Please explore the respective sections for detailed insights.
