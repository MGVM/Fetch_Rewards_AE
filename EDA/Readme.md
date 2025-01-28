# Exploratory Data Analysis (EDA) and Data Quality Checks

## Overview
This script performs **exploratory data analysis (EDA)** and **data quality checks** on three datasets:
- **Brands**: Contains brand-related data.
- **Receipts**: Holds receipt and transaction information.
- **Users**: Stores user data including registration and activity logs.

The script includes:
- Detection and conversion of UNIX timestamps to ISO 8601 format.
- Identification of missing values and potential data quality issues.
- Exploratory analysis to visualize trends, distributions, and anomalies in the datasets.

---

## Key Features

### 1. **Data Cleaning and Preprocessing**
- Converts UNIX timestamps (in milliseconds) to ISO 8601 format using the `convert_unix_to_timestamp` function.
- Handles missing or null fields with default values.
- Filters out invalid JSON lines while maintaining valid objects.

### 2. **Data Quality Analysis**
- **Brands**:
  - Identifies missing or null fields.
  - Validates category codes to ensure proper formatting.
  - Detects duplicate barcodes and brand names.
- **Receipts**:
  - Ensures critical fields like `userId`, `purchaseDate`, and `rewardsReceiptStatus` are present.
  - Checks consistency in `rewardsReceiptItemList`, such as duplicate items or invalid item prices.
  - Flags receipts with invalid or future dates.
- **Users**:
  - Identifies missing critical fields such as `active`, `createdDate`, and `role`.
  - Detects duplicate user records based on combinations of `createdDate` and `lastLogin`.

### 3. **Exploratory Data Analysis (EDA)**
- Visualizes missing values across columns to prioritize cleaning efforts.
- Analyzes numerical fields like `totalSpent` for receipts using histograms.
- Visualizes distributions of categorical fields such as `rewardsReceiptStatus` and `active` status.
- Examines date trends for user registrations and last login activities.

### 4. **Data Summarization**
- Creates summaries of numeric and categorical data.
- Highlights potential anomalies, such as mismatches between `purchased_item_count` and the actual number of items.

---

## Data Quality Metrics

### Missing Values
- The script identifies columns with missing data and calculates the percentage of missing values for each column. Results are visualized using bar plots.

### Duplicate Records
- Duplicate records in the **Users** table are flagged and analyzed based on unique user IDs and date combinations.

### Invalid Data
- Detects invalid or future dates in receipt and user records.
- Ensures data consistency in nested structures such as `rewardsReceiptItemList` for receipts.

---

## Outputs and Visualizations

### 1. **Missing Value Analysis**
   - Tabular and visual reports highlighting columns with missing data.
   - Bar plots showing the percentage of missing values by column.

### 2. **Numerical Data Distribution**
   - Histograms for fields like `totalSpent` and `pointsEarned`.

### 3. **Categorical Data Distribution**
   - Count plots for fields such as `rewardsReceiptStatus` and `topBrand`.

### 4. **Date Analysis**
   - Histograms showing user registration trends and last login activities.

### 5. **Duplicate Analysis**
   - Counts of duplicate records based on key fields, including `barcode` in brands and `id` in users.

---

## File Structure

- **Input Files**:
  - `brands.json`: Raw JSON data for brands.
  - `receipts_fixed.json`: Raw JSON data for receipts.
  - `users_fixed.json`: Raw JSON data for users.

- **Output Files**:
  - `structured_data/bss.json`: Cleaned and structured JSON data for brands.
  - `brand_data_quality_issues.csv`: Detailed report of brand-related quality issues.
  - `missing_value_summary.csv`: Summary of missing values across datasets.

---

## How to Use

### Prerequisites:
- Python 3.x
- Required libraries: `json`, `pandas`, `matplotlib`, `seaborn`

### Run the Script:
Execute the script using Python:
```bash
python eda_script.py
