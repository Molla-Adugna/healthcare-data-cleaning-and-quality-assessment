# healthcare-data-cleaning-and-quality-assessment
Healthcare Data Analytics portfolio project showcasing data cleaning, privacy protection, missing value treatment, categorical standardization and data quality evaluation.  

# Project Goal

Demonstrate the ability to:

  - Clean healthcare datasets
  - Protect patient privacy
  - Handle missing values
  - Remove duplicate records
  - Standardize healthcare variables
  - Evaluate data quality improvements
  - Generate analytical reports

  # Project Structure
    healthcare-data-cleaning-and-quality-assessment/
    │
    ├── data/
    │ ├── raw/
    │ │ └── patient_data.csv
    │ │
    │ └── cleaned/
    │ └── patient_data_cleaned.csv
    │
    ├── notebooks/
    │ └── healthcare_data_cleaning.ipynb
    │
    ├── reports/
    │ ├── before_cleaning_summary.csv
    │ ├── after_cleaning_summary.csv
    │ └── data_quality_report.pdf
    │
    ├── images/
    │ ├── workflow.png
    │ ├── missing_values.png
    │ └── comparison_chart.png
    │
    ├── src/
    │ └── cleaning_pipeline.py
    │
    ├── README.md
    ├── requirements.txt
    └── LICENSE  
# Project Workflow

## Setup
### Installing Required Libraries
The following Libraries are required to run this project. **Pandas** will be used for data manipulation, **NumPy** for numerical operations, **SciPy** for statistical functions and **Scikit-learn** for preprocessing utilities.  
  ```python
# Install the libraries required for this lab
%pip install pandas
%pip install numpy
%pip install scipy
%pip install scikit-learn
```
Example output Screenshot:  
### Suppress Warnings to get a clean output.  
 ```python
# Optional: suppress warnings for cleaner output
def warn(*args, **kwargs):
    pass
import warnings
warnings.warn = warn
warnings.filterwarnings('ignore')
```
Example output Screenshot:  
### Importing Required Libraries  
Import the following libraries for data manipulation, numerical operations, regular expression pattern matching in text processing,statistical functions and preprocessing utilities:
  ```python
import pandas as pd   # For data loading, manipulation, cleaning, and saving (DataFrame operations)
import numpy as np    # For numerical operations, array handling, and missing value operations
import re             # For regular expression pattern matching in text processing

from datetime import datetime, timedelta  # For parsing and manipulating date/time values
import random  # For generating random values during data exploration

from scipy import stats  # For statistical functions like z-score for outlier detection
from sklearn.preprocessing import StandardScaler, MinMaxScaler, OneHotEncoder
# StandardScaler: standardizes features (mean=0, std=1) for ML algorithms
# MinMaxScaler: scales features to a fixed range (typically 0-1)
# OneHotEncoder: converts categorical features into binary indicator variables

print("All libraries imported successfully!")
print("Ready to begin healthcare data preprocessing.")
```
Example output Screenshot:  

  ## Step 1: Load and explore the raw data
  In this step, I load the healthcare dataset and perform the initial exploration to identify data quality issues. This exploration phase helps us make informed decisions about which preprocessing techniques to apply.
  ```python
# Load the raw healthcare data from CSV file
df = pd.read_csv("https://foundations-of-healthcare-data-analytics-4e579d.gitlab.io/labs/Cleaning_and_Validating_Healthcare_Data_Using_Python/raw_data.csv")

# Display the first few rows to get an initial sense of the data
print("First 5 rows of the dataset:")
df.head()
```
Example output Screenshot:  
## Step 2: Understand data quality issues  
Identify systematically the following most common real world healthcare dataset quality issues:  
1. **Missing data**: Important fields left blank or null
2. **Duplicates**: Identical records appearing multiple times
3. **Inconsistent entries**: Same category with different labels (e.g., M vs Male)
4. **Outliers**: Extreme or impossible values (e.g. Age=200, Glucose=500)
  ```python
# Basic dataset structure
print("Dataset dimensions:")
print(f"Number of rows: {df.shape[0]}")
print(f"Number of columns: {df.shape[1]}")
print("\nColumn names and data types:")
print(df.dtypes)
```
Example output Screenshot:  


Check for number and percentage of Missing values per column.
  ```python
# Check for missing values
print("\nMissing values per column:")
print(df.isnull().sum())
print("\nPercentage of missing values:")
print((df.isnull().sum() / len(df) * 100).round(2))
```
Example output Screenshot:  
Check for number of duplicate rows  
  ```python
# Check for duplicate rows
dup_rows = df.duplicated(keep=False)
print(f"\nNumber of duplicate rows: {dup_rows.sum()}")
if dup_rows.any():
    display(df[dup_rows])
```
Example output Screenshot:  
Identify Inconsistent Categorical Entries  
  ```python
# Identify inconsistent categorical entries
print("\nUnique values in Gender column:")
print(df['Gender'].unique())
print("\nUnique values in Ethnicity column:")
print(df['Ethnicity'].unique())
print("\nUnique values in Diagnosis_Code column:")
print(df['Diagnosis_Code'].unique())
```
Example output Screenshot:  
Check for Mixed unities in weight column  
  ```python
# Identify inconsistent categorical entries
print("\nUnique values in Gender column:")
print(df['Gender'].unique())
print("\nUnique values in Ethnicity column:")
print(df['Ethnicity'].unique())
print("\nUnique values in Diagnosis_Code column:")
print(df['Diagnosis_Code'].unique())
```
Example output Screenshot:  
Statistical Summary to Identify Potensial Outliers  
  ```python
# Statistical summary to identify potential outliers
print("\nStatistical summary of numeric columns:")
display(df[['Age', 'Glucose_mg_dL']].describe())
```
Example output Screenshot:  

Check for Date Format Inconsistent  
  ```python
# Check date format inconsistencies
print("\nSample of Diagnosis_Date values (showing mixed formats):")
display(df['Diagnosis_Date'].sample(10, random_state=42))
```
Example output Screenshot:  
Check Class balance for the target variable  
  ```python
# Check class balance for the target variable
print("\nRisk value distribution:")
print(df['Risk'].value_counts())
print("\nRisk percentage distribution:")
print((df['Risk'].value_counts() / len(df) * 100).round(2))
```
Example output Screenshot:  
## Step 3: Detect outliers using statistical methods



