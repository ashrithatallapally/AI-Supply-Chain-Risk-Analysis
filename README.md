# AI-Based Supply Chain Risk Analysis and Prediction

## Project Overview

This project focuses on analyzing supply-chain data and developing a machine learning model to identify potential supply-chain risk.

The project uses exploratory data analysis (EDA) to understand delivery delays, shipping modes, product categories, and disruption patterns. A leakage-aware Logistic Regression model is then developed using selected pre-event business features to estimate the probability of supply-chain risk.

The project was completed as part of the **AICTE | IBM SkillsBuild Data Analytics with AI Internship** conducted through **BharatCares**.

---

## Objectives

The main objectives of this project are:

- Analyze supply-chain order and delivery data.
- Identify patterns in delivery delays.
- Compare delivery performance across shipping modes and product categories.
- Analyze the relationship between disruptions and delivery delays.
- Identify potential factors associated with supply-chain risk.
- Build a machine learning baseline for supply-risk prediction.
- Generate risk probabilities and categorize orders into Low, Medium, and High Risk levels.
- Provide business-oriented recommendations based on the analysis.

---

## Dataset

The project uses the **US Supply Chain Risk Analysis Dataset** available on Kaggle.

Dataset source:

https://www.kaggle.com/datasets/yuanchunhong/us-supply-chain-risk-analysis-dataset

The dataset contains **1,000 supply-chain orders and 24 columns** covering order information, suppliers, product categories, shipping modes, delivery delays, disruption information, supplier reliability, and supply-risk indicators.

The dataset covers the year **2023**.

---

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook / Google Colab
- GitHub

---

## Project Workflow

The project follows the following workflow:

1. Dataset Collection
2. Data Loading
3. Data Inspection
4. Missing-Value and Duplicate Check
5. Data Cleaning
6. Date Conversion
7. Exploratory Data Analysis
8. Observations and Insights
9. Hypothesis Development
10. Target Leakage Check
11. Feature Selection
12. Categorical Encoding
13. Train-Test Split
14. Feature Scaling
15. Logistic Regression
16. Model Evaluation
17. Risk Probability Generation
18. Risk-Level Classification
19. Business Recommendations

---

## Data Cleaning and Preparation

The dataset was inspected for missing values and duplicate records.

There were no duplicate rows.

The main missing values were found in:

- `Disruption_Type`
- `Disruption_Severity`

These missing values represent orders without recorded disruption information.

Date fields were converted into appropriate datetime format for analysis.

---

## Exploratory Data Analysis

Five main visualizations were developed.

### 1. Orders by Product Category

Electronics had the highest number of orders with **210 orders**, while Food and Pharma had **190 orders each**.

The distribution across categories was relatively balanced.

### 2. Average Delay by Shipping Mode

Sea and Rail showed higher average delivery delays than Air and Road.

This indicates that transportation mode is associated with differences in delivery performance.

### 3. Average Delay by Product Category

Textiles had the highest average delay at approximately **2.11 days**, while Food had the lowest at approximately **1.70 days**.

### 4. Average Delay by Disruption Type

Orders with recorded disruptions had substantially higher delivery delays than orders without recorded disruptions.

Weather-related disruptions showed the highest average delay among the disruption categories in the dataset.

### 5. Monthly Average Delivery Delay

Delivery performance varied across the year.

September recorded the highest average delay at approximately **2.46 days**, while February recorded the lowest at approximately **1.56 days**.

---

## Target Leakage Check

Before developing the machine learning model, the relationship between the target variable `Supply_Risk_Flag` and disruption-related variables was examined.

The analysis showed that:

- Orders without recorded disruptions had `Supply_Risk_Flag = 0`.
- Orders with recorded disruption types had `Supply_Risk_Flag = 1`.
- Disruption severity was also directly aligned with the target.
- `Delay_Days` was strongly associated with the target.

Because these variables contain information that is closely tied to the target and may represent information available after or during the disruption event, they were excluded from the prediction features.

This leakage-aware approach was used to make the model more meaningful for pre-event risk estimation.

---

## Machine Learning Approach

A **Logistic Regression** model was developed as a baseline classification model.

### Selected Features

