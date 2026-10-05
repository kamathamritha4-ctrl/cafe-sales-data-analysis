# ☕ Cafe Sales Data Cleaning & Exploratory Analysis

An end-to-end data cleaning and exploratory analysis project using a deliberately messy cafe sales dataset containing 10,000 transactions.

The project focuses on identifying real-world data quality problems, systematically cleaning the dataset, recovering missing values where they could be logically derived, validating the final dataset, and extracting actionable business insights.

---

## 📌 Project Overview

Real-world datasets are rarely clean.

This project uses a deliberately messy cafe sales dataset to simulate common data-quality problems such as:

- Missing values
- Invalid categorical values
- Incorrect data types
- Missing financial information
- Invalid/unparseable dates
- Incomplete product information
- Missing payment and location information

Rather than simply removing incomplete rows, the cleaning process used business rules and relationships between columns to recover values where possible.

The cleaned dataset was then analyzed to understand product performance, sales trends, transaction behavior, payment methods, and sales locations.

---

## 🎯 Objectives

### Data Cleaning
- Identify and quantify data-quality issues
- Standardize invalid values such as `ERROR` and `UNKNOWN`
- Convert columns to appropriate data types
- Validate transaction IDs
- Identify duplicate records
- Recover missing prices using known menu prices
- Recover missing quantities using total spending and unit price
- Recover missing totals using quantity × unit price
- Recover unambiguous missing product values
- Validate transaction dates
- Preserve values that could not be reliably recovered rather than making unsupported assumptions

### Exploratory Data Analysis
- Analyze revenue by product
- Analyze units sold by product
- Identify high-volume and high-revenue products
- Analyze monthly revenue trends
- Analyze revenue by day of week
- Examine payment-method performance
- Compare in-store and takeaway sales

---

## 🗂️ Dataset

The dataset contains **10,000 cafe transactions** and the following original columns:

| Column | Description |
|---|---|
| `transaction_id` | Unique transaction identifier |
| `item` | Product purchased |
| `quantity` | Number of units purchased |
| `price_per_unit` | Price of one unit |
| `total_spent` | Total transaction amount |
| `payment_method` | Payment method used |
| `location` | In-store or takeaway |
| `transaction_date` | Transaction date |

Source: Kaggle — Cafe Sales Dirty Data for Cleaning Training

---

# 🧹 Data Cleaning Process

## 1. Initial Data Quality Assessment

The original dataset was inspected for:

- Missing values
- Duplicate records
- Invalid numeric values
- Invalid transaction IDs
- Incorrect data types
- Inconsistent categorical values
- Financial inconsistencies
- Invalid dates

---

## 2. Standardizing Missing Values

Values such as:

- `ERROR`
- `UNKNOWN`

were standardized to `NaN` so that missing and invalid values could be handled consistently.

---

## 3. Data Type Conversion

Numeric columns were converted to appropriate numeric types:

- `quantity`
- `price_per_unit`
- `total_spent`

The transaction date column was converted to Pandas datetime format.

Categorical columns were stored as categorical data where appropriate.

---

## 4. Financial Data Recovery

The dataset contained relationships that allowed some missing financial values to be recovered.

### Price recovery

Known menu prices were used to recover missing prices when the product was known.

### Quantity recovery

When total spending and unit price were available:

`Quantity = Total Spent / Price Per Unit`

Only valid quantities within the expected range were accepted.

### Total recovery

When quantity and unit price were available:

`Total Spent = Quantity × Price Per Unit`

### Validation

After cleaning, complete financial records were checked against the business rule:

`Total Spent = Quantity × Price Per Unit`

No financial mismatches remained among records where all required financial fields were available.

---

## 5. Missing Product Recovery

Some missing product values could be recovered when the unit price uniquely identified a product.

Values that could not be determined with sufficient confidence were left missing.

This avoided introducing unsupported assumptions into the dataset.

---

## 6. Date Validation

Transaction dates were converted to datetime values.

