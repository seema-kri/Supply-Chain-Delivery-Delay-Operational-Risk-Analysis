# Supply Chain Delivery Delay & Operational Risk Analysis

> Analyzing 172K+ e-commerce orders to find why 54.7% of deliveries run late, what it puts at risk, and predicting late orders before dispatch using **Python, Pandas, Seaborn, Scikit-Learn and SMOTE**.

---

## Table of Contents

1. [Overview](#overview)
2. [Problem Statement](#problem-statement)
3. [Dataset Description](#dataset-description)
4. [Tools & Technologies](#tools--technologies)
5. [Project Structure](#project-structure)
6. [Data Cleaning & Preparation](#data-cleaning--preparation)
7. [EDA & Key Insights](#eda--key-insights)
8. [Machine Learning Model](#machine-learning-model)
9. [Dashboard & Visuals](#dashboard--visuals)
10. [How to Run This Project](#how-to-run-this-project)
11. [Final Recommendations & Future Work](#final-recommendations--future-work)
12. [Author & Contact](#author--contact)

---

## Overview

This project studies the delivery operations of a global e-commerce company that sells sporting goods, fitness equipment, outdoor gear, footwear and apparel across multiple regions. It covers **172,765 orders (Jan 2015 to Jan 2018)** and answers four questions:

- How often are orders late, and by how much?
- How much profit sits on delayed orders?
- Where are the bottlenecks (region, shipping mode, order status, time)?
- Can we predict a late order before it ships?

A full written report is available in [`reports/`](reports/).

## Problem Statement

Actual shipping times often differ from scheduled delivery windows. This causes late deliveries, lost customer trust, unpredictable order profitability, and no way to make reliable delivery promises at checkout.

**Goal:** analyze delivery operations, identify bottlenecks, and build a predictive model that flags high-risk orders so the business can reduce delays and protect profit.

## Dataset Description

| Item | Detail |
|---|---|
| Source | DataCo Smart Supply Chain dataset ([Kaggle](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis)) |
| File | `data/DataCoSupplyChainDataset.xls` |
| Raw size | 180,519 rows x 53 columns |
| After cleaning | 172,765 rows x 20 columns (canceled shipments removed) |
| Duplicates | 0 |
| Period | 2015-01-01 to 2018-01-31 |
| Key columns | Shipping Mode, Days for shipment (scheduled), Days for shipping (real), Delivery Status, Late_delivery_risk, Order Region, Order Status, Order Profit Per Order, order date, shipping date |

## Tools & Technologies

- **Language:** Python 3
- **Data analysis:** Pandas, NumPy
- **Visualization:** Matplotlib, Seaborn
- **Machine learning:** Scikit-Learn (Random Forest), Imbalanced-Learn (SMOTE)
- **Environment:** Jupyter Notebook
- **Version control:** Git, GitHub

## Project Structure

```
Supply-Chain-Delivery-Delay-Operational-Risk-Analysis/
├── data/
│   └── DataCoSupplyChainDataset.xls
├── notebooks/
│   └── supply_chain.ipynb
├── reports/
│   └── README.md
├── visuals/
│   ├── bottleneck_detection_by_category.png
│   ├── delay_distribution_and_profit_analysis.png
│   ├── delay_trend_month_day_hour.png
│   ├── profitability_distribution.png
│   └── top_drivers_late_delivery_central_africa.png
├── .gitignore
├── README.md
└── requirements.txt
```

## Data Cleaning & Preparation

- Dropped personal and free-text columns (names, email, password, street, zip codes), ID columns, a fully empty column (Product Description), a single-value column (Product Status) and a duplicate of profit (Benefit per order).
- Removed **canceled shipments** (172,765 orders kept).
- Converted order and shipping dates to datetime.
- Created features:
  - `Order Processing Time` = shipping date minus order date (days)
  - `Delay` = Order Processing Time minus Scheduled Days
  - `Is_Delayed` = Delay greater than 0
  - `order_month`, `order_day`, `order_hour`
  - `Profitability Flag` = Profit / Loss / Break-even
- **Note:** the dataset flag `Late_delivery_risk` marks 57.29% of orders as late. The Delay-based definition above gives 54.71%. Business analysis uses the Delay-based rate. The ML model predicts `Late_delivery_risk`.

## EDA & Key Insights

### KPIs

| Metric | Value |
|---|---|
| Total orders | 172,765 |
| Late deliveries | 94,523 |
| **Late delivery rate** | **54.71%** |
| On-time rate | 45.29% |
| Total profit (profitable orders only) | $7.5M |
| Profit on delayed orders | $2.1M |
| 90th percentile delay | 3 days |
| Mean profit per order | $22.03 |

### Key findings

- **Shipping mode is the biggest lever.** Late rate: First Class **100%**, Second Class **79.8%**, Standard Class **39.8%**, Same Day **0%**.
- **Delay is mostly 1 day.** 31.0% of orders arrive exactly 1 day late. Orders delayed 1 to 4 days make up the full 54.7%.
- **Unit profit does not change with delay.** Mean profit stays around $20 to $23 at every delay level, so the damage comes from the volume of late orders.
- **Profitability:** 80.7% of orders are profitable, 18.7% lose money, 0.6% break even.
- **Regions:** Central Africa is the worst at 58.7%. Other top regions sit at 55.1% to 56.0%, which points to a company-wide issue.
- **Payment review is a regional problem.** Globally, order statuses are flat (about 53% to 55%). In Central Africa PAYMENT_REVIEW is 80.0% late, and in East Africa 83.3%.
- **Weak departments in Central Africa:** Outdoors (61.3%) and Golf (60.8%).
- **Time patterns are mild.** Peak months: Aug and Sep (55.4%), Dec (55.2%). Lowest: Jul (about 53.7%). Worst hour: 8 PM (57.1%). Day of week varies by about 1.6 points only.

## Machine Learning Model

| Step | Detail |
|---|---|
| Target | `Late_delivery_risk` (1 = late) |
| Features | Scheduled days, order month, order hour, plus frequency-encoded Type, Category, Customer Segment, Department, Order Region, Shipping Mode |
| Split | 80/20 stratified (138,212 train / 34,553 test) |
| Balancing | SMOTE on training set (79,182 per class) |
| Model | Random Forest Classifier |

**Test results**

| Class | Precision | Recall | F1 |
|---|---|---|---|
| 0 (On-time) | 0.68 | 0.73 | 0.70 |
| 1 (Late) | 0.79 | 0.75 | 0.77 |
| **Accuracy** | | | **0.74** |

**Limitation:** scheduled days are fixed per shipping mode (Same Day 0, First 1, Second 2, Standard 4), so shipping mode strongly drives the prediction. Future work below addresses this.

## Dashboard & Visuals

This project uses static analytical charts generated in the notebook.

**Profitability distribution**

![Profitability Distribution](visuals/profitability_distribution.png)

**Delay distribution and profit by delay days**

![Delay Distribution and Profit](visuals/delay_distribution_and_profit_analysis.png)

**Bottleneck detection by category**

![Bottleneck Detection](visuals/bottleneck_detection_by_category.png)

**Top drivers of late delivery, Central Africa**

![Top Drivers Central Africa](visuals/top_drivers_late_delivery_central_africa.png)

**Delay trend by month, day and hour**

![Delay Trend](visuals/delay_trend_month_day_hour.png)

## How to Run This Project

1. **Clone the repository**
   ```bash
   git clone https://github.com/seema-kri/Supply-Chain-Delivery-Delay-Operational-Risk-Analysis.git
   cd Supply-Chain-Delivery-Delay-Operational-Risk-Analysis
   ```
2. **Create a virtual environment (optional)**
   ```bash
   python -m venv venv
   source venv/bin/activate        # Windows: venv\Scripts\activate
   ```
3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```
4. **Check the data path.** The dataset is in `data/`. In the notebook, set the read line to match your file:
   ```python
   df = pd.read_csv('../data/DataCoSupplyChainDataset.csv', encoding='latin-1')
   # or, if you keep the .xls file:
   # df = pd.read_excel('../data/DataCoSupplyChainDataset.xls')
   ```
5. **Open the notebook**
   ```bash
   jupyter notebook notebooks/supply_chain.ipynb
   ```
6. Run all cells from top to bottom. Charts are saved as PNG files.

## Final Recommendations & Future Work

**Recommendations**

1. **Audit First and Second Class shipping (critical).** Promise windows of 1 and 2 days do not match delivered performance. Recalibrate promised dates at checkout or pause First Class until fixed. Study why Same Day works (0% late).
2. **Fix payment review delays in Central and East Africa.** Escalate orders held in review beyond a set time limit.
3. **Plan for peaks.** Reserve carrier capacity before Aug/Sep and Dec, and test an evening dispatch cutoff.
4. **Audit weak departments.** Outdoors and Golf in Central Africa, Health and Beauty and Pet Shop globally.
5. **Cut loss-making orders (18.7%).** Review discounts and shipping costs on low-margin items.
6. **Use the model as an early-warning tool** once leakage is checked, to trigger revised delivery dates and priority packing.

**Future work**

- Remove or test shipping-mode dependence (evaluate on Standard and Second Class only).
- Fit encoders on training data only, and compare Logistic Regression, XGBoost and LightGBM.
- Add feature importance, confusion matrix, ROC-AUC and hyperparameter tuning.
- Predict delay in days (regression), not only late or on-time.
- Build an interactive dashboard (Power BI, Tableau or Streamlit) and deploy the model as an API.
- Add warehouse backlog, weather and carrier tracking features.

## Author & Contact

**Seema Kumari**
Data Analyst | Machine Learning Enthusiast

- GitHub: [@seema-kri](https://github.com/seema-kri)
- LinkedIn: [seema-kumari-375763308](https://linkedin.com/in/seema-kumari-375763308)

---

*If you found this project useful, please give it a star.*
