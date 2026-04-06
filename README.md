# NHANES Analysis: Dietary Fiber, Blood Glucose, and Lipid Profile

## 📌 Project Overview
This project analyzes the relationship between dietary fiber intake and 
metabolic health markers (blood glucose and lipid profile) using NHANES 
(National Health and Nutrition Examination Survey) data.
The analysis follows rigorous data science methodology including outlier 
detection, missingness diagnosis, and machine learning-based imputation.

## 📊 Research Question
Is higher dietary fiber intake associated with:
- Lower fasting blood glucose levels?
- Improved lipid profile (HDL, Total Cholesterol, Triglycerides)?

## 🗂️ Data Sources
NHANES datasets used:
| File | Contents |
|---|---|
| DEMO_L.xpt | Age, gender, demographics |
| DR1TOT_L.xpt | Dietary fiber intake (24-hour recall) |
| GLU_L.xpt | Fasting blood glucose |
| HDL_L.xpt | HDL cholesterol |
| TCHOL_L.xpt | Total cholesterol |
| TRIGLY_L.xpt | Triglycerides |

👉 Data downloaded from: https://wwwn.cdc.gov/nchs/nhanes/

## ⚙️ Methodology

### 1. Data Preparation
- Loaded SAS XPT files using pyreadstat
- Selected relevant variables from each dataset
- Merged all datasets using participant ID (SEQN)
- Renamed variables for readability

### 2. Data Cleaning
- Restricted analysis to adults aged 20 and older (n=571 adolescents removed)
- Removed clinically implausible values:
  - Fiber intake > 60g/day
  - Fasting glucose > 400 mg/dL
  - Triglycerides > 1000 mg/dL
  - Total cholesterol < 70 mg/dL
- Final clean sample: 3,399 participants

### 3. Missing Data Handling
- Assessed missingness using isnull() and visual inspection
- Created missing indicator flags for each variable
- Diagnosed missingness type using independent samples t-tests:
  - Fiber (p<0.001) and Glucose (p=0.023) → MAR (Missing At Random)
  - HDL, TotalChol, Triglycerides (p>0.05) → MCAR (Missing Completely At Random)
- Applied KNN Imputation (k=5) using sklearn:
  - Variables standardized before imputation
  - Back-transformed to original units after imputation
  - Post-imputation means validated against pre-imputation means
- Final dataset: 3,399 participants with zero missing values

## 🛠️ Tools and Libraries
| Tool | Purpose |
|---|---|
| Python | Core programming language |
| pandas | Data manipulation and merging |
| numpy | Numerical operations |
| pyreadstat | Loading SAS XPT files |
| sklearn | Standardization and KNN imputation |
| scipy | Statistical testing (t-tests) |
| matplotlib/seaborn | Visualization |
| Jupyter Notebook | Analysis environment |


## 📈 Current Progress
- [x] Data loading and merging
- [x] Variable selection and renaming
- [x] Outlier detection and removal
- [x] Missing data diagnosis (MAR/MCAR)
- [x] KNN imputation and validation
- [ ] Exploratory data analysis
- [ ] Statistical modeling
- [ ] Visualization and reporting

## 🔜 Next Steps
- Exploratory data analysis (EDA)
- Distribution plots and correlation heatmaps
- Multiple linear regression — fiber vs each metabolic outcome
- Survey-weighted analysis using NHANES sample weights

## 👩‍💻 Author
**Usha Gartoula**
Masters in Clinical Nutrition | MS Data Science (In Progress)