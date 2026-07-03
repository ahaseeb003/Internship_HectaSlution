# 📊 Telco Customer Churn — Exploratory Data Analysis

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Pandas](https://img.shields.io/badge/Pandas-EDA-150458.svg)](https://pandas.pydata.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#license)

A complete, industry-standard **Exploratory Data Analysis (EDA)** of the Telco Customer Churn dataset — built as part of an internship project. The notebook walks through **basic → advanced** EDA techniques, with every function explained so it can be used both as a working analysis and as a learning reference.

---

## 📁 Repository Contents

| File | Description |
|---|---|
| `Telco_Churn_EDA.ipynb` | Main notebook — full EDA pipeline, executed end-to-end |
| `WA_Fn-UseC_-Telco-Customer-Churn.csv` | Source dataset (IBM/Kaggle Telco Customer Churn) |
| `telco_churn_eda_report.html` | Auto-generated profiling report (ydata-profiling) |
| `README.md` | Project documentation (this file) |

---

## 📌 About the Project

**EDA (Exploratory Data Analysis)** is the process of examining a dataset — through summary statistics and visualizations — to understand its structure, uncover patterns and relationships, detect anomalies, and validate assumptions **before** any model is trained. This notebook applies that process to customer churn data to surface the factors most associated with a customer leaving a telecom service.

**Dataset:** 7,043 customers × 21 features, including demographics, account information, subscribed services, and the target label `Churn` (Yes/No).

---

## 🧭 EDA Workflow

The notebook is organized into the following stages, each in its own section with explained code:

| # | Stage | What It Covers |
|---|---|---|
| 1 | **Setup & Data Loading** | Importing libraries, configuring display options, loading the dataset |
| 2 | **Basic EDA** | Structure, data types, missing values, duplicates, cardinality |
| 3 | **Data Cleaning & Preprocessing** | Fixing type issues, handling missing values, standardizing categories, encoding |
| 4 | **Univariate Analysis** | Studying the distribution of a **single variable** at a time (histograms, boxplots, value counts, skewness/kurtosis) |
| 5 | **Bivariate Analysis** | Relationship between **one variable and Churn** (groupby churn rates, crosstabs, grouped plots) |
| 6 | **Multivariate Analysis** | Interactions between **multiple variables** (correlation heatmaps, pairplots, pivot tables) |
| 7 | **Advanced EDA** | Statistical significance testing, outlier detection, feature engineering, multicollinearity (VIF) |
| 8 | **Automated EDA Reporting** | One-line profiling report generation for a quick comprehensive overview |

---

## 🔍 Stage Details

### 1. Setup & Data Loading
Imports `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly`, and `scipy.stats`, then loads the raw CSV into a DataFrame — the standard starting point of any data project.

### 2. Basic EDA
Establishes a first understanding of the dataset:
- `.shape`, `.columns`, `.dtypes` — size and structure
- `.info()`, `.describe()` — types, non-null counts, summary statistics
- `.isnull().sum()`, `.duplicated()` — data quality checks
- `.nunique()`, `.value_counts()` — cardinality and category frequencies

### 3. Data Cleaning & Preprocessing
Resolves real data-quality issues found in the raw file:
- Converts `TotalCharges` from text to numeric (`pd.to_numeric(errors='coerce')`)
- Imputes missing values using business logic (new customers → 0 total charges)
- Standardizes redundant categories (e.g. `"No internet service"` → `"No"`)
- Encodes the target variable for numeric analysis

### 4. Univariate Analysis
**Analysis of a single variable at a time**, independent of any other column. Used to understand each feature's own shape and spread:
- Distribution plots (`sns.histplot`) and boxplots for numeric features (`tenure`, `MonthlyCharges`, `TotalCharges`)
- Count plots for categorical features
- Skewness and kurtosis to assess symmetry and tail behavior

### 5. Bivariate Analysis
Compares each variable **against the churn label** to identify potential drivers of churn:
- Churn rate by category (`.groupby().mean()`)
- Contingency tables (`pd.crosstab`)
- Grouped bar charts and KDE density comparisons by churn status

### 6. Multivariate Analysis
Looks at **relationships among multiple variables simultaneously**:
- Correlation matrix and heatmap
- Pairplots colored by churn
- Pivot tables for multi-dimensional aggregation
- Interactive Plotly visualizations

### 7. Advanced EDA
Statistical and modeling-readiness checks used in professional analysis:
- **Chi-square test** — tests association between categorical features and churn
- **Independent t-test** — tests whether numeric features differ significantly by churn
- **IQR & Z-score methods** — outlier detection
- **Feature engineering** — tenure buckets, average monthly spend
- **VIF (Variance Inflation Factor)** — multicollinearity check before modeling
- **One-hot encoding** — prepares features for machine learning

### 8. Automated EDA Reporting
Generates a full HTML profiling report (`ydata-profiling`) summarizing distributions, correlations, missing data, and alerts in one step — a quick-start supplement to the manual analysis above.

---

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn plotly scipy statsmodels ydata-profiling jupyter
```

### Run the Notebook
```bash
jupyter notebook Telco_Churn_EDA.ipynb
```

Ensure `WA_Fn-UseC_-Telco-Customer-Churn.csv` is in the same directory as the notebook, or update the file path in the **Data Loading** section.

---

## 📈 Key Insights

- Overall churn rate is **~26.5%** — a moderately imbalanced target.
- **Month-to-month contracts** churn far more than one/two-year contracts.
- **Low-tenure** (newer) customers churn at a much higher rate.
- **Fiber optic** internet customers churn more than DSL or no-internet customers.
- Customers **without OnlineSecurity or TechSupport** churn more — a potential upsell opportunity.
- `Contract`, `InternetService`, and `PaymentMethod` are all **statistically significant** (p < 0.05, chi-square test) in relation to churn.
- `TotalCharges` and `tenure` are highly correlated, as expected — flagged for multicollinearity in future modeling.

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Language | Python 3.10+ |
| Data Handling | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn, Plotly |
| Statistics | SciPy, Statsmodels |
| Automated Reporting | ydata-profiling |
| Environment | Jupyter Notebook |

---

## 📚 Dataset Source

[Telco Customer Churn — IBM Sample Dataset (Kaggle)](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

---

## 👤 Author

**Abdul Haseeb**
BS Artificial Intelligence, IBADAT International University, Islamabad
Internship EDA Project

---

## 📄 License

This project is intended for educational and internship purposes.
