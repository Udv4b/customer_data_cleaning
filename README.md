# Customer Data Cleaning Project

## 📌 Project Overview

This project focuses on cleaning and preprocessing a messy customer dataset using **Python and Pandas**.

The original dataset contains common real-world data quality problems such as:

* Missing values
* Inconsistent Customer IDs
* Invalid age values
* Inconsistent categorical values
* Invalid dates
* Invalid purchase amounts
* Invalid feedback scores
* Incorrect email addresses
* Invalid phone numbers
* Duplicate records

The goal of this project is to identify these issues, clean the dataset, standardize the data, and create a reliable dataset suitable for further analysis.

---

## 📂 Dataset

**Original file:** `messy_customer_data.csv`

**Cleaned file:** `cleaned_customer_data.csv`

The dataset contains customer-related information including:

* Customer ID
* Gender
* Age
* Signup Date
* Last Purchase Date
* Purchase Amount
* Feedback Score
* Email
* Phone Number
* Other customer attributes

---

## 🛠️ Technologies Used

* Python
* Pandas
* NumPy
* Jupyter Notebook / Google Colab

---

## 🔍 Data Quality Issues Identified

### 1. Missing Customer IDs

Several records had missing `CustomerID` values.

The IDs were standardized to a consistent format:

```text
C1
C2
C3
C4
...
```

Missing IDs were filled using the corresponding row/customer number.

---

### 2. Inconsistent Customer ID Format

Customer IDs were stored in different formats, such as:

```text
C2
3
C4
5
```

They were standardized to:

```text
C2
C3
C4
C5
```

---

### 3. Inconsistent Gender Values

The `Gender` column contained variations such as:

```text
M
Male
male
F
Female
female
```

These values were standardized to:

```text
Male
Female
```

---

### 4. Invalid Age Values

Some age values were outside a reasonable range, including negative ages and extremely high values.

Invalid values were converted to missing values (`NaN`).

A valid age range of **0–100** was used for validation.

---

### 5. Invalid Dates

Date columns contained invalid entries such as:

```text
not_a_date
missing
```

These values were converted to missing values, while valid dates were converted to Pandas datetime format.

---

### 6. Invalid Purchase Amounts

The `purchase_amount` column contained negative values such as:

```text
-999
```

These were treated as invalid values and converted to missing values.

---

### 7. Invalid Feedback Scores

The `feedback_score` column is expected to contain ratings on a **0–10 scale**.

The dataset contained:

```text
-1
```

These values were treated as invalid/special feedback values and converted to missing values.

The value `-2` was also considered invalid, although no `-2` values were found in the dataset.

Valid feedback scores remain within:

```text
0–10
```

---

### 8. Invalid Email Addresses

Email addresses were checked using a basic email-format validation rule.

Invalid email values were converted to missing values.

Example of an expected format:

```text
customer@example.com
```

---

### 9. Invalid Phone Numbers

Phone numbers were validated to contain exactly **10 digits**.

Invalid values such as:

```text
0
abc123
```

were converted to missing values.

---

### 10. Duplicate Records

The dataset was checked for exact duplicate rows.

Any exact duplicate records were removed during the cleaning process.

---

## 🧹 Data Cleaning Process

The following steps were performed:

```text
Raw Dataset
     ↓
Check Missing Values
     ↓
Check Duplicate Records
     ↓
Clean Customer IDs
     ↓
Standardize Gender
     ↓
Validate Age
     ↓
Convert Dates
     ↓
Clean Purchase Amount
     ↓
Validate Feedback Scores
     ↓
Validate Email Addresses
     ↓
Validate Phone Numbers
     ↓
Remove Duplicate Rows
     ↓
Clean Dataset
```

---

## 📊 CSAT Analysis

After cleaning the feedback scores, Customer Satisfaction (CSAT) was calculated.

The dataset uses a **0–10 feedback scale**.

For this project, ratings of **7 or higher** are considered satisfied.

### Average CSAT

The average feedback score is calculated using:

```python
avg_csat = df["feedback_score"].mean()
```

### CSAT Percentage

The percentage of satisfied customers is calculated using:

```python
valid_scores = df["feedback_score"].dropna()

satisfied_count = (valid_scores >= 7).sum()

csat_percentage = (
    satisfied_count / valid_scores.count()
) * 100
```

This gives the percentage of customers whose feedback score is **7–10**.

---

## 💻 Cleaning Code

The main cleaning operations were performed using Python and Pandas.

Example:

```python
import pandas as pd
import numpy as np

df = pd.read_csv("messy_customer_data.csv")

# Clean Customer ID
df["CustomerID"] = (
    df["CustomerID"]
    .astype("string")
    .str.strip()
    .str.replace("C", "", regex=False)
)

df["CustomerID"] = pd.to_numeric(
    df["CustomerID"],
    errors="coerce"
)

df["CustomerID"] = df["CustomerID"].fillna(
    pd.Series(range(1, len(df) + 1), index=df.index)
)

df["CustomerID"] = (
    "C" + df["CustomerID"].astype(int).astype(str)
)

# Clean Gender
df["Gender"] = (
    df["Gender"]
    .astype("string")
    .str.strip()
    .str.lower()
)

df["Gender"] = df["Gender"].replace({
    "m": "Male",
    "male": "Male",
    "f": "Female",
    "female": "Female"
})

# Clean Age
df.loc[
    (df["Age"] < 0) | (df["Age"] > 100),
    "Age"
] = np.nan

# Convert dates
df["Signup_Date"] = pd.to_datetime(
    df["Signup_Date"],
    errors="coerce"
)

df["Last_purchase_date"] = pd.to_datetime(
    df["Last_purchase_date"],
    errors="coerce"
)

# Clean purchase amount
df.loc[
    df["purchase_amount"] < 0,
    "purchase_amount"
] = np.nan

# Clean feedback scores
df.loc[
    ~df["feedback_score"].between(0, 10),
    "feedback_score"
] = np.nan

# Remove exact duplicates
df = df.drop_duplicates()

# Save cleaned dataset
df.to_csv(
    "cleaned_customer_data.csv",
    index=False
)
```

---

## 📁 Project Structure

```text
customer-data-cleaning/
│
├── messy_customer_data.csv
├── cleaned_customer_data.csv
├── data_cleaning.py
└── README.md
```

---

## ✅ Result

The cleaned dataset is more consistent and suitable for:

* Customer analysis
* CSAT analysis
* Data visualization
* Business intelligence dashboards
* Further statistical analysis
* Machine learning preprocessing

The project demonstrates practical data-cleaning techniques commonly used by **Data Analysts** when working with real-world datasets.

---

## 🎯 Key Skills Demonstrated

* Data Cleaning
* Data Preprocessing
* Exploratory Data Analysis
* Missing Value Handling
* Duplicate Detection
* Data Validation
* Data Type Conversion
* Categorical Data Standardization
* Data Quality Checks
* Python
* Pandas
* NumPy
* Customer Satisfaction (CSAT) Analysis

---

## 👤 Author

**Udvab Biswas**

Data Analyst | Python | SQL | Power BI | Data Visualization

---

## 📜 License

This project is created for educational and portfolio purposes.
