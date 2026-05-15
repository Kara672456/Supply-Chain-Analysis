<div align="center">

<img src="https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white"/>
<img src="https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Seaborn-4c9fc8?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white"/>

<br/><br/>

# 📦 Supply Chain Analysis

### A comprehensive end-to-end data analysis of the DataCo Global Supply Chain Dataset

*Uncovering delivery bottlenecks, profitability drivers, and operational failures across 172,765 orders (2015–2018)*

<br/>

[📊 View Analysis](#-analysis-modules) • [📈 Key Findings](#-key-findings) • [🚀 Quick Start](#-quick-start) • [📄 Report](#-report)

---

</div>

<br/>

## 🗂️ Table of Contents

- [Project Overview](#-project-overview)
- [Business Problem](#-business-problem)
- [Dataset](#-dataset)
- [Project Structure](#-project-structure)
- [Quick Start](#-quick-start)
- [Analysis Modules](#-analysis-modules)
- [Key Findings](#-key-findings)
- [Visualizations](#-visualizations)
- [Strategic Recommendations](#-strategic-recommendations)
- [Tech Stack](#-tech-stack)
- [Report](#-report)
- [License](#-license)

---

<br/>

## 🔭 Project Overview

This project performs a full-stack exploratory and diagnostic analysis of a real-world global supply chain. Starting from raw transactional data, it systematically identifies **where delays happen**, **what causes them**, **how much they cost**, and **what can be done about it** — all backed by charts, KPI tables, and a professionally generated PDF report.

The analysis is structured in six progressive layers, from basic EDA through to root cause analysis and time-series patterns, making it suitable both as a standalone business intelligence deliverable and as a portfolio project demonstrating end-to-end data analytics skills.

<br/>

## 💼 Business Problem

> **"More than half of all orders are arriving late. Where is the supply chain breaking down, and how much is it costing us?"**

| Metric | Value |
|:---|:---|
| 📦 Total Orders Analysed | 172,765 |
| ⏰ Late Delivery Rate | **54.71%** |
| 💰 Total Profit Generated | **$7.5M** |
| 🔥 Profit at Risk from Delays | **$2.1M** |
| ✅ On-Time Delivery Rate | **45.29%** |
| 📅 Dataset Period | Jan 2015 – Jan 2018 |

The business is broadly profitable (80.7% of orders generate positive returns), but the scale of late deliveries represents both a customer satisfaction crisis and a significant financial risk. This analysis pinpoints the exact operational failures driving the problem.

---

<br/>

## 📁 Dataset

**Source:** DataCo Supply Chain Dataset — `DataCoSupplyChainDataset.csv`

| Property | Raw | After Cleaning |
|:---|:---:|:---:|
| Rows | 180,519 | 172,765 |
| Columns | 53 | 20 |
| Duplicates | 0 | 0 |
| Cancelled orders removed | — | 7,754 |
| Encoding | Latin-1 | Latin-1 |

<br/>

### 🧹 Cleaning Steps

1. Dropped columns with 100% missing values (`Product Description`, `Product Image`)
2. Dropped high-missing columns (`Order Zipcode` — 86.2% missing)
3. Removed PII columns (email, password, customer names, addresses)
4. Removed ID/surrogate key columns not needed for analysis
5. Filtered out `Shipping canceled` orders (7,754 rows)
6. Parsed date columns to `datetime` with `dayfirst=False`

<br/>

### 📌 Key Columns

| Column | Type | Description |
|:---|:---:|:---|
| `Days for shipping (real)` | int | Actual days taken to ship the order |
| `Days for shipment (scheduled)` | int | Originally planned shipping duration |
| `Delivery Status` | str | Late delivery / Advance shipping / Shipping on time |
| `Late_delivery_risk` | int | Binary flag — 1 if order is at risk of late delivery |
| `Order Profit Per Order` | float | Net profit (or loss) for each order |
| `Sales` | float | Revenue generated per order |
| `Order Item Profit Ratio` | float | Profit as a ratio of item price |
| `Shipping Mode` | str | Standard Class / Second Class / First Class / Same Day |
| `Order Region` | str | Geographic region of delivery destination |
| `Customer Segment` | str | Consumer / Corporate / Home Office |
| `Department Name` | str | Product department (Fitness, Golf, Technology, etc.) |
| `Type` | str | Payment method — DEBIT / TRANSFER / PAYMENT / CASH |
| `Order Status` | str | COMPLETE / PENDING / PROCESSING / ON_HOLD / etc. |

<br/>

### 🔧 Engineered Features

```python
# Order processing time
df['Order Processing Time'] = (
    df['shipping date (DateOrders)'] - df['order date (DateOrders)']
).dt.days

# Delay vs schedule
df['Delay']      = df['Order Processing Time'] - df['Days for shipment (scheduled)']
df['Is_Delayed'] = df['Delay'] > 0

# Time features
df['order_month'] = df['order date (DateOrders)'].dt.month
df['order_day']   = df['order date (DateOrders)'].dt.day_name()
df['order_hour']  = df['order date (DateOrders)'].dt.hour

# Profitability classification
df['Profitability Flag'] = np.where(
    df['Order Profit Per Order'] > 0, 'Profit',
    np.where(df['Order Profit Per Order'] < 0, 'Loss', 'Break Even')
)
```

---

<br/>

## 🗃️ Project Structure

```
📦 supply-chain-analysis/
│
├── 📓 supply_chain_analysis.ipynb     ← Main analysis notebook
├── 📄 build_report.py                 ← PDF report generator
├── 📋 README.md                       ← You are here
│
├── 📂 data/
│   └── DataCoSupplyChainDataset.csv   ← Raw dataset (download separately)
│
├── 📂 charts/                         ← Auto-generated chart PNGs
│   ├── chart_profitability_pie.png
│   ├── chart_delay_dist.png
│   ├── chart_profit_delay.png
│   ├── chart_region_delay.png
│   ├── chart_shipping_delay.png
│   ├── chart_root_cause.png
│   ├── chart_monthly_trend.png
│   └── chart_kpi_combined.png
│
└── 📂 outputs/
    └── Supply_Chain_Analysis_Report.pdf   ← Final 12-page PDF report
```

---

<br/>

## 🚀 Quick Start

### Prerequisites

- Python 3.8 or higher
- pip

<br/>

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/supply-chain-analysis.git
cd supply-chain-analysis
```

<br/>

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

**`requirements.txt`**
```
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
seaborn>=0.11.0
reportlab>=3.6.0
pdf2image>=1.16.0
jupyter>=1.0.0
```

<br/>

### 3. Add the Dataset

Download `DataCoSupplyChainDataset.csv` and place it in the `data/` folder, or update the path in the notebook:

```python
df = pd.read_csv('data/DataCoSupplyChainDataset.csv', encoding='latin-1')
```

<br/>

### 4. Run the Notebook

```bash
jupyter notebook supply_chain_analysis.ipynb
```

<br/>

### 5. Generate the PDF Report

```bash
python build_report.py
```

> Output: `outputs/Supply_Chain_Analysis_Report.pdf`

---

<br/>

## 🔬 Analysis Modules

### Module 1 — Exploratory Data Analysis

> *Understanding the shape, quality, and distributions of the raw data*

- Dataset dimensions and dtypes
- Duplicate and missing value detection
- Value counts for all low-cardinality categorical columns
- Identification of columns to drop (PII, IDs, high-missing)

```python
print("rows, cols:", df.shape)
print("Duplicates:", df.duplicated().sum())
print(df.isnull().sum().sort_values(ascending=False).head(20))
```

**Categorical distributions found:**

| Variable | Top Category | Count |
|:---|:---|:---:|
| Shipping Mode | Standard Class | 103,153 |
| Customer Segment | Consumer | 89,420 |
| Payment Type | DEBIT | 69,295 |
| Delivery Status | Late delivery | 98,977 |

---

### Module 2 — Business KPI Dashboard

> *Translating raw data into core business metrics*

```
┌─────────────────┬─────────────────┬─────────────────┬─────────────────┐
│  Total Orders   │ Late Deliveries │  On-Time Rate   │  Late Del. Rate │
│    172,765      │     94,523      │    45.29%       │    54.71%       │
├─────────────────┼─────────────────┼─────────────────┼─────────────────┤
│  Total Profit   │ Loss from Delay │  90th %ile Del. │  Avg. Process   │
│    $7.5M        │     $2.1M       │    3 Days       │   3.47 Days     │
└─────────────────┴─────────────────┴─────────────────┴─────────────────┘
```

---

### Module 3 — Profitability Analysis

> *Understanding the financial health of the supply chain*

```python
df['Profitability Flag'].value_counts()
# Profit      139,354  (80.7%)
# Loss         32,295  (18.7%)
# Break Even    1,116   (0.6%)
```

**Profit by delay bucket:**

| Delay | Mean Profit | Total Profit | Orders |
|:---:|:---:|:---:|:---:|
| -2 days (Early) | **$23.36** ↑ | $487,596 | 20,873 |
| -1 day (Early) | $21.60 | $447,629 | 20,719 |
| 0 days (On Time) | $22.25 | $815,430 | 36,650 |
| +1 day (Late) | $22.33 | $1,194,895 | 53,503 |
| +2 days (Late) | $21.13 | $582,111 | 27,551 |
| +3 days (Late) | **$20.03** ↓ | $135,653 | 6,772 |
| +4 days (Late) | $21.37 | $143,107 | 6,697 |

> 📌 Mean profit is **14% lower** at +3 days delay compared to -2 days early.

---

### Module 4 — Bottleneck Detection

> *Identifying which operational dimensions drive the most delays*

```python
def compute_delay_pct_by_category(category):
    cat_df = df.groupby(category).agg(
        Total_Orders = ('Delay', 'count'),
        late_Orders  = ('Delay', 'sum')
    ).reset_index()
    cat_df['delay_pct'] = cat_df['late_Orders'] / cat_df['Total_Orders'] * 100
    return cat_df.sort_values('delay_pct', ascending=False).head(10)

categories = ['Order Region', 'Customer Segment', 'Shipping Mode',
              'Order Status', 'Type', 'Department Name']
```

**Delay rates across dimensions:**

| Dimension | Worst Performer | Delay % |
|:---|:---|:---:|
| 🌍 Order Region | Central Africa | **61.3%** |
| 🚚 Shipping Mode | Second Class | **56.8%** |
| 👥 Customer Segment | Home Office | **55.7%** |
| 📋 Order Status | ON_HOLD | **60.3%** |
| 💳 Payment Type | PAYMENT | **56.4%** |
| 🏪 Department | Pet Shop | **64.8%** |

---

### Module 5 — Root Cause Analysis

> *Drilling into the worst region (Central Africa) to find specific failure drivers*

```python
def top_drivers_for_region(region):
    df_region = df[df['Order Region'] == region].copy()
    drivers = ['Shipping Mode', 'Customer Segment',
               'Order Status', 'Type', 'Department Name']
    # multi-factor delay percentage calculation across all drivers
    # returns top 10 ranked by delay_pct
```

**Top 10 drivers in Central Africa:**

| Rank | Driver | Delay % |
|:---:|:---|:---:|
| 1 | Shipping Mode: First Class | 🔴 **100.0%** |
| 2 | Shipping Mode: Second Class | 🔴 **82.8%** |
| 3 | Order Status: PAYMENT_REVIEW | 🔴 **80.0%** |
| 4 | Order Status: PENDING | 🟠 **69.1%** |
| 5 | Type: PAYMENT | 🟠 **63.6%** |
| 6 | Type: TRANSFER | 🟠 **63.3%** |
| 7 | Order Status: PENDING_PAYMENT | 🟠 **62.6%** |
| 8 | Department: Outdoors | 🟡 **61.3%** |
| 9 | Department: Golf | 🟡 **60.8%** |
| 10 | Customer Segment: Consumer | 🟡 **60.2%** |

---

### Module 6 — Time Series Analysis

> *Finding when delays spike to enable proactive planning*

```python
delay_by_month = df.groupby('order_month')['Is_Delayed'].mean().reset_index()
delay_by_day   = df.groupby('order_day')['Is_Delayed'].mean().reset_index()
delay_by_hour  = df.groupby('order_hour')['Is_Delayed'].mean().reset_index()
```

**Monthly delay rates:**

| Month | Delay % | | Month | Delay % |
|:---|:---:|---|:---|:---:|
| January | 54.2% | | July | 55.2% |
| February | 54.8% | | August | 54.5% |
| March | 53.8% | | September | 55.2% |
| April | 53.8% | | **October** | **59.4% 🔴** |
| May | 54.6% | | **November** | **59.4% 🔴** |
| June | 54.0% | | December | 55.2% |

**Peak delay hours:** 11 PM (57.1%), 2 PM (56.7%), 10 AM (56.0%)

**Peak delay days:** Monday (55.5%), Wednesday & Thursday (55.2%)

---

<br/>

## 📊 Key Findings

<br/>

### 🔴 Critical Issues

> **1. Majority of orders are late**
> 54.71% of all 172,765 orders miss their scheduled delivery date. The supply chain is in a state of chronic lateness, not occasional disruption.

> **2. $2.1M profit tied to delayed orders**
> Of the $7.5M total profit, $2.1M (~28%) is generated by orders that arrived late — representing revenue at constant risk of erosion through customer churn and returns.

> **3. First Class shipping to Central Africa: 100% failure rate**
> Not a single First Class shipment to Central Africa arrived on time across the entire 3-year dataset. This is a complete and unaddressed operational failure.

<br/>

### 🟠 High-Priority Issues

> **4. Pet Shop department: 64.8% delay rate**
> The highest-delayed department by a significant margin. Likely caused by supply chain specifics around live/perishable goods or seasonal demand spikes.

> **5. Q4 holiday surge drives delay peaks**
> October and November consistently reach 59.4% delay rates — 5+ percentage points above the annual average — driven by holiday order volumes without proportional capacity increases.

<br/>

### 🟢 Positive Signals

> **6. Business is fundamentally profitable**
> 80.7% of orders generate profit, with a healthy mean of $22.03 per order. The core business model works — the delivery operations need fixing, not the business itself.

> **7. CASH payments have the lowest delay rate (51.9%)**
> A consistent pattern suggesting faster internal processing for cash transactions. Understanding why could unlock improvements across other payment types.

---

<br/>

## 📈 Visualizations

The project generates **8 publication-quality charts** embedded in the PDF report:

| # | Chart | Type | Key Insight |
|:---:|:---|:---:|:---|
| 1 | Profitability Distribution | Pie | 80.7% profitable, 18.7% loss-making |
| 2 | On-Time vs Late + Dept Delays | Donut + Bar | 54.7% late; Pet Shop worst dept |
| 3 | Order Delay Distribution | Bar | 31% of orders arrive exactly 1 day late |
| 4 | Profit by Delay Days | Dual-axis Line+Bar | Mean profit falls $3.33 from -2 to +3 days |
| 5 | Delay Rate by Region | Horizontal Bar | Central Africa (61.3%) leads globally |
| 6 | Delay Rate by Shipping Mode | Bar | Second Class worst; Standard Class best |
| 7 | Central Africa Root Cause | Horizontal Bar | First Class = 100% delay rate |
| 8 | Monthly Delay Trend | Line | Oct/Nov peak clearly visible at 59.4% |

All charts use a consistent colour palette: teal for positive/good metrics, red for critical issues, amber for moderate-risk findings.

---

<br/>

## 🎯 Strategic Recommendations

### 🚨 P1 — Immediate Action Required

| Action | Target | Expected Impact |
|:---|:---|:---|
| Suspend First & Second Class shipping to Central Africa | Shipping / Regional Ops | Eliminate 100% and 82.8% delay rates immediately |
| Auto-flag PAYMENT_REVIEW and PENDING_PAYMENT orders for fast-tracking | Order Management | Reduce 58–56% delay rates in these statuses |

### 🟠 P2 — High Priority (Next Quarter)

| Action | Target | Expected Impact |
|:---|:---|:---|
| Pre-position Pet Shop & Fitness inventory before Q4 | Inventory Planning | Address 64.8% and 56.1% dept delay rates |
| Increase staffing on Mondays and 10 AM–3 PM peak hours | Workforce Planning | Reduce peak-hour delays by ~2–3% |
| Regional logistics audit: Central Africa, Central Asia, US Center | Regional Operations | Identify carrier and routing failures |

### 🟡 P3 — Medium Priority (This Year)

| Action | Target | Expected Impact |
|:---|:---|:---|
| Deploy SLA monitoring dashboards; target 65%+ on-time rate | KPI & Monitoring | Visibility-driven continuous improvement |
| Investigate why CASH payments have lowest delay (51.9%) | Payment Operations | Apply learnings to DEBIT, TRANSFER, PAYMENT types |

---

<br/>

## 🛠️ Tech Stack

| Library | Version | Purpose |
|:---|:---:|:---|
| **Python** | 3.8+ | Core language |
| **Pandas** | ≥1.3 | Data loading, cleaning, aggregation, groupby |
| **NumPy** | ≥1.21 | Numerical operations, `np.where` profitability flags |
| **Matplotlib** | ≥3.4 | Base chart generation (bar, line, pie, dual-axis) |
| **Seaborn** | ≥0.11 | Statistical styling, `sns.barplot` for bottleneck charts |
| **ReportLab** | ≥3.6 | Programmatic PDF generation with tables, images, canvas |
| **pdf2image** | ≥1.16 | PDF page rendering for quality checks |
| **Jupyter** | ≥1.0 | Interactive notebook environment |

---

<br/>

## 📄 Report

The `build_report.py` script generates a fully formatted **12-page PDF report** including:

| Page | Content |
|:---:|:---|
| 1 | Cover page — title, metadata, dark blue design |
| 2 | Executive Summary + Profitability Pie Chart |
| 3 | Dataset Overview, EDA Tables, Categorical Distributions |
| 4 | KPI Dashboard (metric boxes) + On-Time/Dept chart |
| 5 | Delay Distribution Chart + Profit by Delay Chart + data tables |
| 6 | Regional Delay Chart + Shipping Mode Chart + Status/Payment tables |
| 7 | Central Africa Root Cause Chart + Monthly Trend Chart + tables |
| 8–12 | Conclusions, Findings Summary, Recommendations, Conclusion text |

```bash
python build_report.py
# Output → outputs/Supply_Chain_Analysis_Report.pdf
```

---

<br/>

## 📜 License

This project is intended for educational and portfolio purposes.

The **DataCo Supply Chain Dataset** is a publicly available dataset. Please refer to its original source for licensing terms.

---

<br/>

<div align="center">

Made with 🐍 Python & 📊 Data

*If you found this useful, give it a ⭐ on GitHub*

</div>
