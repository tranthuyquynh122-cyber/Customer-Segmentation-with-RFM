# 📊 Customer Segmentation using RFM for a Global Retail Business | Python

## 📌 Project Overview
**Business question:** How can we segment customers based on purchasing behavior to support targeted marketing strategies?  
**Domain:** E-commerce / Retail Analytics  
Author: Tran Thuy Quynh  
Date: 2025  
Tools Used: Python (Pandas, NumPy, Matplotlib, Seaborn)
## 📑 Table of Contents
1. 📌 [Background & Overview](#background--overview)
2. 📂 [Dataset Description & Data Structure](#dataset-description--data-structure)
3. 🔎 [Final Conclusion & Recommendations](#final-conclusion--recommendations)
---
<a id="background--overview"></a>
## 📌 Background & Overview

## 📌 1. Background & Overview

### Objective

### 📖 What is this project about?

- The Marketing team plans to launch personalized campaigns to improve **customer retention and acquisition**, especially during the holiday season. However, because the dataset is large, manually segmenting customers is no longer practical.

- To address this challenge, the **RFM (Recency, Frequency, Monetary) model** is applied using **Python (Google Colab)** to classify customers into different segments based on their purchasing behavior.

- This project covers several key steps including **data preparation, RFM score calculation, customer segmentation, visualization**, and generating **actionable recommendations** to help the Marketing and Sales teams optimize their strategies.

---

### 👥 Who is this project for?

✔ **Marketing & Sales Department**  
✔ **Decision-makers and business stakeholders**

These stakeholders can use the insights from this analysis to better understand customer behavior and design more effective marketing strategies.

---

### ❓ Business Questions

✔ How can customers be segmented effectively using the **RFM model**?  

✔ Which customer groups should be prioritized for **retention and promotional campaigns**?  

✔ What actionable insights can help improve **marketing strategies and customer engagement**?  

✔ What strategies should be applied to **different customer segments to maximize business value**?
 ✔ What strategies should be applied to different customer segments to maximize business value?

---

## 🧠 RFM Analysis Overview

### 🔎 Why use RFM?

**RFM (Recency, Frequency, Monetary)** is a widely used technique for customer segmentation based on purchasing behavior.

In RFM analysis, each customer receives a score based on three dimensions of their transaction history. These scores help businesses categorize customers into different segments, making it easier to identify target audiences for marketing and sales strategies.

The three key metrics include:

---

### 📅 Recency

Measures the amount of time since a customer's most recent purchase.

- Customers who purchased recently are more likely to respond to marketing campaigns.
- Customers who haven't purchased for a long time may require re-engagement strategies.

---

### 🔁 Frequency

Measures how often a customer makes purchases within a given period.

- High frequency indicates strong engagement and customer loyalty.
- Low frequency suggests occasional or inactive customers.

---

### 💰 Monetary

Measures the total amount of money spent by a customer.

- Customers with higher spending contribute more revenue to the business.
- Customers with lower spending may have smaller economic impact.

---

By combining these three metrics, businesses can group customers based on their overall value and engagement level.

Using the RFM approach enables organizations to:

- Identify their **most valuable customers**
- Detect **customers at risk of churn**
- Design **targeted marketing campaigns**
- Improve **customer retention and engagement strategies**

### 🧩 Important Data Structure Note

This dataset is stored at the **transaction-line level**, not at the customer level.

That means:

- one customer can have many invoices
- one invoice can contain multiple products
- one customer may appear many times in the raw data

So before applying the RFM model, the data must be transformed from:

**transaction level → customer level**

This is an important step because RFM segmentation is performed **per customer**, not per transaction row.

### Table Used

This project uses **one main transaction table**.

---

### Table Schema

| Column Name | Data Type | Description |
|---|---|---|
| InvoiceNo | object | Unique invoice number. If it starts with `"C"`, it indicates a cancelled transaction. |
| StockCode | object | Product code |
| Description | object | Product name |
| Quantity | int | Number of items purchased |
| InvoiceDate | datetime | Date and time of the transaction |
| UnitPrice | float | Price per item |
| CustomerID | int | Unique identifier of each customer |
| Country | object | Customer country |

Additional derived field used in the project:

| Derived Column | Meaning |
|---|---|
| SalesAmount | `Quantity × UnitPrice` |

---

### 📌 Screenshot to insert here

**Insert a screenshot of the raw dataset head / schema here.**

**Use these code cells from your notebook:**
- `df_retail.head()`
- `df_retail.info()`
- `df_retail.describe()`

---

## 3. Data Cleaning & Preprocessing

Before applying the RFM model, the dataset must be cleaned to ensure that the analysis reflects **actual purchasing behavior**.

### Step 1: Standardize column names
Column names were stripped to remove extra spaces.
```python
df_retail.columns = df_retail.columns.str.strip()
```

### Step 2: Remove missing customer IDs
Rows with missing `CustomerID` were removed because RFM is a **customer-level model**. Without customer IDs, transactions cannot be assigned to any customer.
```python
df_retail = df_retail.dropna(subset=["CustomerID"])
df_retail["CustomerID"] = df_retail["CustomerID"].astype(int)
```
### Step 3: Remove cancelled transactions
Orders where `InvoiceNo` starts with `"C"` were excluded because they represent cancelled purchases rather than actual sales.
```python
df_retail = df_retail[~df_retail["InvoiceNo"].astype(str).str.startswith("C")]
```
### Step 4: Remove invalid values
Rows with:

- `Quantity <= 0`
- `UnitPrice <= 0`

were removed because they do not reflect valid purchase transactions.
```python
df_retail = df_retail[(df_retail["Quantity"] > 0) & df_retail["UnitPrice"] > 0]
```
### Step 5: Convert `InvoiceDate` to datetime
This allows time-based analysis such as:

- monthly revenue trend
- purchase recency
- time feature extraction
```python
df_retail["InvoiceDate"] = pd.to_datetime(df_retail["InvoiceDate"],errors="coerce")
df_retail = df_retail.dropna(subset=["InvoiceDate"])
```
### Step 6: Create `SalesAmount`
A new variable was created:
SalesAmount = Quantity * UnitPrice
```python
df_retail["SalesAmount"] = (df_retail["Quantity"] * df_retail["UnitPrice"])
```





















