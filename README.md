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

























