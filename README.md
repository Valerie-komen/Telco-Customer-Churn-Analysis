# Telco Customer Churn Analysis and Prediction

**Exploratory analysis and classification modelling on 7,043 telecom customers, to understand what drives churn and whether it can be predicted**

> Interactive dashboard: [dashboard/churn_dashboard.html](dashboard/churn_dashboard.html)

## In short

- **What:** I looked at why customers leave a telecom provider and built a model to identify customers who are likely to leave.
- **Data:** 7,043 customers with 21 fields each (IBM Telco Customer Churn dataset).
- **Tools:** Python (pandas, scikit-learn), Plotly.
- **Main findings:**
  - Customers on month-to-month contracts churn at 42.7%, compared with 2.8% on two-year contracts.
  - Customers with fibre optic internet or who pay by electronic check are the most likely to leave.
  - The model identifies 4 out of 5 customers who actually leave, with a ROC-AUC of 0.845 (where 0.5 is guessing and 1.0 is perfect).

---

## Problem

A telecom provider is losing 26.5% of its customers. Which customers are leaving, why, and can churn be predicted early enough for the business to do something about it?

**Research questions:**
- Which contract terms, services, and payment methods are linked to churn?
- Do newer customers or customers who pay more churn more often?
- Can churn be predicted well enough to be useful in practice? In my [coffee sales project](https://github.com/Valerie-komen/Coffee-Sales-Analytics-ML), a similar prediction problem did not work because the data was too limited, so I wanted to check whether this dataset has enough balance and information to support a model.

---

## Dataset

Source: IBM's Telco Customer Churn dataset. It has 7,043 customers and 21 fields covering demographics, contract and account details, subscribed services, charges, and churn status.

---

## Methods

| Step | What I did | Files |
|---|---|---|
| **Cleaning** | Fixed `TotalCharges`, which was blank for 11 customers with zero tenure. These were new customers, not errors, so I filled them with 0 instead of dropping them. I also merged repeated category labels | [`notebooks/01_data_cleaning.ipynb`](notebooks/01_data_cleaning.ipynb) |
| **Exploratory analysis** | Churn rate by contract, service, and payment method, tenure and spending patterns, and correlations | [`notebooks/02_exploratory_analysis.ipynb`](notebooks/02_exploratory_analysis.ipynb) |
| **Machine learning** | Trained 3 classifiers (Logistic Regression, Random Forest, Gradient Boosting) to predict churn | [`notebooks/03_machine_learning.ipynb`](notebooks/03_machine_learning.ipynb) |
| **Dashboard** | An interactive HTML dashboard | [`dashboard/churn_dashboard.html`](dashboard/churn_dashboard.html) |

---

## Results

- **Contract length is the strongest driver of churn.** Customers on month-to-month contracts churn at 42.7%, compared with 11.3% for one-year contracts and 2.8% for two-year contracts.
- **Customers with fibre optic internet churn at 41.9%, compared with 19.0% for DSL.**
- **Customers who pay by electronic check churn at 45.3%**, more than double any other payment method.
- **Customers who left had been with the company for less than half as long as those who stayed** (18.0 months compared with 37.6 months), and they paid more per month on average ($74.44 compared with $61.27).

![Churn Dashboard](images/dashboard.png)

---

## Machine Learning

In my coffee sales project, only 2.7% of customers were repeat buyers, and no model performed better than simply predicting "no" for everyone. Here, 26.5% of customers churned, which gives the models much more to learn from. I tested this rather than assuming it would work.

| Model | Test Accuracy | Precision (Churn) | Recall (Churn) | CV ROC-AUC (5-fold) |
|---|---|---|---|---|
| **Logistic Regression** | 0.750 | 0.52 | **0.80** | **0.845 (±0.014)** |
| Random Forest | 0.766 | 0.54 | — | 0.844 (±0.011) |
| Gradient Boosting | 0.796 | 0.65 | — | 0.840 (±0.010) |

Logistic Regression had the best cross-validated ROC-AUC. It is also easier to interpret than the tree models, because each feature has a coefficient that shows whether it increases or reduces churn. The coefficients agree with the exploratory analysis: one-year and two-year contracts reduce churn the most, while fibre optic internet and electronic check payment are among the features that increase it most.

The model has a recall of 80% for churn, which means it identifies 4 out of 5 customers who actually leave. For a retention campaign, this is a reasonable trade-off, because missing a customer who is about to leave usually costs more than sending a retention offer to someone who would have stayed.

![Feature Importance](images/feature_importance.png)
![ROC Curve](images/roc_curve.png)

---

## Installation

```bash
git clone https://github.com/Valerie-komen/Telco-Customer-Churn-Analysis.git
cd Telco-Customer-Churn-Analysis
pip install -r requirements.txt
```

## Reproducibility

1. **Data cleaning:** [`notebooks/01_data_cleaning.ipynb`](notebooks/01_data_cleaning.ipynb) → `data/cleaned_telco_churn.csv`.
2. **Exploratory analysis:** [`notebooks/02_exploratory_analysis.ipynb`](notebooks/02_exploratory_analysis.ipynb).
3. **Machine learning:** [`notebooks/03_machine_learning.ipynb`](notebooks/03_machine_learning.ipynb).
4. **Dashboard:** open [`dashboard/churn_dashboard.html`](dashboard/churn_dashboard.html) in a browser.

All notebook outputs in this repository are from real runs of the code.

---

## Repository Structure

```
Telco-Customer-Churn-Analysis/
├── README.md
├── requirements.txt
├── data/
│   ├── raw_telco_churn.csv
│   └── cleaned_telco_churn.csv
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_exploratory_analysis.ipynb
│   └── 03_machine_learning.ipynb
├── dashboard/
│   └── churn_dashboard.html
└── images/
```

---

## Limitations

- This is a well-known public dataset (IBM Telco Customer Churn), not data from a real company. The findings are useful for showing the method, but they are not recommendations for any specific business.
- There is no data on customer support interactions, competitor pricing, or service quality. These probably explain *why* fibre optic and electronic check customers churn more, which this dataset cannot answer.
- The model is trained on a single point in time. If it were used in practice, it would need to be monitored and updated as contracts, pricing, and competition change.

---

*If you have questions or feedback, feel free to open an issue or connect with me on [LinkedIn](https://www.linkedin.com/in/valerie-komen-468a93355/).*
