# Customer Churn Prediction & Retention Analytics

An end-to-end customer churn analytics project combining Python-based machine learning, customer risk segmentation, revenue-at-risk analysis, retention recommendations, and an interactive Power BI report. The goal is to help a telecom business identify customers who may leave and prioritize retention efforts using data-driven insights.

## Business Problem

Customer churn affects recurring revenue and increases the cost of acquiring replacement customers. Historical reporting can describe who has left, but a retention team also needs a way to identify current customers who may be at risk and decide which customers to contact first.

This project analyzes telecom customer data, trains classification models to estimate churn probability, converts those probabilities into actionable risk groups, and presents business insights and retention actions in Power BI.

## Project Objectives

- Clean and prepare customer data for analysis.
- Explore churn patterns across contracts, tenure, services, payment methods, and charges.
- Compare multiple machine-learning classification models.
- Select a classification threshold aligned with a customer-retention use case.
- Segment customers by estimated churn risk.
- Provide rule-based retention recommendations for prioritizing outreach.
- Present business and model outputs through a three-page Power BI report.

## Dataset

The project uses the **Telco Customer Churn** dataset, which contains customer demographics, subscribed services, account information, charges, tenure, and the historical churn outcome.

- Original records: 7,043
- Records after cleaning: 7,032
- Target column: `Churn`
- Target distribution after cleaning: 1,869 churned customers and 5,163 retained customers
- Historical churn rate: approximately **26.58%**

The original dataset is not included in this repository. Obtain it from the original dataset provider and place it in `data/raw/` before reproducing the workflow. Check the provider's current terms before redistributing any source data.

## Technology Stack

- **Python:** data preparation, exploratory analysis, feature engineering, and model evaluation
- **Pandas, NumPy:** data manipulation
- **Matplotlib, Seaborn:** exploratory visualizations
- **Scikit-learn:** preprocessing pipelines, model training, and evaluation
- **XGBoost:** challenger model comparison
- **Jupyter Notebook:** analysis workflow and experiments
- **SQL / relational-data concepts:** analytical thinking about customer data and business metrics
- **Power BI:** interactive reporting and business-facing visualization
- **Git and GitHub:** version control and project sharing

## Project Workflow

1. **Data understanding** — inspect the dataset, column types, target distribution, and data quality.
2. **Data cleaning** — convert `TotalCharges` to numeric, handle blank values, and remove 11 records that could not be used after conversion.
3. **Exploratory data analysis** — investigate churn patterns by contract, tenure, service adoption, payment method, and monthly charges.
4. **Feature preparation** — separate features from the target, split training and test data, and use a preprocessing pipeline for numeric and categorical variables.
5. **Model comparison** — evaluate Logistic Regression, Decision Tree, Random Forest, and XGBoost.
6. **Threshold selection** — assess the precision/recall trade-off and select a 0.40 probability threshold for the retention-focused use case.
7. **Risk segmentation** — convert estimated churn probabilities into Low, Medium, High, and Very High risk groups.
8. **Retention recommendations** — assign rule-based actions using risk, contract, tenure, charges, and service information.
9. **Power BI reporting** — communicate overall churn, segment-level patterns, risk distribution, revenue exposure, and customer-level actions.

## Machine-Learning Results

The models were evaluated on a held-out test set. Results below use the standard 0.50 classification threshold for the initial model comparison; the selected Logistic Regression model was subsequently evaluated at a 0.40 threshold.

| Model | Accuracy | Precision | Recall | F1-score | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|---:|
| Logistic Regression | 0.8038 | 0.6476 | 0.5749 | 0.6091 | 0.8358 | 0.6226 |
| Decision Tree | 0.7896 | 0.6021 | 0.6150 | 0.6085 | 0.8296 | 0.6059 |
| Random Forest | 0.7861 | 0.6254 | 0.4866 | 0.5474 | 0.8143 | 0.5888 |
| XGBoost | 0.7925 | 0.6314 | 0.5267 | 0.5743 | 0.8341 | 0.6503 |

### Selected model: Logistic Regression

Logistic Regression was selected as the primary model because it achieved the strongest accuracy, precision, F1-score, and ROC-AUC among the evaluated models. XGBoost achieved the highest PR-AUC, while the Decision Tree had slightly higher recall at the default threshold. These trade-offs were considered rather than choosing a model on accuracy alone.

### Threshold optimization

A **0.40 threshold** was selected for the retention workflow to identify more potential churners, accepting that this can also increase false positives.

| Metric | Threshold 0.50 | Threshold 0.40 |
|---|---:|---:|
| Precision | 0.6476 | 0.5808 |
| Recall | 0.5749 | 0.6631 |
| F1-score | 0.6091 | 0.6192 |

This is a business trade-off, not a universally optimal threshold. A real deployment should validate the threshold against retention-team capacity, campaign cost, and the measured value of successfully retained customers.

## Customer Risk Segmentation

Predicted churn probabilities are grouped into four business-readable risk bands:

| Risk level | Probability band |
|---|---|
| Low | Below 0.20 |
| Medium | 0.20 to below 0.40 |
| High | 0.40 to below 0.60 |
| Very High | 0.60 or above |

