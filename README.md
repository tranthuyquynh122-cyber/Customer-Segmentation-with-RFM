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

### Objective:

📘 **What is this project about? What Business Question will it solve?**

This project applies **RFM (Recency – Frequency – Monetary) analysis** to segment customers from a **global online retail company**, with the goal of supporting data-driven marketing and customer retention strategies.

The analysis focuses on understanding purchasing behavior patterns and identifying high-value, loyal, and at-risk customer groups.

- Analyze historical transaction data to understand customer purchasing behavior  
- Segment customers using RFM methodology  
- Compare behavioral differences between **UK vs Non-UK markets**  
- Support personalized marketing and retention strategies  
- Reduce bias caused by the dominance of UK customers in the dataset
🎯 **Main business questions:**
- Which customer segments contribute the most to revenue?
- Which customers are at risk of churn and need re-engagement?
- How do customer behaviors differ between UK and Non-UK markets?
- How can marketing campaigns be tailored by customer segment?

👤 **Who is this project for?**

This project is useful for:

✔️ Marketing & CRM teams  
✔️ Business stakeholders & decision-makers  
✔️ E-commerce / retail companies working with customer transaction data 

<a id="dataset-description--data-structure"></a>
## 📂 Dataset Description & Data Structure

### 📌 Data Source
- Source: ecommerce_retail.xlsx
- Time range: **01/12/2010 – 09/12/2011**
- Format: `.xlsx`
- Size: ~540,000 transaction records  

### 📊 Data Structure

#### Table Used: Transaction Table

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| InvoiceNo | String | Invoice identifier (starting with "C" indicates cancellation) |
| StockCode | String | Product identifier |
| Description | String | Product description |
| Quantity | Integer | Quantity purchased |
| InvoiceDate | Datetime | Transaction timestamp |
| UnitPrice | Float | Price per unit |
| CustomerID | Numeric | Unique customer identifier |
| Country | String | Customer country |

## 🛠 Main Process

### 1️⃣ Data Cleaning & Preprocessing

Before performing analysis, the dataset was cleaned to ensure accuracy and reliability:

- Removed transactions with missing `CustomerID`
- Removed cancelled invoices (`InvoiceNo` starting with "C")
- Removed invalid transactions (`Quantity ≤ 0`, `UnitPrice ≤ 0`)
- Converted `InvoiceDate` into datetime format
- Created transaction value:
Amount = Quantity × UnitPrice

These transformations ensure that monetary values and behavioral metrics are correctly calculated.

---

### 2️⃣ RFM Metric Construction

RFM metrics were calculated at **customer level**, using a snapshot date of **31-12-2011**:

- **Recency (R):** Number of days since the most recent purchase  
- **Frequency (F):** Number of unique invoices  
- **Monetary (M):** Total spending amount  

Each metric was converted into a **quintile score (1–5)** to normalize customer behavior across the population.

---

### 3️⃣ Customer Segmentation Logic

Based on RFM score patterns, customers were grouped into the following segments:

- **Champions** – recent, frequent, and high-value customers  
- **Loyal** – frequent and consistently engaged customers  
- **Potential Loyalists** – customers with growth potential  
- **At Risk** – customers showing declining engagement  
- **Lost** – inactive or churned customers  

This segmentation enables targeted and differentiated marketing strategies.


## 📊 Visualization & Analysis

### 1️⃣ Customer Segment Distribution (UK vs Non-UK)

![Customer Segment Distribution](charts/segment_distribution_uk_nonuk.png)

**Insights:**
- UK customers dominate all segments, confirming strong market imbalance.
- Segment proportions are similar, but absolute volumes differ significantly.
- This validates the need to analyze UK and Non-UK customers separately.

---

### 2️⃣ Average Order Value by Segment

![AOV by Segment](charts/aov_by_segment_uk_nonuk.png)

**Insights:**
- Champions and Loyal segments show higher AOV.
- Several extreme outliers indicate bulk or wholesale-like purchases.
- Non-UK customers tend to have lower and more stable AOV distributions.

➡️ High AOV does not always imply higher engagement — aggressive upselling may backfire.

---

### 3️⃣ RFM Heatmap – UK Market

![RFM Heatmap UK](charts/rfm_score_heatmap_uk.png)

**Observations:**
- UK market shows higher Frequency across most segments.
- Revenue is driven by transaction volume rather than high AOV.
- Loyal and Champion segments form the backbone of total revenue.
- Lost customers still represent a large group, indicating opportunity for reactivation campaigns.

---

### 4️⃣ RFM Heatmap – Non-UK Market
![RFM Heatmap Non-UK](charts/rfm_score_heatmap_non-uk.png)

**Key insights:**
- Champions have the highest Monetary scores but relatively smaller population.
- At-Risk customers still show strong monetary value but long inactivity → high churn risk.
- Potential Loyalists demonstrate strong growth potential and should be nurtured.
- Behavioral patterns differ significantly from UK, reinforcing the need for localized strategies.

---
final-conclusion--recommendations
<a id="final-conclusion--recommendations"></a>
## 🎯 Final Conclusion & Recommendations

Based on the analysis above, the following key insights and actions are recommended:

### ✅ Key Takeaways

✔️ UK and Non-UK markets exhibit fundamentally different purchasing behaviors  
✔️ UK revenue is driven by frequency, while Non-UK revenue is driven by higher AOV  
✔️ At-Risk and Potential Loyalist segments represent high-opportunity targets  
✔️ Applying one global campaign strategy would introduce strong bias  

---

### ✅ Recommendations

🔹 **For Champions**
- Launch VIP & loyalty reward programs  
- Early access to promotions  
- Exclusive bundles  

🔹 **For Loyal Customers**
- Cross-sell & upsell recommendations  
- Personalized product suggestions  
- Subscription or repeat-purchase incentives  

🔹 **For Potential Loyalists**
- Targeted onboarding & nurturing campaigns  
- Limited-time offers to encourage repeat purchases  
- Personalized email marketing  

🔹 **For At-Risk Customers**
- Win-back campaigns with discounts or reminders  
- Personalized reactivation emails  
- Time-sensitive promotions  

🔹 **For Lost Customers**
- Low-cost reactivation campaigns  
- Exclude from aggressive promotions to reduce marketing cost  









