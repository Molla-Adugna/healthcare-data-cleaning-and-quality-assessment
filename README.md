# 🏥 healthcare-data-cleaning-and-quality-assessment
Healthcare Data Analytics portfolio project showcasing data cleaning, privacy protection, missing value treatment, categorical standardization and data quality evaluation.  

## 🎯 Project Goal

**_Demonstrate the ability to:_** 

## ⚙️ Set-up Working Environment:

     1. Install required libraries
     2. Importing required libraries


## 📝  Steps to follow – Starting from loading and cleaning to saving the cleaned datasets:

    Step 1. Load and explore the raw data  

    Step 2. Understand data quality issues  

    Step 3. Detect outliers using statistical methods  
            i. Interquartile Range (IQR) method 
            ii. Z-Score method

    Step 4. Create a clean copy and remove PII  

    Step 5. Remove duplicate rows  

    Step 6. Standardize inconsistent categorical variables  

    Step 7. Normalize mixed units and engineer BMI feature  

    Step 8. Handle missing values with imputation  

    Step 9. Parse dates and engineering temporal features  

    Step 10. Encode categorical variables  

    Step 11. Scale numeric Features  

    Step 12. Save the cleaned dataset  

    Step 13. Evaluate data cleaning results  


  ## 📂 Project Structure
     📦 Healthcare-Data-Cleaning-Pipeline
     │
     ├── 📂 datasets
     │ ├── raw_data.csv
     │ └── cleaned_data.csv
     │
     ├── 📂 notebook
     │ └── healthcare_data_cleaning.ipynb
     │
     ├── 📂 reports
     │ ├── data_quality_report.pdf
     │ └── cleaning_summary.pdf
     │
     ├── 📂 images
     │ ├── workflow.png
     │ ├── outlier_detection.png
     │ └── missing_value_analysis.png
     │
     ├── requirements.txt
     └── README.md 
 
## 🔄 Project Workflow

## ⚙️ Set-up Working Environment
### Installing Required Libraries
The following Libraries are required to run this project. **Pandas** will be used for data manipulation, **NumPy** for numerical operations, **SciPy** for statistical functions and **Scikit-learn** for preprocessing utilities.  
  ```python
# Install the libraries required for this lab
%pip install pandas
%pip install numpy
%pip install scipy
%pip install scikit-learn
```
**_📦 Installation Output:_**  

![Install the required libraries](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Install%20the%20libraries%20required%20for%20this%20project.png)
### Suppress Warnings to get a clean output.  
 ```python
# Optional: suppress warnings for cleaner output
def warn(*args, **kwargs):
    pass
import warnings
warnings.warn = warn
warnings.filterwarnings('ignore')
```
**_✅ Expected Output:_**  

![Supress Warnings](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Supress%20warnings.png)
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
**_💻 Execution Results:_**  

![Importing required libraries](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Importing%20required%20libraries.png)
  ## Step 1: Load and explore the raw data
  In this step, I load the healthcare dataset and perform the initial exploration to identify data quality issues. This exploration phase helps us make informed decisions about which preprocessing techniques to apply.
  ```python
# Load the raw healthcare data from CSV file
df = pd.read_csv("https://foundations-of-healthcare-data-analytics-4e579d.gitlab.io/labs/Cleaning_and_Validating_Healthcare_Data_Using_Python/raw_data.csv")

# Display the first few rows to get an initial sense of the data
print("First 5 rows of the dataset:")
df.head()
```
**_🔍 Results Snapshot:_**  

![First 5 rows of the dataset](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/First%205%20rows%20of%20the%20dataset.png) 
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
**_📸 Output Preview:_**  

![Understand data quality issues](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Understand%20data%20quality%20issues.png) 


### Check for number and percentage of Missing values per column.
  ```python
# Check for missing values
print("\nMissing values per column:")
print(df.isnull().sum())
print("\nPercentage of missing values:")
print((df.isnull().sum() / len(df) * 100).round(2))
```
**_✅ Expected Output:_**  

![Check for number and percentage of missing values per column](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Check%20for%20number%20and%20percentage%20of%20Missing%20values%20per%20column.png)  
### Check for duplicate rows.
  ```python
# Check for duplicate rows
dup_rows = df.duplicated(keep=False)
print(f"\nNumber of duplicate rows: {dup_rows.sum()}")
if dup_rows.any():
    display(df[dup_rows])
```
**_💻 Execution Results:_**  

![Check for number of duplicated rows](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/check%20for%20number%20of%20duplicate%20rows.png)  