The final dataset contained:

- 159 originally missing dates
- 301 invalid/unparseable date values converted to `NaT`
- 460 missing dates after cleaning

Invalid dates were not artificially reconstructed because there was no reliable information available to determine their correct values.

---

# ✅ Final Data Quality

After cleaning:

| Quality Check | Result |
|---|---:|
| Rows | 10,000 |
| Columns | 8 |
| Duplicate transaction IDs | 0 |
| Duplicate rows | 0 |
| Missing transaction IDs | 0 |
| Invalid quantities | 0 |
| Invalid prices | 0 |
| Financial mismatches | 0 |
| Unresolved financial rows | 26 |
| Missing items | 480 |
| Missing dates | 460 |
| Missing payment methods | 3,178 |
| Missing locations | 3,961 |

Not all missing values were imputed.

Values were recovered only when they could be determined using reliable business rules or existing information. Remaining missing values were retained as missing rather than filled with unsupported assumptions.

---

# 📊 Exploratory Data Analysis

## Overall Performance

The cleaned dataset contains:

- **10,000 transactions**
- **30,180 items sold**
- **89,096 total sales**
- **8.93 average transaction value**

---

## Product Performance

### Highest Revenue Product

**Salad**

- Revenue: 19,095
- Revenue share: 21.43%

### Highest Volume Product

**Coffee**

- Units sold: 3,904

This demonstrates an important distinction between **sales volume and revenue contribution**.

Coffee had the highest number of units sold, but Salad generated substantially more revenue because of its higher unit price.

---

## Monthly Sales

Monthly revenue remained relatively stable throughout the year.

Among transactions with valid dates:

- Highest revenue month: **June — 7,353**
- Lowest revenue month: **February — 6,644**

The relatively small difference between the strongest and weakest months suggests limited seasonality in this dataset.

---

## Day-of-Week Performance

Among transactions with valid dates:

- Highest revenue day: **Thursday — 12,401.50**
- Lowest revenue day: **Wednesday — 11,680.50**

Revenue was relatively consistent throughout the week, with no dramatic weekday/weekend effect.

---

## Payment Methods

Among transactions with known payment methods:

| Payment Method | Revenue Share |
|---|---:|
| Credit Card | 33.40% |
| Digital Wallet | 33.31% |
| Cash | 33.29% |

The three payment methods were almost perfectly balanced.

However, approximately 31.8% of payment-method values were missing, so these percentages describe only transactions with known payment methods.

---

## Location

Among transactions with known locations:

| Location | Revenue Share |
|---|---:|
| In-store | 50.58% |
| Takeaway | 49.42% |

Known in-store and takeaway sales were almost evenly split.

Approximately 39.6% of location values were missing, so this comparison should be interpreted with caution.

---

# 💡 Key Business Insights

1. **Salad is the strongest revenue-generating product**, contributing more than one-fifth of total recorded sales.

2. **Coffee is the highest-volume product**, showing that popularity by units sold does not necessarily translate into the highest revenue.

3. **Monthly revenue is relatively stable**, with no strong seasonal pattern visible in the available dated transactions.

4. **Sales are relatively consistent across the week**, although Thursday recorded the highest revenue among known dates.

5. **Payment methods are highly balanced**, suggesting customers use cash, cards, and digital wallets at similar rates among records with known payment information.

6. **In-store and takeaway revenue are nearly evenly divided**, indicating that neither channel dominates the known location data.

---

# ⚠️ Data Limitations

The analysis has several limitations:

- 460 transaction dates are missing after cleaning.
- 480 transactions have missing product information.
- 3,178 payment-method values are missing.
- 3,961 location values are missing.
- 26 transactions still contain unresolved financial information.
- Missing values were not artificially imputed when no reliable recovery method was available.
- Therefore, analyses involving dates, payment methods, locations, or products may represent only the subset of records with available information.

---

# 🛠️ Tools & Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Google Colab
- Jupyter Notebook
- GitHub

