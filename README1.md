# 💄 Nykaa Marketing Campaign — Exploratory Data Analysis

An end-to-end EDA on Nykaa's digital marketing campaign data, built entirely in **Python** (Pandas, NumPy, Matplotlib, Seaborn, Plotly). The goal is to understand how campaigns, channels, and customer segments are performing in terms of revenue, cost, and return on investment — and to turn raw ad-spend numbers into a clear, decision-ready story.

---

## 📌 Project Overview

Nykaa runs marketing campaigns across multiple channels (Email, Social Media, Search, Influencer, etc.) targeting different customer segments. This project analyzes that campaign-level data to answer questions like:

- 💰 Which campaigns and channels generate the most revenue?
- 🎯 Which customer segments respond best to marketing spend?
- 📉 Where is money being spent inefficiently (high cost, low return)?
- 🔻 How many people drop off at each stage of the marketing funnel (Impressions → Clicks → Leads → Conversions)?
- 📈 How does revenue trend over time?

The notebook (`nykaa.ipynb`) walks through this analysis step-by-step, from raw data to an interactive dashboard.

---

## 🗂️ Dataset

**File used:** `nykaa_data.csv`

The dataset contains one row per campaign record with fields such as:

| Column | Description |
|---|---|
| `Date` | Date of the campaign activity |
| `Campaign_Type` | Type of campaign (e.g. Awareness, Conversion, Retention) |
| `Channel_Used` | Marketing channel (Email, Social Media, Search, etc.) |
| `Customer_Segment` | Segment targeted (e.g. New, Returning, Loyal) |
| `Target_Audience` | Audience group targeted |
| `Impressions` | Number of ad impressions |
| `Clicks` | Number of clicks received |
| `Leads` | Number of leads generated |
| `Conversions` | Number of conversions completed |
| `Acquisition_Cost` | Cost spent to acquire customers |
| `Revenue` | Revenue generated |
| `ROI` | Return on investment (as provided in the raw data) |

---

## 🧰 Tools & Libraries

Everything is done in **pure Python** — no external BI tools required.

| Library | Purpose |
|---|---|
| `pandas` | Data loading, cleaning, grouping, aggregation |
| `numpy` | Safe numeric calculations (e.g. avoiding division by zero) |
| `matplotlib` | Static charts (bar plots, line plots) |
| `seaborn` | Styling support for statistical plots |
| `plotly.express` / `plotly.graph_objects` | Interactive charts (bar, treemap, funnel, sunburst) |
| `plotly.subplots` | Combining multiple charts into one dashboard view |

Install everything with:

```bash
pip install pandas numpy matplotlib seaborn plotly
```

---

## 🔍 Step-by-Step Breakdown of the Analysis

### 1️⃣ Setup
Import libraries, set default figure size, and silence noisy warnings so the notebook output stays clean.

### 2️⃣ Data Loading & First Look
```python
df = pd.read_csv("nykaa_data.csv")
df.head()
```
A quick peek at the first few rows to confirm the data loaded correctly.

### 3️⃣ Structure Check
```python
df.shape
df.info()
```
Checks the number of rows/columns and the data type of every column (numeric vs text vs date).

### 4️⃣ Missing Value Check
```python
df.isnull().sum().sort_values(ascending=False)
```
Identifies which columns (if any) have missing values, so they can be handled before analysis.

### 5️⃣ Statistical Summary
```python
df.describe().T
```
Gives mean, min, max, standard deviation, and quartiles for every numeric column — a fast way to spot outliers or unusual ranges.

### 6️⃣ 🧮 Feature Engineering — Building Marketing KPIs
Raw impressions, clicks, and cost don't tell the full story on their own, so new performance metrics are calculated (all protected against divide-by-zero using `np.where`):

- **CTR (Click-Through Rate)** → `Clicks / Impressions × 100`
- **Conversion Rate** → `Conversions / Clicks × 100`
- **Cost Per Click (CPC)** → `Acquisition_Cost / Clicks`
- **Cost Per Lead (CPL)** → `Acquisition_Cost / Leads`
- **Cost Per Acquisition (CPA)** → `Acquisition_Cost / Conversions`
- **ROAS (Return on Ad Spend)** → `Revenue / Acquisition_Cost`

These KPIs are then summarized with `.describe()` to understand the spread of performance across all campaigns.

