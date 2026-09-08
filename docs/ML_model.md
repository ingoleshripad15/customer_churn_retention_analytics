Columns of data :   'customerID', 'gender', 'SeniorCitizen', 'Partner', 'Dependents',

&#x20;      'tenure', 'PhoneService', 'MultipleLines', 'InternetService',

&#x20;      'OnlineSecurity', 'OnlineBackup', 'DeviceProtection', 'TechSupport',

&#x20;      'StreamingTV', 'StreamingMovies', 'Contract', 'PaperlessBilling',

&#x20;      'PaymentMethod', 'MonthlyCharges', 'TotalCharges', 'Churn',

&#x20;      'Total\_services', 'churn\_probability', 'predicted\_churn', 'risk\_level'],

&#x20;     dtype='object')





&#x20;                        **CUSTOMER DATA**

&#x20;                             **│**

&#x20;                             **▼**

&#x20;                    **┌─────────────────┐**

&#x20;                    **│ ColumnTransformer│**

&#x20;                    **└────────┬────────┘**

&#x20;                             **│**

&#x20;            **┌────────────────┼────────────────┐**

&#x20;            **│                │                │**

&#x20;            **▼                ▼                ▼**

&#x20;      **Continuous           Binary        Categorical**

&#x20;            **│                │                │**

&#x20;            **▼                ▼                ▼**

&#x20;     **StandardScaler      passthrough    OneHotEncoder**

&#x20;            **│                │                │**

&#x20;            **└────────────────┼────────────────┘**

&#x20;                             **▼**

&#x20;                      **Processed Features**

&#x20;                             **│**

&#x20;                             **▼**

&#x20;                        **ML Algorithm**



TN — True Negative



Customer actually stayed.



Model predicted stayed.



FP — False Positive



Customer actually stayed.



Model predicted churn.



FN — False Negative



Customer actually churned.



Model predicted stayed.



TP — True Positive



Customer actually churned.



Model predicted churn.



For a retention system, False Negatives can be particularly important, because those are customers who were like**ly to churn but the company failed to identify them.**





* **Customer Risk Scoring → Segmentation → Retention Strategy**



Why Logistic Regression First?



You might wonder:



"Why aren't we starting with XGBoost?"



Because we're building an industry-style ML project, not a model leaderboard.



A good Data Scientist usually establishes a baseline model first.



&#x20;                Baseline

&#x20;                   ↓

&#x20;         Logistic Regression

&#x20;                   ↓

&#x20;         Decision Tree

&#x20;                   ↓

&#x20;         Random Forest

&#x20;                   ↓

&#x20;      Gradient Boosting / XGBoost

&#x20;                   ↓

&#x20;           Model Comparison

&#x20;                   ↓

&#x20;         Final Model Selection



But our real business objective isn't:



"Predict as many customers correctly as possible."



It's:



Identify customers who are at risk of churning early enough that the company can take retention action.



Therefore recall, precision, probability ranking and business cost matter.





our aim to predict:

Customer

&#x20;  ↓

Churn Probability

&#x20;  ↓

Risk Score

&#x20;  ↓

Business Threshold

&#x20;  ↓

Risk Segment

&#x20;  ↓

Recommended Retention Action





Our project can say:



"The trained churn model generates customer-level churn probabilities. A business-oriented threshold of 0.40 was selected based on F1 optimization, improving churn recall to 66.31%. Customers are subsequently classified into four risk tiers to support differentiated retention strategies."



**Customer**

&#x20;  **↓**

**Churn probability**

&#x20;  **↓**

**Risk**

&#x20;  **↓**

**Recommended action**





#### **flow of full project :**

&#x20;                C**ustomer Data**

&#x20;                      **↓**

&#x20;                **Data Cleaning**

&#x20;                      **↓**

&#x20;                 **SQL Analysis**

&#x20;                      **↓**

&#x20;                    **EDA**

&#x20;                      **↓**

&#x20;             **Feature Engineering**

&#x20;                      **↓**

&#x20;             **Model Comparison**

&#x20;                      **↓**

&#x20;            **Logistic Regression**

&#x20;                      **↓**

&#x20;            **Threshold = 0.40**

&#x20;                      **↓**

&#x20;               **Churn Probability**

&#x20;                      **↓**

&#x20;                **Risk Segmentation**

&#x20;                      **↓**

&#x20;            **Retention Recommendation**

&#x20;                      **↓**

&#x20;             **Revenue-at-Risk Analysis**

&#x20;                      **↓**

&#x20;                 **Power BI Dashboard**





**✅ Data loading \& cleaning**

**✅ EDA \& SQL/business analysis**

**✅ Feature engineering**

**✅ Customer profiles**

**✅ Train/test split**

**✅ Preprocessing**

**✅ Model comparison**

**✅ Logistic Regression selected**

**✅ Threshold optimization → 0.40**

**✅ Churn probability**

**✅ Risk levels**

**✅ Retention recommendations**

**✅ Revenue at risk**



**customer\_churn\_predictions.csv**

&#x20;            **│**

&#x20;      **┌─────┴─────┐**

&#x20;      **↓           ↓**

&#x20;  **Power BI     Future API**