### Identify Inconsistent Categorical Entries  
  ```python
# Identify inconsistent categorical entries
print("\nUnique values in Gender column:")
print(df['Gender'].unique())
print("\nUnique values in Ethnicity column:")
print(df['Ethnicity'].unique())
print("\nUnique values in Diagnosis_Code column:")
print(df['Diagnosis_Code'].unique())
```
**_📸 Output Preview:_**  

![Identify inconsistent categorical entries](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Identify%20inconsistent%20categorical%20entries.png)  
### Check for Mixed unities in weight column  
  ```python
# Check for mixed units in Weight column
print("\nUnique Weight values (showing mixed units):")
display(df['Weight'].unique())
```
**_📸 Output Preview:_**  

![Check for mixed units in weight](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Check%20for%20mixed%20units%20in%20weight%20column.png)  

### Statistical Summary to Identify Potensial Outliers  
  ```python
# Statistical summary to identify potential outliers
print("\nStatistical summary of numeric columns:")
display(df[['Age', 'Glucose_mg_dL']].describe())
```
**_📸 Output Preview:_**  

![Statistical summary for potential outliers](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Statistical%20Summary%20for%20potensial%20outliers.png)  

### Check for Date Format Inconsistent  
  ```python
# Check date format inconsistencies
print("\nSample of Diagnosis_Date values (showing mixed formats):")
display(df['Diagnosis_Date'].sample(10, random_state=42))
```
**_📸 Output Preview:_**  

![Check for format inconsistancies](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Check%20for%20date%20format%20inconsistent.png)   
### Check Class balance for the target variable  
  ```python
# Check class balance for the target variable
print("\nRisk value distribution:")
print(df['Risk'].value_counts())
print("\nRisk percentage distribution:")
print((df['Risk'].value_counts() / len(df) * 100).round(2))
```
**_📸 Output Preview:_**  

![Check for class balance for the target value](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Check%20for%20class%20balance%20for%20the%20target%20variables.png)  
## Step 3: Detect outliers using statistical methods  
I used the two most common statistical methods to detect outliers:  
### Interquartile Range (IQR) method
- **Formula**: IQR = Q3 − Q1 (difference between 75th and 25th percentiles)
- **Outlier definition**: Values below Q1 − 1.5 × IQR or above Q3 + 1.5 × IQR
- **Best for**: Non-normally distributed data (robust against skewness)

### Z-Score method
- **Formula**: Z = (Value − Mean) / Standard Deviation
- **Outlier definition**: |Z-score| > 3 (more than 3 standard deviations from mean)
- **Best for**: Normally distributed data
 ```python
# Function to detect outliers using IQR method
def iqr_outliers(series):
    """
    Detect outliers using the Interquartile Range (IQR) method.

    Parameters:
    series: pandas Series - numeric column to check for outliers

    Returns:
    pandas Series - containing only the outlier values
    """
    q1 = series.quantile(0.25)  # 25th percentile
    q3 = series.quantile(0.75)  # 75th percentile
    iqr = q3 - q1                # Interquartile range
    lower_bound = q1 - 1.5 * iqr
    upper_bound = q3 + 1.5 * iqr
    return series[(series < lower_bound) | (series > upper_bound)]

# Detect outliers in Age
print("Age outliers (IQR method):")
age_outliers = iqr_outliers(df['Age'].dropna())
print(f"Found {len(age_outliers)} outliers in Age")
print(f"Outlier values: {age_outliers.unique()}")


# Detect outliers in Glucose
print("\nGlucose outliers (IQR method):")
glucose_outliers = iqr_outliers(df['Glucose_mg_dL'].dropna())
print(f"Found {len(glucose_outliers)} outliers in Glucose_mg_dL")
if len(glucose_outliers) > 0:
    print(f"Outlier values: {glucose_outliers.unique()}")
```
**_📸 Output Preview:_**  

![Detect Outliers using statistical methods](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Detect%20outliers%20using%20statistical%20methods.png)  
## Step 4: Create a clean copy and remove PII

Privacy protection is paramount in healthcare data. **Personally Identifiable Information (PII)** includes any data that can directly or indirectly identify an individual. Common PII in healthcare includes:

- **Direct identifiers**: Patient names, email addresses, phone numbers, addresses
- **Semi-identifiers**: Patient IDs (can be kept if properly anonymized)
- **Sensitive dates**: Birth dates, exact diagnosis dates (often generalized)

