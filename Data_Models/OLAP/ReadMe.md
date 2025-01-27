# OLAP Star Schema Model Documentation

This document outlines the **OLAP Star Schema model** design, its structure, and possible future enhancements. The schema is optimized for analytical querying and reporting, making it an essential component for addressing key business questions.

---

## **Star Schema Design**

### **Fact Table: Receipts_Fact**
The **Receipts_Fact** table stores transactional data with references to related dimension tables for analytical insights.

- **Fields**:
  - `receipt_id` (Primary Key): Unique identifier for each transaction.
  - `user_id` (Foreign Key): Links to `Users_Dim`.
  - `brand_id` (Foreign Key): Links to `Brands_Dim`.
  - `date_id` (Foreign Key): Links to `Date_Dim`.
  - `bonus_points_earned`: Number of bonus points awarded.
  - `points_earned`: Total points earned from the receipt.
  - `total_spent`: Total amount spent.
  - `purchased_item_count`: Total items purchased in the transaction.
  - `rewards_receipt_status`: Status of the receipt (e.g., Finished, Rejected).

---

### **Dimension Tables**

#### **1. Users_Dim**
- **Purpose**: Provides demographic and behavioral attributes of users.
- **Fields**:
  - `user_id` (Primary Key): Unique identifier for each user.
  - `state`: User's state of residence.
  - `created_date`: Date the user account was created.
  - `last_login`: Date the user last logged in.
  - `role`: Role of the user (e.g., CONSUMER).
  - `active`: Boolean indicating whether the user is active.

#### **2. Brands_Dim**
- **Purpose**: Describes brand metadata.
- **Fields**:
  - `brand_id` (Primary Key): Unique identifier for each brand.
  - `barcode`: Barcode associated with the brand.
  - `brand_code`: Code representing the brand.
  - `category`: Category of the brand (e.g., Snacks, Beverages).
  - `category_code`: Unique code for the category.
  - `name`: Name of the brand.
  - `top_brand`: Boolean indicating whether the brand is a top-tier brand.

#### **3. Date_Dim**
- **Purpose**: Provides detailed time-based attributes for analysis.
- **Fields**:
  - `date_id` (Primary Key): Unique identifier for each date.
  - `full_date`: Complete date (e.g., 2023-12-31).
  - `year`: Year of the date.
  - `month`: Month of the date.
  - `day`: Day of the date.
  - `day_name`: Name of the day (e.g., Monday).
  - `is_weekend`: Boolean indicating if the date falls on a weekend.
  - `is_holiday`: Boolean indicating if the date is a holiday.

---

## **Assessment of the Data Model**

### **Structure and Suitability**
The proposed star schema model is well-structured and capable of answering the given business questions due to its logical organization of fact and dimension tables, enabling efficient querying and analysis.

### **Strengths**
1. **Query Optimization**:
   - Denormalized structure reduces the complexity of joins, enabling faster analytical queries.
2. **Scalability**:
   - The model can accommodate additional measures or dimensions (e.g., geographic or promotional data) without significant restructuring.
3. **Ease of Use**:
   - Simplifies business logic for stakeholders using reporting tools like Tableau or Power BI.

### **Potential Enhancements**
- **Incorporating a `CPGs_Dim` Table**:
  - Adding a dimension for Consumer Packaged Goods (CPG) entities could support hierarchical analysis at the CPG level.
- **Pre-Aggregated Metrics**:
  - Including pre-aggregated metrics in `Receipts_Fact` (e.g., monthly totals) for frequently used queries could further optimize performance.

---

## **Conclusion**

The OLAP star schema model is specifically designed to support analytical querying and reporting. Its structure ensures that it meets business requirements efficiently while remaining scalable for future enhancements. By combining well-defined fact and dimension tables, the model provides actionable insights across multiple dimensions of analysis.
