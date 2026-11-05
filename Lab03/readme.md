# Lab 03 · Pandas for Data Manipulation and Cleaning

**Course:** CS-3105 Machine Learning Lab  
**Instructor:** Engr. Muhammad Awais  
**Student Name:** Hasnain Malik  
**Roll Number:** 2024-SE-37  
**CLO 6 (C-4 Analyze, PLO 2 Problem Analysis):** Analyze real-world datasets by applying data manipulation and cleaning techniques.

---

## 📌 Lab Overview & Objectives
The primary objective of **Lab 03** is to transition from basic NumPy arrays to structured tabular data using **Pandas**. This lab focuses on loading real-world messy datasets, identifying data quality issues, applying systematic cleaning steps, and extracting meaningful statistical insights through vectorised operations.

* **Dataset:** `pakistan_used_cars.csv` (24,973 real-world used car ads from OLX Pakistan).
* **Final Cleaned Output:** `pakistan_used_cars_clean.csv` with a final shape of **(20484, 11)**.

---

## 🔍 Data Quality Issues & Cleaning Strategy
Real-world datasets contain anomalies that can severely distort machine learning models if left unaddressed. In Task 3, we detected and resolved **six major types of data problems** in a strict, logical sequence (text formatting and cleaning first, followed by deduplication):

| Problem Type | Description in Raw Dataset | Cleaning Method & Pandas Idiom | Impact on Analysis |
| :--- | :--- | :--- | :--- |
| **1. Missing Values** | Missing entries in `Brand`, `Model`, `City`, etc. | Dropped rows missing essential identifiers (`Brand`, `Model`); filled categorical columns (`Registered City`, `Fuel`, `Transaction Type`) with `"Unknown"`. | Prevents loss of valid rows while handling categorical gaps safely. |
| **2. Formatting Errors** | Extra whitespace, inconsistent capitalization in city names (`" MUZAFFARABAD"`), and price text strings (`"Rs 1,430,000"`). | Used `.str.strip()`, `.str.title()`, and string replacement (`.str.replace()`) to clean text and price tokens. | Standardizes categories and prepares text columns for numeric casting. |
| **3. Wrong Data Types** | `Price` stored as `object` (text) due to currency symbols and commas; `Year` stored as `float` due to NaNs. | Converted `Price` to `int64` using `pd.to_numeric()` and `Year` to nullable integer type `Int64`. | Enables mathematical comparisons, filtering, and aggregations without `TypeError`. |
| **4. Inconsistent Categories** | Typo variants such as `"Sukkar"` instead of `"Sukkur"` and `"Corrolla Altis"`. | Mapped and replaced incorrect category strings using `.replace()`. | Prevents category splitting during `groupby` aggregations. |
| **5. Duplicate Records** | Exact identical ad entries posted multiple times (2,028 duplicates). | Counted and removed exact duplicates using `.drop_duplicates()`. | Eliminates skewness and inflated frequency counts in statistical metrics. |
| **6. Invalid Values / Outliers** | Impossible kilometre values (< 100 or > 1,000,000, placeholders like `12345`), fake inflated prices (`12,345,678`), and luxury outliers. | Applied boolean masking to set invalid `KMs Driven` to `NaN` and dropped fake or heavily distorted price rows. | Protects downstream models and statistical summaries from extreme noise. |

---

## 🛠️ Summary of Tasks Performed

### **Task 1: Load and First Look**
* Imported `pandas` and loaded `pakistan_used_cars.csv` into DataFrame `df`.
* Verified initial dataset dimensions: **(24973, 9)**.
* Inspected data types and identified that the `Price` column was incorrectly stored as an `object` data type due to embedded currency symbols (`Rs`) and commas.

### **Task 2: Data Retrieval & Indexing**
* Extracted specific columns (`Brand`, `Model`, `Price`) and rows using `.loc[]` and `.iloc[]`.
* Filtered records using boolean masks to calculate specific queries (e.g., cars registered in Islamabad, Honda hybrid vehicles).
* Demonstrated why direct numerical comparison on uncleaned price columns results in a `TypeError`.

### **Task 3: Data Cleaning Pipeline**
* Executed the 6-step cleaning pipeline systematically.
* **Self-Check Verification:** Achieved the exact required final shape of **(20484, 9)** after cleaning.

### **Task 4: Statistical Analysis & Feature Engineering**
* **Feature Engineering:** Created `Price_lakh` (price divided by 100,000) and `Age` (`2020 − Year`).
* **Aggregations:** Analyzed top brands (Suzuki, Toyota, Honda leading by volume), top registered cities (Karachi leading), and median prices across fuel types (Hybrids commanding the highest median price).
* **Depreciation Analysis:** Tracked Corolla GLI median prices across model years (2010–2018), demonstrating a steady annual depreciation of roughly 1 to 1.5 lakh rupees.
* **Transaction Analysis:** Compared cash versus installment/leasing listings, highlighting market volume dominance on cash transactions.

### **Task 5: Persistence**
* Exported the processed DataFrame to `pakistan_used_cars_clean.csv` (`index=False`), ensuring complete readiness for Lab 04 (EDA & Visualization).

### **Task 6: Critical Reflection**
* Concluded that ignoring missing values or uncleaned category formatting leads to severely misleading aggregation groups and runtime exceptions. Emphasized that data cleaning is the foundational step of reliable machine learning workflows.

---

## 📂 Repository Structure
```text
ML-Lab-2024-SE-37/
└── Lab03/
    ├── Lab03.ipynb                      # Main Jupyter Notebook containing all solved tasks
    ├── pakistan_used_cars_clean.csv     # Cleaned dataset generated from Task 5
    └── README.md                        # Comprehensive documentation and lab summary