Regulations like **HIPAA** (USA) and **GDPR** (Europe) require removing or anonymizing PII before data analysis or sharing.

I'll create a copy of the original data (to preserve the raw data) and remove PII columns.  
  ```python
# Create a working copy - keep original data untouched for reference
df_clean = df.copy()
print("Working copy created. Original data preserved.")
```
**_📸 Output Preview:_**  

![Create a clean copy and remove PII](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Create%20a%20clean%20copy%20and%20remove%20PII.png)  
  ```python
# Identify and remove PII columns
pii_columns = ['Patient_Name', 'EmailID']
print(f"Removing PII columns: {pii_columns}")

df_clean = df_clean.drop(columns=pii_columns, errors='ignore')

print("\nColumns after removing PII:")
print(df_clean.columns.tolist())
print(f"\nReduced from {len(df.columns)} to {len(df_clean.columns)} columns")
```
**_📸 Output Preview:_**  

![Identify and remove PII](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Identify%20and%20remove%20PII%20columns.png)  
## Step 5: Remove duplicate rows

Duplicate records can occur due to data entry errors, system glitches, or merging datasets. They can:
- Bias analysis by overrepresenting certain patients
- Inflate dataset size artificially
- Cause data leakage in train-test splits

I'll identify and remove exact duplicate rows, keeping only the first occurrence.  
  ```python
# Count duplicates before removal
duplicates_before = df_clean.duplicated().sum()
rows_before = len(df_clean)

# Remove exact duplicate rows (keep first occurrence)
df_clean = df_clean.drop_duplicates(keep='first')

# Report results
rows_after = len(df_clean)
print(f"Rows before deduplication: {rows_before}")
print(f"Duplicate rows found: {duplicates_before}")
print(f"Rows after deduplication: {rows_after}")
print(f"Rows removed: {rows_before - rows_after}")
```
**_📸 Output Preview:_**  

![Remove duplicate rows](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Remove%20duplicate%20rows.png)  
## Step 6: Standardize inconsistent categorical variables

Inconsistent categorical data is common in healthcare due to:
- Multiple data entry personnel with different conventions
- Data merging from different systems
- Typos and abbreviations

You need to standardize:
- **Gender**: Convert M/Male/m → 'Male', F/Female/f → 'Female'
- **Ethnicity**: Standardize capitalization (asian/Asian/ASIAN → 'Asian')
- **Diagnosis_Code**: Fix typos and standardize (OCD/ocd → 'OCD', ANXITY → 'ANX')

This ensures categorical data is **clean, consistent, and machine-readable**.  

  ```python
# Standardize Gender column
print("Before standardization - Gender unique values:")
print(df_clean['Gender'].value_counts(dropna=False))

# Convert to lowercase and strip whitespace for consistent matching
df_clean['Gender'] = df_clean['Gender'].astype(str).str.strip().str.lower()

# Define mapping for known variations
gender_map = {
    'male': 'Male', 'm': 'Male',
    'female': 'Female', 'f': 'Female',
    'other': 'Other',
    'nan': np.nan, 'none': np.nan
}

# Apply mapping
df_clean['Gender'] = df_clean['Gender'].replace({'nan': np.nan})
df_clean['Gender'] = df_clean['Gender'].map(
    lambda x: gender_map.get(x, x.capitalize() if pd.notna(x) else x)
)

print("\nAfter standardization - Gender unique values:")
print(df_clean['Gender'].value_counts(dropna=False))
```
**_📸 Output Preview:_**  

![Standardize gender column](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/standardize%20%20Gender%20Column.png)  

  ```python
# Standardize Ethnicity column
print("Before standardization - Ethnicity unique values:")
print(df_clean['Ethnicity'].value_counts(dropna=False))

# Standardize capitalization
df_clean['Ethnicity'] = df_clean['Ethnicity'].astype(str).str.strip()
df_clean['Ethnicity'] = df_clean['Ethnicity'].replace({'nan': np.nan})
df_clean['Ethnicity'] = df_clean['Ethnicity'].where(
    df_clean['Ethnicity'].isna(),
    df_clean['Ethnicity'].str.capitalize()
)

print("\nAfter standardization - Ethnicity unique values:")
print(df_clean['Ethnicity'].value_counts(dropna=False))
```  
**_📸 Output Preview:_**  