The model uses the following business-relevant pre-event features:

- `Product_Category`
- `Quantity_Ordered`
- `Shipping_Mode`
- `Order_Value_USD`
- `Historical_Disruption_Count`
- `Supplier_Reliability_Score`

Categorical variables were converted using one-hot encoding.

Numerical features were standardized using `StandardScaler`.

The dataset was divided into:

- **80% training data**
- **20% testing data**

Stratified splitting was used to preserve the class distribution.

---

## Model Evaluation

The Logistic Regression baseline achieved the following results on the unseen test set:

| Metric | Result |
|---|---:|
| Accuracy | 49.50% |
| Precision | 50.85% |
| Recall | 58.25% |

### Confusion Matrix

| | Predicted 0 | Predicted 1 |
|---|---:|---:|
| Actual 0 | 39 | 58 |
| Actual 1 | 43 | 60 |

The results indicate that the selected pre-event features have limited predictive power for the target in this dataset.

Therefore, the model should be treated as a **baseline analytical model rather than a production-ready risk prediction system**.

---

## Risk Probability and Risk Levels

The model generates a probability of supply-chain risk for each test order.

Project-defined thresholds were used to categorize the predicted probabilities:

| Risk Probability | Risk Level |
|---|---|
| < 0.45 | Low Risk |
| 0.45 – 0.55 | Medium Risk |
| > 0.55 | High Risk |

### Test Set Risk Distribution

| Risk Level | Orders | Percentage |
|---|---:|---:|
| Low Risk | 9 | 4.5% |
| Medium Risk | 151 | 75.5% |
| High Risk | 40 | 20.0% |

These thresholds were defined specifically for this project and were not externally validated.

---

## Key Findings

1. The distribution of orders across product categories was relatively balanced.
2. Sea and Rail had higher average delivery delays than Air and Road.
3. Delivery delays differed across product categories.
4. Orders with recorded disruptions had substantially higher delays.
5. Monthly delivery performance varied across the year.
6. September recorded the highest average delivery delay.
7. The leakage-aware Logistic Regression model achieved 49.50% accuracy on unseen test data.
8. The selected pre-event features showed limited predictive power for supply-risk classification.

---

## Hypotheses

### Hypothesis 1 – Transportation and Handling

The higher delays observed for Sea and Rail might be related to longer transportation or handling processes associated with these shipping modes.

### Hypothesis 2 – Product-Specific Handling

Differences in category-level delays could possibly be associated with differences in supply-chain handling, transportation, or processing requirements.

### Hypothesis 3 – Monthly Operational Conditions

The higher average delay observed in September might be associated with operational or transportation conditions that are not captured by the current dataset.

These hypotheses require additional operational data for further validation.

---

## Business Recommendations

Based on the analysis:

1. **Review Sea and Rail Operations**  
   Examine schedules, handling time, coordination, and other operational factors associated with these transportation modes.

2. **Monitor Category-Level Delivery Performance**  
   Pay closer attention to Textiles, Pharma, and Electronics because they showed relatively higher average delays.

3. **Track Monthly Delivery Performance**  
   Monitor delivery performance over time and investigate unusually high-delay periods such as September using additional operational data.

---

## Limitations

- The dataset contains only 1,000 orders.
- The dataset covers one calendar year.
- Several disruption-related variables were excluded from modeling because of target leakage concerns.
- The selected pre-event features produced limited predictive performance.
- The risk-level thresholds were project-defined and not externally validated.
- Additional operational and historical features may be required for stronger prediction.
- The analysis identifies associations and patterns and does not establish causal relationships.

---

## Future Scope

Future improvements could include:

- Adding longer-term supplier performance histories.
- Including transportation route and distance information.
- Including carrier and handling-time information.
- Using multi-year supply-chain data.
- Comparing additional machine learning algorithms.
- Calibrating risk thresholds using actual business costs.
- Adding more operational and supplier-level features.
- Improving data-quality validation.

---

## Project Structure

```text
AI-Supply-Chain-Risk-Analysis/
│
├── AI_Supply_Chain_Risk_Analysis.ipynb
├── requirements.txt
├── README.md
└── AI_Supply_Chain_Risk_Analysis_Report.pdf
