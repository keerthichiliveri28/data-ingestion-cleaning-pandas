# Data Ingestion, Cleaning & Preprocessing with Pandas

## Project Overview

This project demonstrates an end-to-end data ingestion, cleaning,
validation, preprocessing, and feature-engineering workflow using
**Python and Pandas**.

A deliberately messy retail-sales dataset containing more than **10,000
transaction records** was processed to address common real-world
data-quality problems such as missing values, duplicate records,
inconsistent data types, inconsistent categorical values, and numerical
outliers.

The final result is a clean, standardized dataset ready for exploratory
data analysis, reporting, visualization, and business intelligence.

------------------------------------------------------------------------

## Objectives

The main objectives of this project are to:

-   Load a raw business dataset into Pandas.
-   Audit the dataset for quality issues.
-   Identify and handle missing values.
-   Detect and remove duplicate records.
-   Correct inconsistent data types and formats.
-   Standardize categorical values.
-   Identify and handle numerical outliers.
-   Engineer useful business features.
-   Validate the cleaned dataset.
-   Export the final dataset as `clean_dataset.csv`.

------------------------------------------------------------------------

## Dataset

The project uses a synthetic but realistic **Retail Sales Transactions**
dataset.

### Raw Dataset

-   **Rows:** 12,620
-   **Columns:** 15
-   Contains intentionally introduced data-quality issues.
-   Includes missing values, duplicates, mixed data types, inconsistent
    text formatting, and numerical outliers.

### Clean Dataset

-   **Rows:** 12,500
-   **Columns:** 19
-   **Missing cells after cleaning:** 0
-   **Duplicate rows after cleaning:** 0

------------------------------------------------------------------------

## Data Quality Issues Addressed

### 1. Missing Values

Missing values were identified using Pandas:

``` python
df.isna().sum()
```

Different strategies were used depending on the column:

-   Categorical columns → mode imputation
-   Customer IDs → `"UNKNOWN"`
-   Dates → median date
-   Numeric fields → median or business-rule-based imputation

------------------------------------------------------------------------

### 2. Duplicate Records

Duplicate transactions were detected using:

``` python
df.duplicated().sum()
```

Exact duplicate records were removed with:

``` python
df = df.drop_duplicates()
```

**120 duplicate rows were removed.**

------------------------------------------------------------------------

### 3. Incorrect Data Types

Several fields intentionally contained mixed formats.

Examples included:

-   Currency values such as `$1,250.00`
-   Quantities stored as strings
-   Discounts represented as percentages such as `15.5%`
-   Dates represented in different formats

These were converted into consistent Pandas data types using functions
such as:

``` python
pd.to_numeric()
pd.to_datetime()
```

------------------------------------------------------------------------

### 4. Inconsistent Categorical Values

Text fields were cleaned by removing unnecessary whitespace and
standardizing capitalization.

For example:

``` text
north
North
 NORTH
```

were standardized to:

``` text
North
```

The same process was applied to payment-method values and other
categorical fields.

------------------------------------------------------------------------

### 5. Outlier Handling

Unusually large transaction quantities were identified and handled using
percentile-based clipping.

The 1st and 99th percentiles were used as boundaries:

``` python
q_low, q_high = df["quantity"].quantile([0.01, 0.99])
df["quantity"] = df["quantity"].clip(q_low, q_high)
```

This prevents extreme values from disproportionately affecting
downstream analysis while retaining the transactions.

------------------------------------------------------------------------

## Feature Engineering

Several new business-oriented features were created.

### Year

Extracted from the transaction date:

``` python
df["year"] = df["transaction_date"].dt.year
```

### Month

Extracted from the transaction date:

``` python
df["month"] = df["transaction_date"].dt.month
```

### Year-Month

Created to support monthly trend analysis:

``` python
df["year_month"] = df["transaction_date"].dt.to_period("M")
```

### Profit Margin

Calculated using:

``` python
df["profit_margin"] = df["profit"] / df["sales"]
```

This provides a standardized measure of profitability across
transactions.

------------------------------------------------------------------------

## Cleaning Workflow

The overall pipeline follows this sequence:

