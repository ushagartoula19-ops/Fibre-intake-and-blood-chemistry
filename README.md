# NHANES Analysis: Effect of Dietary Fiber on Metabolic Health Markers

## 📌 Project Overview
This project analyzes the relationship between dietary fiber intake and 
metabolic health markers using NHANES (National Health and Nutrition 
Examination Survey) data — a nationally representative survey of the 
US adult population. The analysis follows rigorous data science 
methodology including clinical outlier detection, statistical 
missingness diagnosis, machine learning based imputation, exploratory 
data analysis, and multiple linear regression modeling.

## 📊 Research Question
Is higher dietary fiber intake independently associated with:
- Lower fasting blood glucose levels?
- Higher HDL cholesterol?
- Lower Total Cholesterol?
- Lower Triglycerides?
- Lower LDL Cholesterol?

## 🗂️ Data Sources
| File | Contents |
|---|---|
| DEMO_L.xpt | Age, gender, demographics |
| DR1TOT_L.xpt | Dietary fiber intake (24-hour dietary recall) |
| GLU_L.xpt | Fasting blood glucose |
| HDL_L.xpt | HDL cholesterol |
| TCHOL_L.xpt | Total cholesterol |
| TRIGLY_L.xpt | Triglycerides and calculated LDL cholesterol |

👉 Data downloaded from: https://wwwn.cdc.gov/nchs/nhanes/

**Note:** LDL cholesterol (LBDLDL) in NHANES is calculated using 
the Friedewald equation: LDL = TotalChol - HDL - (Triglycerides/5) 
and is not directly measured from blood.

## ⚙️ Methodology

### 1. Data Preparation
- Loaded SAS XPT files using pyreadstat
- Selected relevant variables from each dataset
- Merged all six datasets using participant ID (SEQN)
- Renamed variables for readability

### 2. Data Cleaning
Restricted analysis to adults aged 20 and older.
Removed clinically implausible values based on clinical judgment 
and individual inspection:

| Variable | Criterion | Reason |
|---|---|---|
| Age | < 20 years | Adolescent reference ranges differ from adults |
| Fiber | > 60g/day | Implausible 24-hour dietary recall value |
| Glucose | > 400 mg/dL | Extreme values distort regression |
| Triglycerides | > 1000 mg/dL | Severe hypertriglyceridemia dominates analysis |
| Total Cholesterol | < 70 mg/dL | Likely data errors (values of 62 and 63 identified) |
| HDL | > 120 mg/dL | Physiologically unusual |
| LDL | < 20 or > 300 mg/dL | Implausible or extreme values |

**Final clean sample: 3,388 adult participants**

### 3. Missing Data Handling
- Created missing indicator flags for all variables
- Diagnosed missingness type using independent samples t-tests

| Variable | P-value | Classification |
|---|---|---|
| Fiber | <0.001 | MAR — related to age |
| Glucose | 0.024 | MAR — related to age |
| HDL | 0.202 | MCAR |
| TotalChol | 0.194 | MCAR |
| Triglycerides | 0.138 | MCAR |
| LDL | 0.128 | MCAR |

- Applied KNN Imputation (k=5) consistently across all variables
- Variables standardized before imputation and back-transformed afterward
- Floating point precision errors corrected after inverse scaling
- All values rounded to 1 decimal place
- Post-imputation means validated — virtually identical to pre-imputation means
- **Final dataset: 3,388 participants with zero missing values**

### 4. Exploratory Data Analysis
- Descriptive statistics and distribution plots for all variables
- Correlation heatmap — fiber showed near-zero correlations with 
  all outcomes confirming confounding by age and gender
- Linearity check using LOWESS smooth curves — heteroscedasticity 
  detected across all outcomes
- Gender stratified analysis — t-tests confirmed significant 
  differences across all six variables (p < 0.05)
- Age stratified analysis — ANOVA confirmed significant differences 
  across three age groups (p < 0.001)

### 5. Regression Modeling
Five multiple linear regression models were built controlling 
for age and gender as confounders:

| Model | Outcome | Fiber Coefficient | P-value | Significant |
|---|---|---|---|---|
| 1 | Fasting Glucose | -0.1475 mg/dL per gram | 0.016 | ✅ Yes |
| 2 | HDL Cholesterol | +0.0817 mg/dL per gram | 0.003 | ✅ Yes |
| 3 | Total Cholesterol | +0.2549 mg/dL per gram | 0.002 | ✅ Yes |
| 4 | Triglycerides (log) | -0.0017 log units per gram | 0.084 | ❌ No |
| 5 | LDL Cholesterol | +0.1913 mg/dL per gram | 0.007 | ✅ Yes* |

*LDL finding interpreted cautiously — LDL is mathematically 
derived from TotalChol and HDL using the Friedewald equation.

## 🔑 Key Findings
Despite near-zero bivariate correlations between fiber and 
metabolic outcomes — which suggested no relationship — multiple 
regression controlling for age and gender revealed:

- **Fiber significantly reduces fasting glucose** — consistent 
  with published meta-analytic evidence
- **Fiber significantly increases HDL** — consistent with 
  established biological mechanisms
- **Fiber showed no significant association with triglycerides** 
  — consistent with published literature
- **Age and gender are strong confounders** — confirming that 
  simple correlation analysis alone is insufficient for 
  dietary epidemiology research

## ⚠️ Limitations
- Cross-sectional design — causality cannot be established
- No medication data — statin and diabetes medication use 
  could not be controlled for
- Single 24-hour dietary recall — may not reflect habitual 
  fiber intake
- LDL is calculated not directly measured — mathematical 
  dependency with other lipid variables
- Heteroscedasticity detected in linearity checks — mild 
  violation of regression assumptions
- Glucose analyzed without log transformation — skewness 
  driven by diabetic subpopulation noted as limitation

## 🛠️ Tools and Libraries
| Tool | Purpose |
|---|---|
| Python | Core programming language |
| pandas | Data manipulation and merging |
| numpy | Numerical operations |
| pyreadstat | Loading SAS XPT files |
| sklearn | StandardScaler and KNNImputer |
| scipy | Statistical testing (t-tests, ANOVA) |
| statsmodels | Multiple linear regression (OLS) |
| matplotlib / seaborn | Visualization |
| Jupyter Notebook | Analysis environment |
| VS Code | Development environment |
| GitHub | Version control and portfolio |

## 📁 Repository Structure
├── data/
│   └── nhanes_clean_imputed.csv    # Final cleaned dataset
├── NHANES_DATA_ANALYSIS.ipynb      # Main analysis notebook
├── distributions.png               # Distribution plots
├── correlation_heatmap.png         # Correlation heatmap
├── gender_comparison.png           # Gender stratified boxplots
├── age_comparison.png              # Age stratified boxplots
├── linearity_check.png             # Linearity check plots
└── README.md
## ✅ Project Progress
- [x] Data loading and merging
- [x] Variable selection and renaming
- [x] Clinical outlier detection and removal
- [x] Missing data diagnosis (MAR/MCAR)
- [x] KNN imputation and validation
- [x] Descriptive statistics
- [x] Distribution plots
- [x] Correlation heatmap
- [x] Linearity check
- [x] Gender stratified analysis with t-tests
- [x] Age stratified analysis with ANOVA
- [x] Five multiple linear regression models
- [x] Literature comparison

## 👩‍💻 Author
**Usha Gartoula**
Masters in Clinical Nutrition | MS Data Science (In Progress)
GitHub: https://github.com/ushagartoula19-ops