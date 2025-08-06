import pandas as pd

# Step 1: Load the dataset (assuming tab-separated)
df = pd.read_csv("marketing_campaign.csv", sep='\t')

# Step 2: Rename columns (lowercase, no spaces)
df.columns = [col.strip().lower().replace(" ", "_") for col in df.columns]

# Step 3: Show missing values
print("Missing values per column:")
print(df.isnull().sum())

# Step 4: Remove duplicate rows
df = df.drop_duplicates()

# Step 5: Standardize text fields
text_cols = df.select_dtypes(include='object').columns
for col in text_cols:
    df[col] = df[col].astype(str).str.strip().str.title()

# Step 6: Convert date columns
if 'dt_customer' in df.columns:
    df['dt_customer'] = pd.to_datetime(df['dt_customer'], errors='coerce', dayfirst=True)

# Step 7: Fix numeric data types
if 'year_birth' in df.columns:
    df['year_birth'] = pd.to_numeric(df['year_birth'], errors='coerce').astype('Int64')

# Step 8: Export cleaned data to Excel
df.to_excel("cleaned_marketing_campaign.xlsx", index=False)
print("✅ Cleaned dataset saved as 'cleaned_marketing_campaign.xlsx'")
