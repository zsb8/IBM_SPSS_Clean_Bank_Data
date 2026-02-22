# Data Preprocessing and Cleaning via IBM SPSS Modeler

## Project Overview

This project demonstrates a comprehensive data engineering workflow within IBM SPSS Modeler to transform "dirty" bank customer data into a model-ready dataset. The primary focus is on addressing data quality issues including missing values, structural inconsistencies, and statistical outliers.

## Data Quality Audit (Initial State)

A preliminary scan using the Data Audit node revealed several critical issues that would compromise machine learning model performance:

| Issue Type | Description |
|------------|-------------|
| **Missing Values** | 12 nulls in age, 17 in income, and 2 in children |
| **Structural Noise** | Multiple fields contained empty strings and white spaces instead of proper nulls |
| **Invalid Categorical Values** | Inconsistent labeling such as 'F' vs 'FEMALE' and 'YES' vs 'Y' |
| **Outliers** | Impossible values like -18 or 611 for age, and -1 for income |

<img width="961" height="242" alt="image" src="https://github.com/user-attachments/assets/e1a122f9-3ae0-45c2-a968-f5ce9ab5342e" />

These are some outlier data:   
<img width="572" height="390" alt="image" src="https://github.com/user-attachments/assets/c0b2b05e-3918-4934-9b83-2e5b1c6a557e" />

## IBM SPSS Modeler Components Used

The workflow leverages the following specialized nodes and algorithms:

| Component | Purpose |
|-----------|---------|
| **Data Audit Node** | Initial data quality assessment to identify distributions, missing rates, and outliers |
| **Reclassify Node** (Multiple Mode) | Standardize categorical variables by mapping various shorthand or incorrect strings to a unified label |
| **SuperNode** | Container node to encapsulate complex logic for outlier handling and missing value imputation |
| **Type Node** | Define measurement levels (Continuous, Nominal, Flag) and specify "Check" constraints |
| **C&RT Algorithm** | Classification and Regression Trees — decision-tree based method for predicting and filling missing values |
| **Derive Node** (Formula Mode) | Create the `income_norm` field through custom mathematical expressions |
| **Binning Node** | Transform continuous age data into discrete categories using the Fixed-width method |
| **Filter Node** | Remove redundant or original uncleaned fields after reclassification |

## Workflow Implementation

### 1. Categorical Standardization

Invalid labels were corrected via the Reclassify function. For instance, 'F' and 'FEMALE' were consolidated into a single 'FEMALE' category to ensure feature consistency.
<img width="900" height="686" alt="image" src="https://github.com/user-attachments/assets/28578899-089e-42a1-ab2d-79a2dcbc7f08" />

### 2. Outlier & Missing Value Treatment

The project utilizes an **"Identify then Impute"** strategy:

- **Nullification**: Meaningless values (e.g., negative ages or extreme income spikes) were first converted to nulls.
- **Algorithmic Imputation**: The C&RT algorithm was applied to fill these gaps with predicted values, maintaining the statistical integrity of the dataset better than simple mean substitution.

<img width="458" height="325" alt="image" src="https://github.com/user-attachments/assets/f1dc5db8-c005-4d02-8e08-49a9b19e3fd3" />
<img width="390" height="334" alt="image" src="https://github.com/user-attachments/assets/c0e90706-1d9e-40fd-8b14-81ebdc427b84" />

After the above-mentioned data pre-processing steps, we successfully got the clean data with no outliers, no extreme, no null value, no empty string, no white space, and no blank value.   
<img width="954" height="740" alt="image" src="https://github.com/user-attachments/assets/54b424b9-43a6-46e8-b247-1378f4ef6e49" />

### 3. Feature Engineering
<img width="527" height="423" alt="image" src="https://github.com/user-attachments/assets/2f8b38b7-9087-4aaa-8246-697c31633ceb" />

- **Normalization**: Applied Min-Max scaling to income using the formula:
  $$x_n = \frac{X_i - Min}{Max - Min}$$
  to bring values into a [0, 1] range.
<img width="443" height="141" alt="image" src="https://github.com/user-attachments/assets/d6592d76-1718-4b34-8cd6-4887e3f8ba20" />

- **Binning**: The age variable was discretized into 5 distinct bins to reduce noise and handle non-linear relationships.
<img width="535" height="402" alt="image" src="https://github.com/user-attachments/assets/8cd43400-3a6c-446f-92a5-edfcf06bed02" />

## Results
<img width="1040" height="382" alt="image" src="https://github.com/user-attachments/assets/c9320d5a-30d3-498a-803c-97a3f09ed2db" />

The final output, verified by a post-processing Data Audit, achieved a **100% success rate** in cleaning:

- Zero missing values, nulls, or white spaces
- Zero outliers or extreme values
- Standardized flag and nominal measurements across all 11 fields