![Standardize ethinicity column](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Standardrize%20Ethinicity%20Column.png) 
  ```python
# Standardize Diagnosis_Code column
print("Before standardization - Diagnosis_Code unique values:")
print(df_clean['Diagnosis_Code'].value_counts(dropna=False))

# Convert to uppercase and fix common typos
df_clean['Diagnosis_Code'] = df_clean['Diagnosis_Code'].astype(str).str.strip().str.upper()
df_clean['Diagnosis_Code'] = df_clean['Diagnosis_Code'].replace({
    'NAN': np.nan,
    'ANXITY': 'ANX',  # Fix typo
    'OCD.': 'OCD'      # Remove trailing period
})

print("\nAfter standardization - Diagnosis_Code unique values:")
print(df_clean['Diagnosis_Code'].value_counts(dropna=False))
```
**_📸 Output Preview:_**  

![Standardize diagnosis-code column](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Standardize%20Diagnosis%20Column.png) 
  ```python
# Fill missing categorical values with 'Unknown'
df_clean['Gender'] = df_clean['Gender'].fillna('Unknown')
df_clean['Ethnicity'] = df_clean['Ethnicity'].fillna('Unknown')
df_clean['Diagnosis_Code'] = df_clean['Diagnosis_Code'].fillna('Unknown')

print("Missing categorical values filled with 'Unknown'")
print("\nFinal categorical value counts:")
print("\nGender:")
print(df_clean['Gender'].value_counts())
print("\nEthnicity:")
print(df_clean['Ethnicity'].value_counts())
print("\nDiagnosis_Code:")
print(df_clean['Diagnosis_Code'].value_counts())
```
**_📸 Output Preview:_**  

![Fill missing categorical values with 'Unknown'](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Fill%20missing%20categorical%20values%20with%20Unknown.png) 
## Step 7: Normalize mixed units and engineer BMI feature

Healthcare data often contains mixed measurement units due to different countries or systems using different standards (metric vs imperial). You need to:

1. **Normalize Weight**: Convert all weights to kg (from mixed kg and lbs)
2. **Convert Height**: Convert cm to meters for BMI calculation
3. **Engineer BMI**: Body Mass Index is a clinically important derived feature

**BMI Formula**: BMI = Weight(kg) / Height(m)²

**BMI Categories**:
- Underweight: < 18.5
- Normal: 18.5 - 24.9
- Overweight: 25 - 29.9
- Obese: ≥ 30

```python
# Function to convert weight to kg (handles both numeric kg and string 'lbs' format)
def weight_to_kg(x):
    """
    Convert weight to kilograms.
    Handles numeric values (assumed kg) and strings with 'lbs' suffix.

    Examples:
    70 -> 70.0 kg
    '150lbs' -> 68.04 kg
    '150 lbs' -> 68.04 kg
    """
    if pd.isna(x):
        return np.nan

    # If already numeric, assume it's in kg
    if isinstance(x, (int, float, np.integer, np.floating)):
        return float(x)

    # Handle string values
    s = str(x).strip().lower()

    # Check for lbs pattern (e.g., '150lbs' or '150 lbs')
    match = re.match(r'^\s*([0-9]+(?:\.[0-9]+)?)\s*lbs?\s*$', s)
    if match:
        lbs = float(match.group(1))
        return round(lbs * 0.45359237, 2)  # Convert lbs to kg

    # Try to parse as numeric (assume kg)
    try:
        return float(s)
    except:
        return np.nan

# Apply weight conversion
df_clean['Weight_kg'] = df_clean['Weight'].apply(weight_to_kg)

print("Weight conversion examples:")
display(df_clean[['Weight', 'Weight_kg']].head(10))
```
**_📸 Output Preview:_**  

![Normalize Mixed units and eengeener BMI feature](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Normalize%20Mixed%20units%20and%20eengeener%20BMI%20feature.png)  


### Convert height from cm to meters
  ```python
# Convert height from cm to meters
df_clean['Height_cm'] = pd.to_numeric(df_clean['Height_cm'], errors='coerce')
df_clean['Height_m'] = df_clean['Height_cm'] / 100.0

print("Height converted from cm to meters")
```

**_📸 Output Preview:_**  

![height converted from cm to meters](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Height%20converted%20from%20cm%20to%20meter.png)  

### Calculate BMI (Body Mass Index)
  ```python
# Calculate BMI (Body Mass Index)
df_clean['BMI'] = df_clean.apply(
    lambda row: round(row['Weight_kg'] / (row['Height_m'] ** 2), 2)
    if pd.notna(row['Weight_kg']) and pd.notna(row['Height_m']) and row['Height_m'] > 0
    else np.nan,
    axis=1
)

print("BMI calculated successfully")
print("\nSample of engineered features:")
display(df_clean[['Weight', 'Weight_kg', 'Height_cm', 'Height_m', 'BMI']].head(10))
```

