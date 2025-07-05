# 📊 Credit Card Transaction Report — Power BI + PostgreSQL

This project showcases a **Power BI dashboard** built to analyze credit card transaction data. The data is cleaned and transformed using **PostgreSQL** and visualized in Power BI. The dashboard includes meaningful customer segmentation, revenue trends, and key performance indicators, powered by advanced **DAX queries**.

---

## 🧠 Purpose of the Report

The goal of this report is to gain deep insights into:

* Customer demographics (age, income)
* Revenue trends (weekly performance)
* Transaction behaviors (volume, frequency, spending habits)

---

## 🚀 Tech Stack

| Tool           | Description                                    |
| -------------- | ---------------------------------------------- |
| **Power BI**   | Dashboard creation and visualization           |
| **PostgreSQL** | Data cleaning and transformation               |
| **CSV Files**  | Raw data source (customers & transactions)     |
| **DAX**        | Custom logic and calculated fields in Power BI |

---

## 🗃️ Data Pipeline

1. ✅ Cleaned and structured **CSV** files
2. ✅ Imported data into **PostgreSQL**
3. ✅ Connected PostgreSQL to **Power BI**
4. ✅ Applied **DAX logic** for metrics & insights
5. ✅ Created visuals and slicers for interactivity

---

## 📷 Dashboard Preview

> Example:
> ![image](https://github.com/user-attachments/assets/f3a42c8f-855a-4b48-afee-5c81977189ba)


---

## 🧮 DAX Queries Explained

Here’s a breakdown of the custom DAX measures and columns used in the report:

---

### 📌 1. Age Group Bucketing

```dax
AgeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[customer_age] < 30, "20-30",
    'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
    'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
    'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
    'public cust_detail'[customer_age] >= 60, "60+",
    "unknown"
)
```

**Explanation:**
This DAX formula categorizes customers into age groups based on their age. It helps segment the customer base visually (e.g., bar chart by age group).

---

### 📌 2. Income Group Classification

```dax
IncomeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[income] < 35000, "Low",
    'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] <70000, "Med",
    'public cust_detail'[income] >= 70000, "High",
    "unknown"
)
```

**Explanation:**
This measure classifies users into **Low**, **Medium**, or **High** income groups. This is useful for analyzing spending behavior by income level.

---

### 📌 3. Week Number Calculation

```dax
week_num2 = WEEKNUM('public cc_detail'[week_start_date])
```

**Explanation:**
This formula extracts the week number from a transaction date, allowing us to analyze weekly trends and compare revenues week over week.

---

### 📌 4. Revenue Calculation

```dax
Revenue = 'public cc_detail'[annual_fees] + 
          'public cc_detail'[total_trans_amt] + 
          'public cc_detail'[interest_earned]
```

**Explanation:**
This measure calculates total revenue per transaction by summing:

* Annual card fees
* Total transaction amount
* Interest earned

This is the **core KPI** used in various visuals and comparisons.

---

### 📌 5. Current Week Revenue

```dax
Current_week_Reveneue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])
    )
)
```

**Explanation:**
This DAX formula calculates revenue for the **most recent week** in the dataset. It’s dynamic and updates automatically as new data comes in.

---

### 📌 6. Previous Week Revenue

```dax
Previous_week_Reveneue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]) - 1
    )
)
```

**Explanation:**
Similar to the one above, but calculates revenue for the **week before the current week**. Useful for trend analysis and growth comparisons.

---


---

## 🛠 Sample PostgreSQL Steps

1. **Create Tables:**

```sql
CREATE TABLE cust_detail (...);
CREATE TABLE cc_detail (...);
```

2. **Import CSV to SQL:**

```sql
COPY cust_detail FROM '/path/to/customer_details.csv' DELIMITER ',' CSV HEADER;
COPY cc_detail FROM '/path/to/credit_card_transactions.csv' DELIMITER ',' CSV HEADER;
```

3. **Clean Data with SQL Queries**
   Remove nulls, duplicates, standardize formats, etc.

---

## ⭐ Feedback

If you found this project useful, please ⭐️ star the repository and share your feedback!

---