### 7️⃣ 📊 Overall Business Snapshot
Simple aggregate totals are printed to get the big picture in seconds:

- Total Revenue 💵
- Total Acquisition Cost 💸
- Total Impressions 👀
- Total Clicks 🖱️
- Total Leads 🧲
- Total Conversions ✅
- Average ROI and Average ROAS 📈

### 8️⃣ 🏆 Revenue by Campaign Type
```python
df.groupby("Campaign_Type")["Revenue"].sum().sort_values(ascending=False)
```
Followed by a bar chart to visually rank which campaign types (e.g. Awareness vs Conversion vs Retention) drive the most revenue — and a separate check on which campaign type has the **best average ROI**, since the highest revenue campaign isn't always the most efficient one.

### 9️⃣ 📡 Channel Performance
```python
df.groupby("Channel_Used").agg({"Revenue":"sum", "Conversions":"sum", "ROI":"mean"})
```
Compares every marketing channel side-by-side on three metrics at once — revenue generated, conversions delivered, and average ROI — to find the most valuable channel overall.

### 🔟 👥 Customer Segment Analysis
Groups the data by `Customer_Segment` to see which type of customer (new, returning, loyal, etc.) contributes the most revenue and conversions, visualized with a horizontal bar chart.

### 1️⃣1️⃣ 🎯 Target Audience Analysis
Similarly groups by `Target_Audience` to see which audience group is the most profitable to target.

### 1️⃣2️⃣ 🔻 Marketing Funnel
Builds a simple funnel table:

```
Impressions → Clicks → Leads → Conversions
```
This shows exactly where potential customers drop off — e.g. a huge gap between Impressions and Clicks would suggest a creative/targeting problem, while a gap between Leads and Conversions would point to a sales/follow-up issue.

### 1️⃣3️⃣ 📅 Monthly Revenue Trend
```python
df["Date"] = pd.to_datetime(df["Date"])
df["Month"] = df["Date"].dt.to_period("M")
```
Revenue is grouped by month and plotted as a line chart to reveal seasonality, growth, or decline over time.

### 1️⃣4️⃣ 🖥️ Interactive Dashboard (Plotly)
The final and most visual part of the notebook builds a **single-page marketing dashboard** using `plotly.subplots`, combining:

- 🔢 **KPI Cards** — Total Revenue, ROAS & ROI at a glance
- 📊 **Bar Chart** — Revenue by Channel
- 🌳 **Treemap** — Revenue by Customer Segment
- 📈 **Line Chart** — Monthly Revenue Trend
- 🔻 **Funnel Chart** — Full marketing funnel visualization

All styled in `plotly_dark` theme for a clean, presentation-ready look.

### 1️⃣5️⃣ ☀️ Sunburst Chart
A final drill-down visualization showing the hierarchy:
```
Channel → Campaign Type → Customer Segment
```
sized by revenue, making it easy to spot the exact combination (e.g. "Social Media → Conversion Campaign → Loyal Customers") that performs best.

---

## 📁 Suggested Project Structure

```
nykaa-marketing-eda/
│
├── nykaa.ipynb          # main analysis notebook
├── nykaa_data.csv        # raw dataset (add your own)
├── README.md              # this file
└── requirements.txt       # pinned dependencies
```

---

## ▶️ How to Run

1. Clone this repository
2. Add `nykaa_data.csv` to the project folder
3. Install the required libraries:
   ```bash
   pip install -r requirements.txt
   ```
4. Open the notebook:
   ```bash
   jupyter notebook nykaa.ipynb
   ```
5. Run all cells from top to bottom ▶️

---

## 💡 Key Takeaways You Can Expect

- A clear ranking of **which campaigns and channels are worth the spend**, and which aren't.
- A full picture of **funnel drop-off**, useful for pinpointing where the marketing pipeline is leaking.
- KPI-driven view (CTR, CPC, CPL, CPA, ROAS) instead of relying on raw totals alone.
- An interactive dashboard that can be shared with stakeholders without needing external BI software.

---

## 🙌 Author's Note

This project is meant as a practical, from-scratch example of marketing analytics using only Python — no Excel pivot tables, no Power BI/Tableau. Every KPI and chart in this README traces back directly to code inside `nykaa.ipynb`, so it can be reproduced end-to-end on any similar campaign dataset.