**_📸 Output Preview:_**  

![BMI calculated sucessfully](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Calculated%20BMI%20Successfully.png)  
## Step 8: Handle missing values with imputation

Missing values are inevitable in healthcare data. Common causes include:
- Tests not performed for all patients
- Data entry errors
- Equipment failures
- Patient privacy restrictions

### Why use Median instead of Mean?

For healthcare data, **median imputation** is often preferred over mean because:

1. **Robust to outliers**: Healthcare data often contains extreme values (very high glucose, unusual ages)
2. **Mean is sensitive**: A few extreme values can skew the mean significantly
3. **Median represents center**: The middle value of sorted data, unaffected by extremes
4. **Preserves distribution**: Better maintains the shape of skewed distributions
5. **Simple and fast**: Computationally efficient with no assumptions about distribution

You'll impute missing values in numeric columns (Age, Weight_kg, Height_cm, BMI, Glucose_mg_dL) using their respective medians.
  ```python
# Check missing values before imputation
print("Missing values before imputation:")
numeric_cols = ['Age', 'Weight_kg', 'Height_cm', 'BMI', 'Glucose_mg_dL']
print(df_clean[numeric_cols].isnull().sum())
```
**_📸 Output Preview:_**  

![Check missing values before imputation](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Handle%20messing%20value%20inputation.png)  

### Median imputation for numeric columns
  ```python
# Median imputation for numeric columns
for col in numeric_cols:
    median_val = df_clean[col].median()
    df_clean[col] = df_clean[col].fillna(median_val)
    print(f"Imputed {col} with median = {median_val}")
```
**_📸 Output Preview:_**  

![Median imputation for numeric columns](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Median%20Imputation%20numeric%20values.png)  

### Verify imputation
  ```python
# Verify imputation
print("\nMissing values after imputation:")
print(df_clean[numeric_cols].isnull().sum())
print("\nAll numeric missing values successfully imputed!")
```
**_📸 Output Preview:_**  

![Verify imputation](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Missing%20Values%20for%20imputation.png)    

## Step 9: Parse dates and engineer temporal features

Temporal features can be highly informative in healthcare:
- **Diagnosis year**: May reflect changes in diagnostic practices or disease prevalence
- **Time since diagnosis**: Important for understanding disease progression
- **Seasonal patterns**: Some conditions vary by time of year

You'll parse the inconsistent date formats and extract useful temporal features.  

  ```python
# Parse dates with mixed formats (YYYY-MM-DD and DD/MM/YYYY)
df_clean['Diagnosis_Date_parsed'] = pd.to_datetime(
    df_clean['Diagnosis_Date'],
    errors='coerce',  # Convert unparseable dates to NaT (Not a Time)
    dayfirst=True     # Assume day comes first in ambiguous formats
)

print("Date parsing results:")
print(f"Successfully parsed: {df_clean['Diagnosis_Date_parsed'].notna().sum()} dates")
print(f"Failed to parse: {df_clean['Diagnosis_Date_parsed'].isna().sum()} dates")

print("\nSample of original vs parsed dates:")
display(df_clean[['Diagnosis_Date', 'Diagnosis_Date_parsed']].head(10))
```

**_📸 Output Preview:_**  

![Date parsing results](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Date%20parsing%20result.png)   

  ```python
# Extract year from diagnosis date
df_clean['Diagnosis_Year'] = df_clean['Diagnosis_Date_parsed'].dt.year

# Calculate days since diagnosis (relative to most recent date in dataset)
ref_date = df_clean['Diagnosis_Date_parsed'].max()
if pd.isna(ref_date):
    ref_date = pd.to_datetime("today")

df_clean['Days_Since_Diagnosis'] = (
    ref_date - df_clean['Diagnosis_Date_parsed']
).dt.days

print(f"\nReference date for calculating time since diagnosis: {ref_date.date()}")
print("\nSample of engineered temporal features:")
display(df_clean[['Diagnosis_Date_parsed', 'Diagnosis_Year', 'Days_Since_Diagnosis']].head(10))
```

**_📸 Output Preview:_**  

![Extract year from diagnosis date](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Referrence%20date%20for%20calculating%20since%20diagnosis.png)   

## Step 10: Encode categorical variables

Machine learning algorithms require numeric input. Categorical variables must be converted to numbers through **encoding**.

### One-hot encoding

