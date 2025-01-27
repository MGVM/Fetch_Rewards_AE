# OLTP Data Model Documentation

This document outlines the **OLTP Data Model** design, its structure, relationships, and the potential for future enhancements. It also includes a high-level ETL process to transform data into an OLAP (Star Schema) model for analytical purposes.

---

## **Tables in OLTP Data Model**

### **1. Users**
- **Purpose**: Stores user information.
- **Fields**:
  - `user_id` (Primary Key): Unique identifier for each user.
  - `state`: User's state of residence.
  - `created_date`: Date when the user account was created.
  - `last_login`: Date of the user's last login.
  - `role`: Role of the user (e.g., CONSUMER).
  - `active`: Boolean indicating if the user is currently active.

---

### **2. Receipts**
- **Purpose**: Tracks transactional data and links to users.
- **Fields**:
  - `receipt_id` (Primary Key): Unique identifier for each receipt.
  - `user_id` (Foreign Key): Links to `Users`.
  - `bonus_points_earned`: Number of bonus points awarded.
  - `bonus_points_earned_reason`: Reason for awarding bonus points.
  - `create_date`: Date when the receipt was created.
  - `date_scanned`: Date when the receipt was scanned.
  - `finished_date`: Date when the receipt process was completed.
  - `modify_date`: Date when the receipt record was last modified.
  - `points_awarded_date`: Date when points were awarded.
  - `points_earned`: Total points earned from the receipt.
  - `purchase_date`: Date of the transaction.
  - `purchased_item_count`: Number of items purchased.
  - `rewards_receipt_status`: Status of the receipt (e.g., Finished, Rejected).
  - `total_spent`: Total amount spent on the receipt.

---

### **3. Brands**
- **Purpose**: Contains metadata about brands.
- **Fields**:
  - `brand_id` (Primary Key): Unique identifier for each brand.
  - `barcode`: Barcode associated with the brand.
  - `brand_code`: Unique code for the brand.
  - `category`: Category of the brand (e.g., Snacks, Beverages).
  - `category_code`: Unique code for the category.
  - `cpg_id` (Foreign Key): Links to `CPGs`.
  - `name`: Name of the brand.
  - `top_brand`: Boolean indicating if the brand is a top-tier brand.

---

### **4. ReceiptItems**
- **Purpose**: Stores item-level details of receipts.
- **Fields**:
  - `item_id` (Primary Key): Unique identifier for each item.
  - `receipt_id` (Foreign Key): Links to `Receipts`.
  - `barcode`: Barcode of the item.
  - `description`: Description of the item.
  - `final_price`: Final price after discounts.
  - `item_price`: Original price of the item.
  - `points_earned`: Points earned for the item.
  - `quantity`: Quantity purchased.
  - `brand_id` (Foreign Key): Links to `Brands`.

---

## **Future Enhancements: CPGs Table**

### **Proposed Table: CPGs**
- **Purpose**: Represents Consumer Packaged Goods (CPG) entities.
- **Fields**:
  - `cpg_id` (Primary Key): Unique identifier for each CPG entity.
  - `name`: Name of the CPG entity.
  - `industry`: Industry or sector of the CPG.
  - `region`: Geographic region covered by the CPG.

### **Benefits of Adding CPGs Table**:
1. **Data Normalization**:
   - Prevents duplication of CPG-related data in the `Brands` table.
2. **Enhanced Analysis**:
   - Supports CPG-level reporting and aggregation for analytics.
3. **Scalability**:
   - Facilitates the addition of new fields (e.g., CPG-specific metadata) without modifying the `Brands` table.
4. **Improved Query Performance**:
   - Allows efficient joins when filtering or aggregating data by CPG.

---

## **ETL Transformation Steps**

### **Overview**
The ETL process transforms the normalized OLTP schema into a denormalized OLAP star schema for analytical purposes. Below are the high-level steps:

1. **Extract**:
   - Pull data from the OLTP tables: `Users`, `Receipts`, `Brands`, and `ReceiptItems`.
   - Ensure data integrity by validating foreign key relationships and date formats.

2. **Transform**:
   - **Receipts_Fact**:
     - Aggregate `points_earned`, `bonus_points_earned`, and `total_spent` for each `receipt_id`.
     - Map `user_id`, `brand_id`, and `purchase_date` to the corresponding dimension tables.
   - **Users_Dim**:
     - Extract `user_id`, `state`, `created_date`, `last_login`, `role`, and `active`.
   - **Brands_Dim**:
     - Extract brand metadata (`brand_id`, `name`, `category`, etc.).
   - **Date_Dim**:
     - Derive date attributes (e.g., year, month, day) from fields like `purchase_date` and `create_date`.

3. **Load**:
   - Populate the OLAP schema with transformed data:
     - Insert metrics into `Receipts_Fact`.
     - Insert descriptive attributes into `Users_Dim`, `Brands_Dim`, and `Date_Dim`.

---

## **Schema Diagram**

### OLTP Model:
- Includes `Users`, `Receipts`, `Brands`, and `ReceiptItems`.
- Future enhancements: `CPGs` table for brand grouping.

### OLAP Model:
- Star schema with `Receipts_Fact` at the center and `Users_Dim`, `Brands_Dim`, and `Date_Dim` as dimensions.

---

## **Conclusion**

This OLTP model is designed to ensure data integrity and support efficient transactional operations. By transforming it into a star schema through the outlined ETL process, it becomes optimized for analytical querying and reporting. The potential addition of a `CPGs` table can enhance scalability and enable hierarchical analysis.