The analyzed customer population was segmented as follows:

| Risk level | Customers | Historical churn rate within group |
|---|---:|---:|
| Low | 3,585 | 6.61% |
| Medium | 1,334 | 28.49% |
| High | 1,062 | 47.46% |
| Very High | 1,051 | 71.17% |

The High and Very High groups contain **2,113 customers**. These groups can be used to prioritize retention outreach, but risk scores are estimates and do not guarantee that an individual customer will churn.

## Retention Recommendations

The project generates rule-based recommendations to help turn risk scores into possible next steps. Examples include:

- **Very High risk:** prioritize a retention call, consider a relevant contract incentive, review high charges, or offer an appropriate support service.
- **High risk:** consider an annual-contract incentive, support bundle, online-security offer, or targeted communication.
- **Medium risk:** focus on early-tenure engagement, high-charge customer engagement, or personalized communication.
- **Low risk:** continue standard customer engagement.

These are illustrative decision rules, not experimentally proven interventions. In a business setting, offers should be checked for eligibility and margin impact, and their effectiveness should be measured through controlled experiments where practical.

## Power BI Report

The Power BI report is organized into three pages:

### 1. Executive Overview
Provides a high-level view of customer count, historical churn, churn rate, risk distribution, monthly charges, and monthly charges associated with high-risk customers.

### 2. Customer & Churn Analysis
Explores churn across contract type, internet service, payment method, number of subscribed services, and average monthly charges by churn status.

### 3. Churn Risk & Retention Strategy
Connects model-generated risk groups to revenue exposure and retention actions, including a customer-level action list for follow-up.

The report file is located at `dashboard/customer_churn_dashboard.pbix`. Screenshots are available in `dashboard/Screenshots/`.

**Metric definitions:** `Churn` represents the historical outcome in the dataset. `churn_probability` is the model-estimated probability. `predicted_churn` is the classification made using the selected threshold. `risk_level` is a separate, human-readable probability band. “Revenue at risk” in this project means the sum of monthly charges associated with High and Very High risk customers; it is an exposure indicator, not a forecast of guaranteed revenue loss.

## Key Business Insights

- Approximately **26.58%** of customers in the cleaned dataset had churned.
- Customers who churned had higher average monthly charges (approximately **74.44**) than customers who stayed (approximately **61.31**).
- Churn rates varied substantially by service adoption: customers with one service had a churn rate of approximately **45.76%**, compared with approximately **5.28%** among customers with six services.
- The Very High risk group had a historical churn rate of approximately **71.17%**, compared with approximately **6.61%** in the Low risk group.
- Lowering the classification threshold from 0.50 to 0.40 increased recall from approximately **57.49% to 66.31%**, supporting a retention workflow that places value on identifying more potential churners.

These findings describe associations in this dataset. They do not, by themselves, establish that any particular service, contract, or charge causes churn.

## Repository Structure

```text
customer_churn_project/
├── dashboard/
│   ├── customer_churn_dashboard.pbix
│   └── Screenshots/
├── data/
│   ├── raw/                 # Source data; not included in this repository
│   └── processed/
│       ├── customer_churn_clean.csv
│       └── customer_churn_predictions.csv
├── docs/
│   ├── data interpretation and observation files
│   ├── data dictionary
│   └── ML model documentation
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── eda.ipynb
│   └── ML_model.ipynb
├── src/
│   └── preprocessing.py
├── Project_Documentation.docx
├── README.md
├── requirements.txt
└── .gitignore
```

Some filenames in the current repository may differ from the cleaned names shown in this outline. Update the outline if you rename or remove files.

## Reproducing the Project

1. Clone the repository:

   ```bash
   git clone https://github.com/ingoleshripad15/customer_churn_retention_analytics.git
   cd customer_churn_retention_analytics
   ```

2. Create and activate a virtual environment (recommended):

   **Windows**
   ```bash
   python -m venv .venv
   .venv\Scripts\activate
   ```

   **macOS/Linux**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Obtain the Telco Customer Churn dataset from its original provider and place the CSV in `data/raw/`. Follow the notebook's expected input filename/path, adjusting it if needed.

5. Open the notebooks in `notebooks/` and run the data-understanding, EDA, and modeling workflow in order. Confirm any file paths in the notebooks match your local directory.

6. Open `dashboard/customer_churn_dashboard.pbix` in Power BI Desktop to explore the report. If you regenerate the processed prediction CSV, confirm the column names and data types expected by the report.

> Reproduction note: exact results can vary with library versions, preprocessing changes, random seeds, or dataset revisions. The notebooks are the primary record of the implemented workflow.

## Limitations and Future Improvements

- Validate the model on newer customer data and monitor for data drift.
- Evaluate calibration and business-specific threshold choices.
- Test retention interventions and quantify incremental retention and profitability.
- Add automated tests, a reproducible training script, and model/version tracking.
- Add scheduled scoring and controlled report delivery if moving toward operational use.
- Review privacy, access control, data retention, and customer-contact policies before using real customer data.

## Disclaimer

This is a portfolio and learning project using a public telecom churn dataset. Predictions and retention recommendations are analytical outputs, not guarantees of future behavior or revenue recovery.