One-hot encoding creates **binary (0/1) columns** for each category:

**Example**: If Diagnosis_Code has values ['DEP', 'OCD', 'ANX']
- Creates columns: `Diagnosis_Code_DEP`, `Diagnosis_Code_OCD`, `Diagnosis_Code_ANX`
- A patient with 'OCD' gets: [0, 1, 0]

**Why use one-hot encoding?**
- Treats all categories equally (no implicit ordering)
- Works with all ML algorithms
- Prevents models from assuming numerical relationships between categories

**Alternative**: Label Encoding (1, 2, 3...) should only be used for ordinal data with natural ordering.

You'll apply one-hot encoding to Gender, Ethnicity, and Diagnosis_Code.  


  ```python
# Apply one-hot encoding to categorical columns
print("Columns before encoding:")
print(df_clean.columns.tolist())
print(f"Total columns: {len(df_clean.columns)}")

df_final = pd.get_dummies(
    df_clean,
    columns=['Diagnosis_Code', 'Gender', 'Ethnicity'],
    drop_first=False  # Keep all columns (set True to drop one for linear models)
)

print("\nColumns after encoding:")
print(df_final.columns.tolist())
print(f"Total columns: {len(df_final.columns)}")
print(f"\nNew encoded columns created: {len(df_final.columns) - len(df_clean.columns)}")
```


**_📸 Output Preview:_**  

![Columns before and after encoding](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Columns%20before%20and%20after%20encoding3.png)   


  ```python
# Preview the encoded dataset
print("Sample of encoded data:")
display(df_final.head())
```
**_📸 Output Preview:_**  

![Sample of encoded data](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Sample%20of%20encoded%20data3.png)   

## Step 11: Scale numeric features

### Why scale features?

Many machine learning algorithms are sensitive to feature scale:
- **Example**: Age (range 0-100) vs Glucose (range 70-500)
- Without scaling, algorithms may give more importance to features with larger values
- Algorithms affected: Logistic Regression, SVM, KNN, Neural Networks, K-Means
- Algorithms NOT affected: Tree-based models (Decision Trees, Random Forest, XGBoost)

### StandardScaler (Z-score normalization)

**Formula**: z = (x - μ) / σ
- Transforms data to have **mean = 0** and **standard deviation = 1**
- **Best for**: Algorithms assuming normal distribution (Linear/Logistic Regression, SVM)
- **Range**: Typically between -3 and +3 (but unbounded)

You'll apply StandardScaler to all numeric features.  
  ```python
# Define numeric columns to scale
numeric_cols_to_scale = ['Age', 'Weight_kg', 'Height_cm', 'BMI', 'Glucose_mg_dL', 'Days_Since_Diagnosis']

# Filter to only existing columns
numeric_cols_existing = [col for col in numeric_cols_to_scale if col in df_final.columns]

print(f"Scaling {len(numeric_cols_existing)} numeric features:")
print(numeric_cols_existing)
```
**_📸 Output Preview:_**  

![Scaling 6 numeric features](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Scaling%206%20numeric%20figure.png)   


  ```python
# Fill any remaining missing values with median before scaling
df_final[numeric_cols_existing] = df_final[numeric_cols_existing].fillna(
    df_final[numeric_cols_existing].median()
)

print("Verified no missing values before scaling:")
print(df_final[numeric_cols_existing].isna().sum())
```
**_📸 Output Preview:_**  

![Verifying no missing values before scaling](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Verifiyed%20no%20missing%20values%20before%20scaling.png)   
  ```python
# Apply StandardScaler
scaler = StandardScaler()
scaled_columns = [col + '_scaled' for col in numeric_cols_existing]
df_final[scaled_columns] = scaler.fit_transform(df_final[numeric_cols_existing])

print("Scaling complete!")
print("\nScaled features statistics (should have mean≈0, std≈1):")
display(df_final[scaled_columns].describe())
```
**_📸 Output Preview:_**  

![Scaling complete](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Scaling%20complete.png)   

  ```python
# Compare original vs scaled values
print("\nComparison of original vs scaled values:")
comparison_cols = ['Age', 'Age_scaled', 'Glucose_mg_dL', 'Glucose_mg_dL_scaled']
display(df_final[comparison_cols].head(10))
```
**_📸 Output Preview:_**  

![Comparision of original vs scaled values](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Comparision%20of%20original%20vs%20scaled%20values.png)   

## Step 12: Save the cleaned dataset

