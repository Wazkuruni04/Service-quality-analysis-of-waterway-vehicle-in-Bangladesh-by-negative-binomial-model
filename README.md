# Service-quality-analysis-of-waterway-vehicle-in-Bangladesh-by-negative-binomial-model
import pandas as pd
import statsmodels.api as sm
import statsmodels.formula.api as smf

# Load the Excel file
file_path = "Data Barishal RUET.xlsx"
xls = pd.ExcelFile("/Data Barishal RUET.xlsx")

# Load relevant sheets
data_df = pd.read_excel(xls, sheet_name="DATA")

# Selecting relevant columns
independent_vars = [
    "gender binary", "agelessthantwenty", "agetwentytoforty", "agefortytosixty", 
    "agegreaterthansixty", "uneducated", "education"
]

# Creating a dependent variable (Perception Score)
data_df["Perception_Score"] = data_df[["Safety FacilitiesTotal", "Cleanliness Total SQTotal"]].sum(axis=1)

# Dropping rows with missing values
df_clean = data_df[independent_vars + ["Perception_Score"]].dropna()

# Fit Negative Binomial model
formula = "Perception_Score ~ " + " + ".join(independent_vars)
model = smf.glm(formula, data=df_clean, family=sm.families.NegativeBinomial()).fit()

# Print summary
print(model.summary())