``` text
Raw CSV
   ↓
Load with Pandas
   ↓
Initial Data Audit
   ↓
Standardize Column Names
   ↓
Clean Text / Categories
   ↓
Correct Data Types
   ↓
Remove Duplicates
   ↓
Handle Invalid Values & Outliers
   ↓
Impute Missing Values
   ↓
Recalculate Financial Metrics
   ↓
Feature Engineering
   ↓
Final Validation
   ↓
Export clean_dataset.csv
```

------------------------------------------------------------------------

## Before vs After

  Data Quality Measure             Before          After
  ------------------------- ------------- --------------
  Rows                             12,620         12,500
  Columns                              15             19
  Duplicate rows                      120              0
  Missing cells                   Present              0
  Mixed data types                Present   Standardized
  Inconsistent categories         Present   Standardized
  Engineered features         Not present          Added

------------------------------------------------------------------------

## Project Files

``` text
data-ingestion-cleaning-pandas/
│
├── data_ingestion_cleaning_preprocessing.ipynb
├── raw_retail_sales.csv
├── clean_dataset.csv
└── README.md
```

### File Descriptions

  -----------------------------------------------------------------------------------
  File                                            Description
  ----------------------------------------------- -----------------------------------
  `data_ingestion_cleaning_preprocessing.ipynb`   Complete Pandas cleaning and
                                                  preprocessing workflow

  `raw_retail_sales.csv`                          Original messy dataset

  `clean_dataset.csv`                             Cleaned and standardized dataset

  `README.md`                                     Project documentation
  -----------------------------------------------------------------------------------

------------------------------------------------------------------------

## Technologies Used

-   **Python**
-   **Pandas**
-   **NumPy**
-   **Jupyter Notebook**
-   **CSV**

------------------------------------------------------------------------

## How to Run the Project

### 1. Clone the repository

``` bash
git clone https://github.com/YOUR-USERNAME/data-ingestion-cleaning-pandas.git
cd data-ingestion-cleaning-pandas
```

### 2. Install dependencies

``` bash
pip install pandas numpy jupyter
```

### 3. Launch Jupyter Notebook

``` bash
jupyter notebook
```

Open:

``` text
data_ingestion_cleaning_preprocessing.ipynb
```

### 4. Run the notebook

Run the cells from top to bottom. The notebook loads
`raw_retail_sales.csv`, performs the cleaning and feature-engineering
steps, validates the result, and exports:

``` text
clean_dataset.csv
```

------------------------------------------------------------------------

## Validation

The final dataset was checked to ensure that:

-   No missing cells remain.
-   No exact duplicate rows remain.
-   Dates use a standardized format.
-   Numeric columns contain numeric values.
-   Categorical values are standardized.
-   Profit and sales calculations are internally consistent.
-   Engineered fields are present.
-   The cleaned CSV can be loaded successfully with Pandas.

Example validation:

``` python
clean = pd.read_csv("clean_dataset.csv")

print("Rows:", len(clean))
print("Missing cells:", clean.isna().sum().sum())
print("Duplicate rows:", clean.duplicated().sum())
```

Expected result:

``` text
Rows: 12500
Missing cells: 0
Duplicate rows: 0
```

------------------------------------------------------------------------

## Business Value

Data cleaning is a critical step before performing analytics or building
dashboards.

This project creates a reliable foundation for future analysis such as:

-   Monthly sales trends
-   Regional performance comparison
-   Product-category analysis
-   Customer purchase analysis
-   Profitability analysis
-   Return-rate analysis
-   Sales-channel comparison
-   KPI dashboard development

The resulting dataset can therefore be used as the input for subsequent
**Exploratory Data Analysis, statistical analysis, and interactive
dashboard projects**.

------------------------------------------------------------------------

## Author

**Keerthi Chiliveri**

Data Analytics & Business Intelligence

------------------------------------------------------------------------

## Project Status

**Completed**

The project satisfies the required deliverables:

-   [x] Raw dataset with 10,000+ records
-   [x] Data ingestion using Pandas
-   [x] Missing-value handling
-   [x] Duplicate removal
-   [x] Data-type correction
-   [x] Outlier handling
-   [x] Feature engineering
-   [x] Before/after validation
-   [x] Clean CSV export
-   [x] Jupyter Notebook documentation
