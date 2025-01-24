# Crowdfunding_ETL

## Data Analytics Bootcamp - Project 2

### **Team Members:**
- **Sean Schallberger**
- **Yadav Dhruvi**
- **Bryan Carney**

### **Project Timeline:**
- **Start Date:** January 23, 2025
- **Due Date:** January 30, 2025 (11:59 PM)

---

## **Overview**
This project focuses on building an **ETL (Extract, Transform, Load)** pipeline using Python, Pandas, and PostgreSQL. By extracting and transforming data, we will generate **CSV files**, create an **Entity-Relationship Diagram (ERD)**, define a **table schema**, and upload data to a PostgreSQL database. This process demonstrates end-to-end ETL pipeline development and database integration.

---

## **Project Objectives**
1. Extract data from Excel files.
2. Transform the data into a format suitable for database insertion.
3. Generate CSV files for different data entities.
4. Create a PostgreSQL database and corresponding tables.
5. Load data into the PostgreSQL database.
6. Verify the database contents.

---

## **Work Allocation**

### **Person A**
1. Create the **Category and Subcategory DataFrames**:
   - Extract data from `crowdfunding.xlsx`.
   - Create `category.csv` and `subcategory.csv`.
   - Ensure proper data formatting and validation.

2. **Database Integration**:
   - Create the database schema.
   - Define relationships, primary keys, and foreign keys.

---

### **Person B**
1. Create the **Campaign DataFrame**:
   - Extract and transform campaign-related data.
   - Format columns and ensure data consistency.
   - Export as `campaign.csv`.

2. **PostgreSQL Import**:
   - Load `campaign.csv` and verify table data.
   - Perform SQL queries to test data accuracy.

---

### **Person C**
1. Create the **Contacts DataFrame**:
   - Extract and transform data from `contacts.xlsx`.
   - Use dictionary methods or regular expressions for data extraction.
   - Export as `contacts.csv`.

2. **Database Testing**:
   - Import `contacts.csv` into the database.
   - Validate data using SQL queries and debug any issues.

---

## **Deliverables**

### **1. Extract and Transform Data**

#### **Category DataFrame**
- Columns:
  - `category_id`: Sequential entries (e.g., `cat1`, `cat2`, ...).
  - `category`: Unique category titles.
- Exported as **`category.csv`**.

#### **Subcategory DataFrame**
- Columns:
  - `subcategory_id`: Sequential entries (e.g., `subcat1`, `subcat2`, ...).
  - `subcategory`: Unique subcategory titles.
- Exported as **`subcategory.csv`**.

#### **Campaign DataFrame**
- Columns:
  - `cf_id`
  - `contact_id`
  - `company_name`
  - `description` (formerly `blurb`)
  - `goal` (float)
  - `pledged` (float)
  - `outcome`
  - `backers_count`
  - `country`
  - `currency`
  - `launch_date` (datetime)
  - `end_date` (datetime)
  - `category_id` (matches `category.csv`)
  - `subcategory_id` (matches `subcategory.csv`)
- Exported as **`campaign.csv`**.

#### **Contacts DataFrame**
- Columns:
  - `contact_id` (integer)
  - `first_name`
  - `last_name`
  - `email`
- Exported as **`contacts.csv`**.

### **2. Database Schema**
- **Database Name:** `crowdfunding_db`
- Tables:
  - `category`
  - `subcategory`
  - `campaign`
  - `contacts`
- Relationships and constraints are defined with **primary keys** and **foreign keys**.

### **3. PostgreSQL Integration**
- **Database Creation Script:** `crowdfunding_db_schema.sql`
- Imported CSV data into corresponding tables.
- Verified data insertion with `SELECT *` statements.

---

## **Instructions**

### **Setup**
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/Crowdfunding_ETL.git
   ```
2. Navigate to the project directory:
   ```bash
   cd Crowdfunding_ETL
   ```

### **Run the Project**

#### **ETL Pipeline**
- Open and run `ETL_Mini_Project_SSchallberger_BCarney_YDhruvi.ipynb` in Jupyter Notebook.

#### **Database Setup**
1. Start PostgreSQL.
2. Execute the schema script:
   ```bash
   psql -U username -d crowdfunding_db -f crowdfunding_db_schema.sql
   ```
3. Import CSV data into the database.
4. Verify table contents with SQL queries.

---

## **Project Timeline**
| Date       | Milestone                                   |
|------------|--------------------------------------------|
| Jan 23     | Project Kickoff                            |
| Jan 24     | Person A: Category and Subcategory DataFrames |
| Jan 25     | Person B: Campaign DataFrame               |
| Jan 26     | Person C: Contacts DataFrame               |
| Jan 27     | Person A: Database Schema and Integration  |
| Jan 28     | Testing and Debugging (All Members)        |
| Jan 29     | Final Documentation and Submission (All Members) |