Now that you've completed all preprocessing steps, you'll save the cleaned dataset to a CSV file. This file is now ready for:
- Exploratory data analysis (EDA)
- Machine learning model training
- Statistical analysis
- Sharing with team members (with PII removed)

```python
# Save cleaned dataset
output_path = "healthcare_cleaned_data.csv"
df_final.to_csv(output_path, index=False)

print(f"✓ Cleaned dataset saved to: {output_path}")
print(f"\nFinal dataset shape: {df_final.shape[0]} rows × {df_final.shape[1]} columns")
print(f"Original dataset shape: {df.shape[0]} rows × {df.shape[1]} columns")
```


**_📸 Output Preview:_**  

![Cleaned dataset saved](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Cleaned%20dataset%20saved.png)   

```python
# Display working directory
import os
print(f"\nFile saved in directory: {os.getcwd()}")
```
**_📸 Output Preview:_**  

![File saved in directory](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Filesaved%20in%20a%20directory.png)   

## Step 13: Evaluate data cleaning results

Let's compare the raw and cleaned datasets to verify preprocessing was successful.  
```python
# Compare column structures
print("="*60)
print("COLUMN COMPARISON")
print("="*60)
print(f"\nRaw data columns ({len(df.columns)}):")
print(df.columns.tolist())
print(f"\nCleaned data columns ({len(df_clean.columns)}):")
print(df_clean.columns.tolist())
print(f"\nFinal encoded data columns ({len(df_final.columns)}):")
print(df_final.columns.tolist())
```

**_📸 Output Preview:_**  

![Compare column structure](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Compare%20column%20structure2.png)   

```python
# Compare missing values
print("\n" + "="*60)
print("MISSING VALUES COMPARISON")
print("="*60)
print("\nRaw data missing values:")
print(df.isna().sum())
print(f"\nTotal missing values in raw data: {df.isna().sum().sum()}")

print("\nCleaned data missing values:")
print(df_clean.isna().sum())
print(f"\nTotal missing values in cleaned data: {df_clean.isna().sum().sum()}")
```
**_📸 Output Preview:_**  

![Missing values comparision](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Missing%20Values%20comparision.png)  
### Compare Statistical Summaries  
```python
# Compare statistical summaries
print("\n" + "="*60)
print("STATISTICAL SUMMARY COMPARISON")
print("="*60)
print("\nRaw data summary:")
display(df.describe())

print("\nCleaned data summary:")
display(df_clean.describe())
```
**_📸 Output Preview:_**  

![Statistical Summary comparison](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Statistical%20Summary%20compariosn.png)  
![Cleaned data summary](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Cleaned%20data%20summary.png)  

### Compare Scategorical standardization (Gender example)
```python
# Compare categorical standardization (Gender example)
print("\n" + "="*60)
print("CATEGORICAL STANDARDIZATION - GENDER EXAMPLE")
print("="*60)
print("\nRaw Gender value counts:")
print(df['Gender'].value_counts(dropna=False))

print("\nCleaned Gender value counts:")
print(df_clean['Gender'].value_counts(dropna=False))

print(" Successfully standardized from 6 variations to 4 consistent categories!")
```
**_📸 Output Preview:_**  

