# 📊 Customer Segmentation using RFM for a Global Retail Business | Python

## 📌 Project Overview
**Business question:** How can we segment customers based on purchasing behavior to support targeted marketing strategies?  
**Domain:** E-commerce / Retail Analytics  
Author: Tran Thuy Quynh  
Date: 2025  
Tools Used: Python (Pandas, NumPy, Matplotlib, Seaborn)
## 📑 Table of Contents

📌 **1.** [Background & Overview](#1-background--overview)

📂 **2.** [Dataset Description & Data Structure](#2-dataset-description--data-structure)

🧹 **3.** [Data Cleaning & Preprocessing](#3-data-cleaning--preprocessing)

🔎 **4.** [Exploratory Data Analysis (EDA)](#4-exploratory-data-analysis-eda)

📊 **5.** [Apply RFM Model](#5-apply-rfm-model)

📈 **6.** [Visualization & Analysis](#6-visualization--analysis)

💡 **7.** [Insight & Recommendation](#7-insight--recommendation)
---

<a id="1-background--overvie"></a>
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


## 📂 2. Dataset Description & Data Structure

### 📌 Data Source

The dataset used in this project is an **E-commerce retail transaction dataset** provided for customer behavior analysis.

- **Source:** E-commerce retail transaction dataset  
- **Size:** **541,910 rows × 8 columns** (Sheet 1: E-commerce retail transactions)  
- **Additional Data:** Customer segmentation details in **Sheet 2**  
- **Format:** `.xlsx` file (Excel workbook with **two sheets**)  

## 📁 Data Structure & Relationships

### 1️⃣ Tables Used

The dataset consists of **two tables (sheets):**

- **Sheet 1: E-commerce Retail** – Contains **transaction-level data**, including order details, customer IDs, product information, and purchase records.

- **Sheet 2: Segmentation** – Stores **customer segments along with their RFM scores**, which are used for customer behavior analysis.

---

### 2️⃣ Table Schema & Data Snapshot

#### 📌 Sheet 1: E-commerce Retail

### 📄 Table Schema: E-commerce Retail

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| InvoiceNo | object | Unique invoice number for each transaction (6-digit). If it starts with **"C"**, it indicates a cancellation. |
| StockCode | object | Unique product (item) code (5-digit). |
| Description | object | Product (item) name. |
| Quantity | int64 | The number of units purchased per transaction. |
| InvoiceDate | datetime64[ns] | Date and time when the transaction occurred. |
| UnitPrice | float64 | Price per unit of the product in sterling. |
| CustomerID | float64 | Unique 5-digit identifier for each customer. |
| Country | object | Name of the country where the customer resides. |

---

#### 📌 Sheet 2: Segmentation

### 📊 Customer Segmentation & RFM Scores

| Segment | RFM Score |
|-------|-----------|
| Champions | 555, 554, 544, 545, 454, 455, 445 |
| Loyal | 543, 444, 435, 355, 354, 345, 344, 335 |
| Potential Loyalist | 553, 551, 552, 541, 542, 533, 532, 531, 452, 451, 442, 441, 431, 453, 433, 432, 423, 353, 352, 351, 342, 341, 333, 323 |
| New Customers | 512, 511, 422, 421, 412, 411, 311 |
| Promising | 525, 524, 523, 522, 521, 515, 514, 513, 425, 424, 413, 414, 415, 315, 314, 313 |
| Need Attention | 535, 534, 443, 434, 343, 334, 325, 324 |
| About To Sleep | 331, 321, 312, 221, 213, 231, 241, 251 |
| At Risk | 255, 254, 245, 244, 253, 252, 243, 242, 235, 234, 225, 224, 153, 152, 145, 143, 142, 135, 134, 133, 125, 124 |
| Cannot Lose Them | 155, 154, 144, 214, 215, 115, 114, 113 |
| Hibernating Customers | 332, 322, 233, 232, 223, 222, 132, 123, 122, 212, 211 |
| Lost Customers | 111, 112, 121, 131, 141, 151 |

---

### Relationship Between Tables

The **Segmentation table** is derived from the **E-commerce Retail transaction table** after applying the **RFM model**.

The relationship is established through:

- **CustomerID** → used to aggregate transactions at the customer level
- RFM scores are calculated from transactional metrics:
  - **Recency** → days since last purchase
  - **Frequency** → number of purchases
  - **Monetary** → total spending

This structure allows transaction data to be transformed into **customer-level behavioral insights for segmentation and marketing analysis.**

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
### ✅ Output of the cleaning step
<img width="1039" height="359" alt="image" src="https://github.com/user-attachments/assets/50b52b5a-dec7-460c-b05e-658992627f40" />

After cleaning, the dataset contains only:

- valid purchases
- customers with IDs
- positive quantity and unit price
- usable transaction dates

---

# 🔎 4. Exploratory Data Analysis (EDA)

Before applying the RFM model, exploratory analysis was performed to understand customer purchasing behavior.

---

## 🌍 Revenue by Country

Revenue was aggregated by country to identify which markets contribute the most to total sales.

<img width="1126" height="457" alt="image" src="https://github.com/user-attachments/assets/d06361da-f899-4a53-a835-dd20209af5da" />

### Key Observation

The chart shows an extremely uneven revenue distribution across countries.

- The **United Kingdom** generated **7,308,392** in revenue
- The second-largest market, **Netherlands**, generated only **285,446**
- Other countries such as **EIRE, Germany, and France** also contributed far less compared with the UK

This means the UK overwhelmingly dominates the dataset in terms of sales value.
---

### 📊 Revenue Share

The revenue share table confirms this concentration:
<img width="346" height="320" alt="image" src="https://github.com/user-attachments/assets/cc034d3a-6e0b-46ed-852d-372415938d43" />


The **UK alone contributes approximately 82% of total revenue**, which is significantly higher than all other countries combined individually.

This is a very strong indication that the UK is the core market in the dataset.  
From a business perspective, any customer analysis that aims to support revenue-focused marketing decisions should pay particular attention to this market.

## 📦 Transaction by Country

Transaction counts by country were analyzed using the number of **unique invoices**, which better reflects actual order activity.

<img width="921" height="542" alt="image" src="https://github.com/user-attachments/assets/2bf6cef1-cf60-45ea-84e4-65b9267f589a" />
\\
### Key Observation

The UK also dominates in terms of transaction volume.

- **United Kingdom:** 16,646 transactions
- **Germany:** 457 transactions
- **France:** 389 transactions
- **EIRE:** 260 transactions

All remaining countries contribute only a very small number of orders compared to the UK.

### Insight

This result reinforces the finding from the revenue chart.

The UK is not only the largest market by **revenue**, but also the most active market by **transaction volume**.  
This means the UK drives the majority of purchasing activity in the dataset and contains the richest customer behavior patterns for segmentation analysis.

## 🎯 Market Selection Decision

Based on the analysis above, the **United Kingdom** was selected as the focus market for further analysis.

### Reasons

1. The UK generates **over 80% of total revenue**.
2. The UK has the **highest transaction volume** in the dataset.
3. Analyzing multiple countries together may introduce bias due to differences in purchasing behavior.

Therefore, focusing on the UK allows the segmentation model to analyze customers within a **consistent market environment**.

# 💡 UK Market Exploratory Data Analysis

After identifying the **United Kingdom as the dominant market** in the dataset, the analysis now focuses specifically on the UK to better understand customer purchasing patterns before applying the RFM segmentation model.
<img width="1005" height="271" alt="image" src="https://github.com/user-attachments/assets/83d8cd9c-f6fe-4cd2-a2e7-eb6cd28e2832" />


Filtering the dataset to the UK market results in:

- **Total records:** 354,321 transactions
- **Unique customers:** 3,920
- **Unique invoices:** 16,646



This market contains the majority of the business activity and therefore provides the most meaningful customer behavior data for segmentation analysis.

---

# 📅 Monthly Revenue Trend

To understand how sales evolve over time, monthly revenue in the UK market was analyzed.

<img width="1096" height="460" alt="image" src="https://github.com/user-attachments/assets/1eb581a7-5570-4198-9a9b-4d1455e6df87" />


### Key Observations

The revenue trend shows noticeable fluctuations throughout the year.

Revenue remains relatively stable during the first half of the year, ranging roughly between **£400K – £550K per month**.

However, starting from **September**, revenue begins to increase significantly.

The peak occurs in **November**, where revenue reaches nearly **£1M**, indicating a major surge in purchasing activity.

### Insight

This pattern suggests strong **seasonal demand**, likely driven by holiday shopping periods such as **Black Friday and Christmas preparation**.

Understanding this seasonal spike is important for marketing teams because it highlights when customer engagement and purchasing activity are highest.

This also indicates that promotional campaigns and marketing investments may be most effective during the final quarter of the year.

---

# 🛒 Basket Size Distribution

The distribution of purchase quantities was analyzed to understand the typical basket size in the UK market.

<img width="908" height="456" alt="image" src="https://github.com/user-attachments/assets/6dde0655-2c20-4d03-86f1-2dc79a0fe7f1" />


### Key Observations

The distribution is **highly right-skewed**.

Most transactions involve **small quantities**, indicating that customers typically purchase only a few items per order.

However, a small number of transactions contain extremely large quantities, creating a long tail in the distribution.

### Insight

This suggests that while most customers make **small to medium-sized purchases**, there are occasional **bulk purchases**, likely from business buyers or wholesale customers.

These outliers are important to consider because they can significantly influence revenue metrics.

Understanding basket size distribution helps interpret the **Monetary component of the RFM model**, as some customers may generate high revenue through large single purchases.

---

# ⭐ Top Selling Products

The top-selling products in the UK were identified based on the total quantity sold.

<img width="986" height="400" alt="image" src="https://github.com/user-attachments/assets/732f2db2-af3c-48ad-9357-02554de74593" />


### Key Observations

Several products stand out with significantly higher sales volumes.

The most popular product is:

**PAPER CRAFT , LITTLE BIRDIE** with over **80,000 units sold**.

Other high-demand products include:

- MEDIUM CERAMIC TOP STORAGE JAR
- WORLD WAR 2 GLIDERS ASSTD DESIGNS
- JUMBO BAG RED RETROSPOT
- WHITE HANGING HEART T-LIGHT HOLDER

Many of these products appear to be **decorative household items or gift products**, suggesting a strong demand for small decorative goods.

### Insight

The concentration of sales among a small number of products indicates that certain items drive a significant portion of purchase activity.

These products may represent **core items in the product catalog**, making them strong candidates for promotional campaigns, bundling strategies, or recommendation systems.

Understanding product popularity also provides context for customer purchasing behavior when analyzing RFM segments.

---

# 🔑UK Market Insights

The UK market analysis reveals several key patterns:

1. **Strong seasonal revenue growth**, especially in the final quarter of the year.
2. **Small basket sizes dominate**, although occasional bulk purchases exist.
3. **A small number of products drive a large share of sales volume.**

These insights provide important context before applying the **RFM model**, helping interpret customer purchasing behavior more accurately.


# 🧠 5. Apply RFM Model

After exploring customer purchasing behavior in the UK market, the **RFM model (Recency, Frequency, Monetary)** is applied to segment customers based on their purchasing patterns.

RFM analysis helps businesses understand customer value and engagement levels, enabling more targeted marketing strategies.

---

# Step 1 — Define Snapshot Date

To calculate **Recency**, a reference point in time is required.  
This project uses **December 31, 2011** as the snapshot date.

```python
from datetime import datetime

SNAPSHOT_DATE = datetime(2011, 12, 31)
```

This date represents the end of the dataset period and is used to calculate how recently customers made their last purchase.

---

# Step 2 — Build RFM Table

The next step is to aggregate transaction data at the **customer level**.

For each customer we calculate:

- **Last Purchase Date**
- **Frequency (number of transactions)**
- **Monetary value (total spending)**

```python
def build_rfm_table(df_market, snapshot_date):

    rfm_market_table = (
        df_market.groupby("CustomerID", as_index=False)
        .agg(
            LastPurchaseDate=("InvoiceDate", "max"),
            Frequency=("InvoiceNo", "nunique"),
            Monetary=("SalesAmount", "sum")
        )
    )

    rfm_market_table["Recency"] = (
        snapshot_date - rfm_market_table["LastPurchaseDate"]
    ).dt.days

    return rfm_market_table


rfm_uk_table = build_rfm_table(df_uk_retail, SNAPSHOT_DATE)

rfm_uk_table.head()
```

<img width="451" height="195" alt="image" src="https://github.com/user-attachments/assets/7b4ab53a-85a1-4d8c-9d67-7ec24e30a6a0" />


This table summarizes each customer's purchasing behavior.

---

# Step 3 — Create RFM Scores

To standardize comparisons between customers, the three RFM metrics are converted into **scores from 1 to 5** using quantile-based binning.

```python
# Recency: smaller is better
rfm_uk_table["R_score"] = pd.qcut(
    rfm_uk_table["Recency"],
    q=5,
    labels=[5,4,3,2,1]
).astype(int)

# Frequency: larger is better
rfm_uk_table["F_score"] = pd.qcut(
    rfm_uk_table["Frequency"].rank(method="first"),
    q=5,
    labels=[1,2,3,4,5]
).astype(int)

# Monetary: larger is better
rfm_uk_table["M_score"] = pd.qcut(
    rfm_uk_table["Monetary"],
    q=5,
    labels=[1,2,3,4,5]
).astype(int)
```

Each customer now receives a **score between 1 and 5** for each dimension.

---

# Step 4 — Create RFM Code

The RFM scores are combined to form an **RFM code**.

```python
rfm_uk_table["RFM_Code"] = (
    rfm_uk_table["R_score"].astype(str)
    + rfm_uk_table["F_score"].astype(str)
    + rfm_uk_table["M_score"].astype(str)
)
```
<img width="716" height="358" alt="image" src="https://github.com/user-attachments/assets/707290a8-6502-406a-b915-43715a99ff99" />


The RFM code represents the overall customer value profile.

---

# Step 5 — Customer Segmentation

Customers are then assigned to behavioral segments based on **Recency and Frequency scores**.

```python
def assign_rfm_segment(row):

    r = row["R_score"]
    f = row["F_score"]

    if (r >= 4) and (f >= 4):
        return "Champions"

    elif (r >= 3) and (f >= 4):
        return "Loyal Customers"

    elif (r >= 4) and (f >= 2):
        return "Potential Loyalists"

    elif (r == 5) and (f == 1):
        return "New Customers"

    elif (r == 4) and (f == 1):
        return "Promising"

    elif (r == 3) and (f >= 2):
        return "Need Attention"

    elif (r == 2) and (f <= 2):
        return "About To Sleep"

    elif (r <= 2) and (f >= 3):
        return "At Risk"

    elif (r == 1) and (f >= 4):
        return "Can't Lose Them"

    elif (r <= 2) and (f <= 2):
        return "Hibernating"

    else:
        return "Lost"


rfm_uk_table["Segment"] = rfm_uk_table.apply(assign_rfm_segment, axis=1)
```

---

# Step 6 — Organize Segment Order

To maintain logical ordering in visualizations, segments are converted into a categorical variable.

```python
segment_order = [
    "Champions",
    "Loyal Customers",
    "Potential Loyalists",
    "New Customers",
    "Promising",
    "Need Attention",
    "About To Sleep",
    "At Risk",
    "Can't Lose Them",
    "Hibernating",
    "Lost"
]

rfm_uk_table["Segment"] = pd.Categorical(
    rfm_uk_table["Segment"],
    categories=segment_order,
    ordered=True
)
```

---

# Final RFM Table

```python
rfm_uk_table[
    [
        "CustomerID",
        "Recency",
        "Frequency",
        "Monetary",
        "R_score",
        "F_score",
        "M_score",
        "RFM_Code",
        "Segment"
    ]
].head()
```
<img width="653" height="189" alt="image" src="https://github.com/user-attachments/assets/46870c16-ca9d-483a-8950-f9cba73f9057" />


The final table provides a **complete view of customer purchasing behavior and segmentation results**, which will be used for further analysis and marketing insights.
# 📊 6. Visualization & Analysis

After applying the RFM model, the next step is to visualize customer segments and interpret their business meaning.

This section helps answer important questions such as:

- Which segments contain the largest number of customers?
- Which segments generate the most revenue?
- How do purchasing behaviors differ across segments?
- How can grouped segments reveal broader business patterns over time?

---

# 6.1 Customer Segment Distribution

This chart shows the number of customers in each RFM segment.
<img width="1334" height="459" alt="image" src="https://github.com/user-attachments/assets/4a56e43c-ff1b-45a7-af08-7654ce337e84" />


### Analysis

The customer base is not distributed evenly across segments.

- **Champions** represent the largest segment, with around **1,000 customers**
- **Hibernating** and **At Risk** are also large groups, each containing roughly **600 customers**
- **Potential Loyalists** form another meaningful segment, indicating a substantial number of customers with future growth potential
- Segments such as **New Customers**, **Promising**, and especially **Can't Lose Them** are much smaller

### Insight

This distribution suggests that the business has a **strong base of highly engaged customers**, which is a positive sign.

However, the presence of many **Hibernating** and **At Risk** customers also indicates that a large portion of the customer base is no longer fully engaged.

This means the business is in a mixed position:

- it already has a strong core of loyal and high-value customers
- but it also faces a meaningful churn risk from inactive or declining customers

---

# 6.2 Revenue Contribution by Segment

This chart shows how much total revenue is generated by each RFM segment.

<img width="1308" height="468" alt="image" src="https://github.com/user-attachments/assets/486631a5-56b3-4beb-93ef-23180550a577" />


### Analysis

Revenue contribution is even more concentrated than customer count.

- **Champions** generate by far the highest total revenue, contributing the majority of segment revenue
- **At Risk** and **Loyal Customers** are the next most important revenue contributors
- **Potential Loyalists** also contribute a meaningful amount of revenue
- Low-engagement segments such as **Promising**, **New Customers**, and **Can't Lose Them** contribute very little overall revenue

### Insight

This confirms that **Champions are the economic engine of the business**.

Even though some lower-engagement segments contain non-trivial customer counts, they do not contribute nearly as much revenue.

This implies that the business should prioritize:

1. **retaining Champions**
2. **protecting Loyal Customers**
3. **re-engaging At Risk customers**, since they still represent high monetary value

The strong revenue contribution from **At Risk** customers is especially important because it signals potential revenue loss if these customers are not recovered.

---

# 6.3 Average Order Value by Segment

This boxplot compares the distribution of **average order value** across customer segments.

<img width="1326" height="455" alt="image" src="https://github.com/user-attachments/assets/e5b12741-9e85-477a-88dc-73984718c406" />


### Analysis

Most segments have relatively low average order values clustered near the bottom of the chart, while several segments contain extreme outliers.

A few observations stand out:

- **Potential Loyalists** show some very large outliers, including one extremely high-value case
- **Hibernating** also includes a very large outlier
- **At Risk** contains several notable high-value orders
- Core segments such as **Champions** and **Loyal Customers** appear more consistently distributed, although they also include some larger-value purchases

### Insight

This chart suggests that segment value is not only driven by frequency, but also by **basket size variation**.

Some segments with relatively moderate overall engagement may still include **very high-value transactions**.

This is particularly important for:

- **At Risk** customers, because they may have historically placed valuable orders
- **Potential Loyalists**, because some of them are already showing strong purchase value and could become highly profitable if nurtured properly

In short, not all low-frequency or non-core segments are low-value.  
Some contain high-spending customers who deserve more strategic attention.

---

# 6.4 Average R/F/M Scores by Segment

This heatmap compares the average **Recency, Frequency, and Monetary scores** across segments.
<img width="795" height="557" alt="image" src="https://github.com/user-attachments/assets/20ddc29c-b17c-4f1b-9f23-e6778d9eb4e9" />


### Analysis

The heatmap clearly shows the behavioral differences between segments.

#### Champions
Champions score very highly across all three dimensions, confirming that they are the most engaged and valuable customers.

#### Loyal Customers
- lower recency than Champions, but still high
- strong frequency and monetary scores

This indicates consistent repeat purchasing behavior.

#### Potential Loyalists
- very strong recency
- moderate frequency and monetary scores

These customers are active recently, but have not yet reached the same purchasing depth as Loyal Customers or Champions.

#### New Customers / Promising
- very high recency
- very low frequency
- relatively low monetary scores

These customers are recent but still early in their relationship with the business.

#### At Risk
- low recency
- moderate-to-high frequency
- moderate monetary score

This means these customers used to be active and valuable but have recently stopped purchasing.

#### Hibernating / Lost
- low recency
- low frequency
- low monetary

These customers are clearly disengaged.

### Insight

The heatmap validates that the segmentation logic is working correctly.

Each segment exhibits a distinct behavioral pattern:

- **Champions** = best across all dimensions
- **Loyal Customers** = strong repeat buyers
- **Potential Loyalists** = highly recent, but still growing
- **At Risk** = previously engaged customers with declining activity
- **Hibernating / Lost** = weak across all metrics

This is important because it confirms that the segments are not just labels — they reflect real differences in customer behavior.

---

# 6.5 Group Segment Contribution

### Group Customer Segments

To simplify analysis and make the results easier to interpret from a business perspective, the detailed RFM segments were grouped into **four broader categories**:

| Group Segment | Included Segments |
|---|---|
| **Loyal & High Value** | Champions, Loyal Customers |
| **New & Potential** | Potential Loyalists, Promising, New Customers |
| **High Risk Customers** | At Risk, Can't Lose Them |
| **Inactive & Lost** | About To Sleep, Hibernating, Lost, Need Attention |

This grouping helps summarize customer behavior into **four strategic customer groups**, making it easier to analyze customer distribution, revenue contribution, and trends over time.

<img width="803" height="609" alt="image" src="https://github.com/user-attachments/assets/511ace95-a001-44ae-9209-8660adf0f52e" />


### Analysis

The pie chart shows that:

- **Inactive & Lost** account for the largest share at **37.2%**
- **Loyal & High Value** account for **33.3%**
- **High Risk Customers** account for **15.1%**
- **New & Potential** account for **14.4%**

### Insight

This grouped view provides a more strategic perspective on the customer base.

The customer portfolio is nearly balanced between:

- customers who are currently valuable and engaged
- customers who are already inactive or becoming inactive

This suggests a key business challenge:

> while the company has a strong high-value base, a very large share of customers is already disconnected or close to churn.

This reinforces the need for two parallel strategies:

1. **retain and grow Loyal & High Value customers**
2. **reactivate High Risk / Inactive customers where possible**

---

# 6.6 Segment Distribution Over Time

This line chart tracks how the grouped customer segments change across months.

<img width="1134" height="560" alt="image" src="https://github.com/user-attachments/assets/db45bffd-7518-47cb-bde5-8db05ef45634" />


### Analysis

Several patterns stand out:

- **Loyal & High Value** customers remain the dominant group throughout the timeline and increase noticeably toward the later months
- **Inactive & Lost** also rise sharply in certain months, especially around October
- **New & Potential** spike strongly in November, suggesting a wave of newly acquired or recently active customers
- **High Risk Customers** remain relatively stable early on but decline sharply toward the end

### Insight

This chart suggests that customer behavior is dynamic and influenced by seasonality.

The rise in **Loyal & High Value** and **New & Potential** customers toward the final quarter aligns with the earlier UK monthly revenue trend, where sales peaked in late 2011.

This indicates that peak season not only increases revenue, but also improves the composition of active customer segments.

However, the fluctuations in **Inactive & Lost** show that customer drop-off remains a serious issue outside peak periods.

---

# 6.7 Trend of Recency by Group Segment

This stacked area chart shows how **Recency** changes over time across grouped customer segments.
<img width="1099" height="424" alt="image" src="https://github.com/user-attachments/assets/4b9ee955-9ccc-441d-b23b-c477fb3d3705" />


### Analysis

Recency trends vary meaningfully by segment group.

- **Loyal & High Value** maintain relatively low recency compared with weaker groups, indicating more recent purchase activity
- **Inactive & Lost** and **High Risk Customers** tend to contribute more to higher recency values
- Toward the final months, overall recency declines sharply, which is expected because more customers become active during peak season

### Insight

Lower recency in the later months suggests that more customers are purchasing recently, especially during peak demand periods.

This is another signal that the business experiences stronger customer activation in the final quarter.

---

# 6.8 Trend of Frequency by Group Segment

This stacked area chart tracks **Frequency** over time across grouped segments.

<img width="1163" height="409" alt="image" src="https://github.com/user-attachments/assets/86ca5750-1a23-42ca-9c44-4b14df612052" />


### Analysis

The most important pattern is the dominance of **Loyal & High Value** customers in total frequency.

- This group drives the largest share of repeat purchase activity across all months
- Frequency rises significantly in September, October, and especially November
- **New & Potential** also contribute more strongly in the later months, indicating new customer activation
- **High Risk Customers** decline toward the end

### Insight

This chart confirms that **Frequency is one of the main drivers of business performance**.

The later-month increase suggests that customer repeat purchasing intensifies during peak season.

Since Frequency is central to loyalty, this supports the business impact argument that increasing repeat purchases is crucial for sustainable revenue growth.

---

# 6.9 Trend of Monetary by Group Segment

This stacked area chart tracks **Monetary value** over time across grouped segments.

<img width="1115" height="439" alt="image" src="https://github.com/user-attachments/assets/a5a5b164-b295-4918-9df1-bd98c4b8f0e7" />


### Analysis

Monetary contribution is heavily dominated by **Loyal & High Value** customers.

- This group consistently contributes the largest share of total monetary value
- Total monetary value rises sharply from September onward and peaks in November
- **New & Potential** also contribute more in the late months
- **Inactive & Lost** and **High Risk Customers** contribute less consistently

### Insight

This confirms that the business depends heavily on its loyal and high-value customers for revenue generation.

The spike in late 2011 suggests that the business benefits strongly from seasonal demand, but this also highlights the importance of maintaining a healthy core customer base outside seasonal peaks.



# 🔑 Key Takeaways from Visualization & Analysis

Several major conclusions can be drawn from the visual analysis:

### 1. Champions are the most important segment
They are the largest segment and generate by far the most revenue, making them the core customer group.

### 2. At Risk customers are commercially important
Although not as large as Champions, they contribute substantial revenue, meaning they should be prioritized for win-back strategies.

### 3. Potential Loyalists represent future growth
They are highly recent customers with room to increase in both frequency and monetary value.

### 4. Inactive & Lost customers account for a large share of the customer base
This indicates customer disengagement is a major challenge and should not be ignored.

### 5. Frequency is a major business driver
The trend charts show that repeat purchasing activity is strongly associated with growth in both customer value and revenue, especially during peak season.

---

# 💡 7. Insight & Recommendation


## A. Customer Segmentation Strategy

👉 Based on the insights above, we recommend the following:

### 1. Loyal & High Value Group (38.3%)

- 38.3% of customers, **highest revenue contribution**.
- Strong purchasing frequency and stable engagement.
- High willingness to spend and respond to premium offerings.

🔹 **Recommendation:**

- Analyze preferred products and introduce **premium or bundled versions**.
- Implement **upselling and cross-selling strategies**.
- Strengthen **loyalty programs, referral incentives, and exclusive promotions**.

Impact:  
Protecting and nurturing this group helps **maximize customer lifetime value and stabilize revenue**.

---

### 2. High-Risk Customers (23.5%)

- Previously high-value customers but **declining engagement**.
- Increasing Recency and decreasing purchase frequency.
- Risk of moving into inactive segments.

🔹 **Recommendation:**

- Launch **re-engagement campaigns** with personalized incentives.
- Offer **targeted discounts and time-limited promotions**.
- Reach out to high-value customers to understand **reasons for disengagement**.

Impact:  
Reactivating these customers can **recover lost revenue and reduce churn risk**.

---

### 3. New & Potential Customers (11.7%)

- Lower spending but **strong growth potential**.
- Increasing purchase activity but still unstable.
- Opportunity to build long-term loyalty.

🔹 **Recommendation:**

- Develop **onboarding strategies to encourage second purchases**.
- Run **nurturing campaigns with welcome offers**.
- Use **reviews and recommendations to build trust**.

Impact:  
Encouraging repeat purchases helps **convert new customers into loyal buyers**.

---

## B. Business Recommendation

In an e-commerce retail environment, the RFM (Recency, Frequency, Monetary) model supports targeted marketing strategies, with a primary focus on **Frequency (F)**.

### Why Focus on Frequency (F)?

🔹 **Increasing Purchase Frequency = Sustainable Revenue Growth**

- More frequent purchases lead to **stable and predictable revenue**.
- Retaining existing customers is **cheaper than acquiring new ones**, reducing Customer Acquisition Cost (CAC).

🔹 **Insights from the Charts: High-Risk & New Customers Have Lower Frequency**

- **High-Risk Customers:** previously valuable but now purchasing less frequently.  
  Without intervention, they may move into inactive segments.

- **New & Potential Customers:** low purchase frequency indicates that buying habits are not yet established.

🔹 **Even Loyal Customers Need Retention**

- Loyal customers may still reduce purchase frequency over time.
- Businesses should maintain engagement through **loyalty programs, personalized offers, and exclusive deals**.

---

### Final Takeaway

Customer value is highly concentrated in **loyal and high-frequency buyers**, while a significant portion of customers are either new or at risk of churn.

By focusing on **increasing purchase frequency, retaining high-value customers, and reactivating declining segments**, the business can improve **customer lifetime value and long-term revenue growth**.
