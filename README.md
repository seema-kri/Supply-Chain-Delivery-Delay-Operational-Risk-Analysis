# Supply Chain Delivery Delay & Operational Risk Analysis

## Project Overview

An end-to-end supply chain analytics and machine learning project designed to investigate late deliveries, identify operational risk factors, and predict potential fulfillment SLA breaches.

## Business Problem

Late deliveries can affect customer satisfaction, inventory planning, logistics costs, and operational efficiency. This project analyzes order-level data to understand delivery performance and support proactive risk management.

## Key Findings

* Dataset size: 172K+ orders (verify exact row count).
* Late-delivery rate: 54.7% (verify calculation and target definition).
* Objective: identify factors associated with late deliveries and predict fulfillment SLA breaches.

## Project Workflow

1. Data cleaning and preprocessing.
2. Exploratory data analysis (EDA).
3. Delivery delay and operational risk analysis.
4. Feature engineering and preparation.
5. Random Forest classification.
6. SMOTE for training-set class imbalance, where appropriate.
7. Model evaluation and business recommendations.

## Tools & Technologies

* Python
* Pandas and NumPy
* Matplotlib and Seaborn
* Scikit-learn
* Imbalanced-learn
* Jupyter Notebook
* Git and GitHub

## Machine Learning Evaluation

Report the actual test-set results:

* Precision
* Recall
* F1-score
* ROC-AUC, where appropriate
* Confusion matrix
* Baseline versus tuned model performance

## Business Recommendations

Document evidence-based recommendations for prioritizing at-risk orders, monitoring delivery performance, and improving fulfillment operations.

## Repository Structure

* `data/` — dataset documentation and permitted data files
* `notebooks/` — analysis and model development
* `reports/` — findings and recommendations
* `visuals/` — charts and model evaluation figures
* `src/` — reusable Python scripts, if applicable

## How to Run

1. Clone this repository.
2. Create and activate a Python virtual environment.
3. Install dependencies using `pip install -r requirements.txt`.
4. Obtain the dataset according to `data/README.md`.
5. Open the notebooks and run them in numerical order.

## Limitations

Predictions depend on data quality, available features, and the evaluation design. Model performance does not guarantee future delivery outcomes.

## Future Improvements

* Improve early identification of at-risk orders.
* Compare alternative classification models.
* Monitor model performance over time.
* Develop a delivery-risk dashboard.