![Compare Categorical standardization](https://github.com/Molla-Adugna/healthcare-data-cleaning-and-quality-assessment/blob/main/screen_shot_images/Categorical%20Standardization.png)   




# 🏥 Healthcare Data Cleaning & Quality Assessment
> **Part of the Healthcare Data Analytics Portfolio**  
> *Transforming messy, non-compliant clinical data into clean, privacy-preserved, ML-ready datasets.*

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Healthcare Analytics](https://img.shields.io/badge/Healthcare_Analytics-0078D4?style=for-the-badge)
![Data Cleaning](https://img.shields.io/badge/Data_Cleaning-2E8B57?style=for-the-badge)
![ML Ready](https://img.shields.io/badge/ML_Ready-FF6F00?style=for-the-badge)

---

## 📌 Table of Contents
- [Project Overview](#-project-overview)
- [Key Objectives](#-key-objectives)
- [Dataset Architecture](#-dataset-architecture)
- [Data Privacy & HIPAA Compliance](#-data-privacy-and-compliance)
- [Data Processing Workflow](#-data-processing-workflow)
- [Before vs. After Comparison](#-results--impact)
- [Repository Structure](#-project-structure)
- [How to Run](#-how-to-run)
- [Author](#-author)

---

## 📖 Project Overview

Healthcare data in real-world clinical environments is notoriously dirty—suffering from inconsistent data entry, mixed measurement systems, duplicate records, missing clinical codes, and embedded **Personally Identifiable Information (PII)**. 

This repository details an end-to-end healthcare data preprocessing and quality assessment pipeline written in **Python (Pandas/NumPy)**. The pipeline ingests raw, messy patient data and produces an anonymized, standardized, and machine-learning-ready dataset optimized for clinical prediction models.

> ⚠️ **Disclaimer:** The dataset used is a synthetic healthcare dataset sourced from the *Coursera Healthcare Data Analytics Lab*. It contains no real patient health information (PHI/PII) and is intended solely for educational and analytical demonstration.

---

## 🎯 Key Objectives

* **Data Quality Audit:** Identify, quantify, and document clinical data anomalies (missing values, typos, duplicate visits, unit mismatches).
* **Privacy Preservation:** Strip PII attributes (`Patient_Name`, `EmailID`) to align with healthcare privacy standards (HIPAA/GDPR compliance frameworks).
* **Clinical Unit Standardization:** Unify mixed measurement systems (e.g., converting mixed `lbs` and `kg` values into standardized `kg`).
* **Feature Engineering:** Derive actionable clinical indicators such as **Body Mass Index (BMI)** from raw height and weight.
* **ML Readiness:** One-hot encode categorical features and scale numeric variables for downstream modeling.

---

## 📂 Dataset Architecture

| Attribute | Specification |
| :--- | :--- |
| **Source** | Coursera Healthcare Data Analytics Lab |
| **Record Count** | 200 Synthetic Patient Records |
| **Format** | Tabular CSV |
| **Domain** | Healthcare Operations & Clinical Risk |

### Initial Data Schema & Quality Issues

| Field Name | Data Type | Identified Issues / Preprocessing Goal |
| :--- | :--- | :--- |
| `Patient_ID` | String | Unique Identifier (Kept) |
| `Patient_Name` | String | **PII** ❌ (Removed for Privacy) |
| `EmailID` | String | **PII** ❌ (Removed for Privacy) |
| `Age` | Integer | Missing values & unrealistic outliers |
| `Gender` | Categorical | Inconsistent entries (`M`, `Male`, `F`, `Female`) |
| `Ethnicity` | Categorical | Standardized categories |
| `Weight` | Mixed | Mixed units (`lbs` vs. `kg`) |
| `Height_cm` | Float | Used alongside weight for BMI calculation |
| `Diagnosis_Date` | Date | Mixed formats (`YYYY-MM-DD` and `DD/MM/YYYY`) |
| `Diagnosis_Code` | Categorical | Typos (`ANXITY` instead of `ANX`) & missing values |
| `Glucose_mg_dL` | Numeric | Missing clinical values |
| `Risk` | Binary | Target Variable (`0` or `1`) |

---

## 🔒 Data Privacy and Compliance

Prior to statistical analysis or model training, healthcare data pipelines must strictly enforce privacy guards.  

[ Raw Dataset ] ──► [ PII Inspection ] ──► [ Drop Name & Email ] ──► [ De-identified Dataset ]  
* **Stripped Fields:** `Patient_Name`, `EmailID`
* **Result:** Minimizes risk of patient re-identification while preserving the statistical utility of demographic and clinical metrics.

---

## 🔄 Data Processing Workflow

```mermaid
flowchart TD
    A[Raw Healthcare CSV] --> B[Exploratory Data Audit]
    B --> C[PII Removal & Anonymization]
    C --> D[Missing Value Imputation]
    D --> E[Categorical & Date Standardization]
    E --> F[Unit Conversion & Outlier Treatment]
    F --> G[Feature Engineering: BMI & Indicators]
    G --> H[Categorical Encoding & Feature Scaling]
    H --> I[Export Clean, ML-Ready CSV]

📊 Results & Impact
Before vs. After Preprocessing
```python
Metric / DimensionRaw Dataset (Before)Processed Dataset (After)
Privacy ComplianceExposed PII (Name, Email)100% De-identified
Categorical ConsistencyMixed (M, Male, F, Female)Standardized (Male, Female)
Measurement UnitsMixed (kg and lbs)Standardized Metric System (kg)
Clinical DiagnosticsTypos present (ANXITY)Normalized Codes (ANX)
Date FormatsUnparsed / Mixed StringsISO-8601 (YYYY-MM-DD)
Derived MetricsNoneCalculated BMI & Risk Flags
Machine Learning ReadyNo (Strings, NaNs, Raw scale)Yes (Encoded, Scaled, Clean)
```
