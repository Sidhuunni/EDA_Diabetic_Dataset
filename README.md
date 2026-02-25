# EDA_Diabetic_Dataset
EDA on diabetic to find out what all factors affects diabetics
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings("ignore")
sns.set(style="whitegrid", palette="Set2")
plt.rcParams["figure.figsize"] = (10, 6)
numerical_cols = ['age', 'bmi', 'hbA1c_level', 'blood_glucose_level']
# Load file
df = pd.read_csv("diabetes_dataset.csv")
print("\n--- Dataset Preview ---")
print(df.head())
print(f"\nShape: {df.shape}")
df.info()
#Data Quality Check
#missing values and duplicates
print("\n--- Missing Values ---")
print(df.isna().sum())
print("\n--- Duplicate Rows ---")
print(f"Duplicate count: {df.duplicated().sum()}")
#Descriptive Statistics
num_cols = df.select_dtypes(include=np.number).columns
cat_cols = df.select_dtypes(exclude=np.number).columns
print("\n--- Numerical Features Summary ---")
print(df[num_cols].describe().T)
print("\n--- Categorical Features Summary ---")
for col in cat_cols:
    print(f"\n{col} distribution:\n", 
df[col].value_counts(normalize=True) * 100)
#Univariate Analysis
# Histogram + KDE + Boxplot
numerical_cols = ['age', 'bmi', 'hbA1c_level', 'blood_glucose_level']
# Print Descriptive Statistics
print("Descriptive Statistics for Numerical Features")
print(df[numerical_cols].describe().T)
#Histograms
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
axes = axes.flatten()
for i, col in enumerate(numerical_cols):
    sns.histplot(df[col], kde=True, ax=axes[i], bins=30)
    axes[i].set_title(f'Distribution of {col}', fontsize=14)
    axes[i].set_xlabel(col, fontsize=12)
plt.suptitle('Univariate Analysis: Numerical Feature Distributions', 
fontsize=16, y=1.02)
plt.tight_layout()
plt.show()
categorical_cols = [
'gender'
'heart_disease'
, 
'diabetes']
# Plot Count Plots
fig, axes =
 plt.subplots(2
, 
'smoking_history'
, 
'hypertension', 
, 3
, figsize=(
15, 10
axes =
 axes.flatten()
for
 i, col 
in 
enumerate
if i < 
(categorical_cols):
len
of columns
        sns.countplot(x
        axes[i].set_title(
for j 
))
(categorical_cols): 
# Ensure we don't exceed the number 
=df[col], ax=
axes[i], palette=
f'Count of {
col}'
'viridis')
, fontsize=
14)
        axes[i].set_xlabel(col, fontsize=
12)
        axes[i].tick_params(axis=
'x'
, rotation=
in range(
len
(categorical_cols), 
20)
len
(axes)):
    fig.delaxes(axes[j])
    plt.suptitle(
'Univariate Analysis: Categorical Feature Counts', 
fontsize=
16, y=
1.02)
plt.tight_layout()
plt.show()
# Numerical vs Numerical (scatterplots)
key_target_vars = [
'blood_glucose_level'
target_var = 
'diabetes'
fig, axes =
 plt.subplots(2
, 2
, 
'bmi'
, 
'age'
, figsize=(
axes =
 axes.flatten() 
for
 i, col 
in 
enumerate
(key_target_vars):
    sns.scatterplot(x=
alpha=
0.3)
    axes[i].set_title(
15, 12
))
df[col], y=
, 
'hbA1c_level']
df[target_var], ax=
axes[i], 
# Increased font sizes for better readability
f'{
col}
 vs. {
target_var}'
    axes[i].set_xlabel(col, fontsize=
14)
    axes[i].set_ylabel(target_var, fontsize=
    axes[i].tick_params(axis=
'both'
, fontsize=
14)
, which=
16)
'major'
, labelsize=
12)
plt.tight_layout()
plt.show()


#  Numerical vs Categorical
fig, axes =
 plt.subplots(1
axes =
 axes.flatten()
, 2
, figsize=(
10, 5
))
# 1. Boxplot: diabetes vs blood_glucose_level
sns.boxplot(x=
'diabetes'
ax=
axes[0
])
axes[0
fontsize=
12)
, y=
'blood_glucose_level'
, data=
df, 
].set_title(
'Blood Glucose Level by Diabetes Status', 
# 2. Violin Plot: heart_disease vs bmi
sns.violinplot(x=
'heart_disease'
, y=
axes[1
'bmi'
, data=
df, ax=
axes[1
].set_title(
'BMI Distribution by Heart Disease Status', 
])
fontsize=
12)
plt.tight_layout()
plt.show()
print(
"Categorical vs Categorical (Crosstab Heatmap)")
crosstab_data =
 pd.crosstab(df[
df[
'heart_disease'
'smoking_history'
], normalize=
'index'
plt.figure(figsize=(
10, 6
))
sns.heatmap(crosstab_data, annot=
cbar_kws={
'label'
: 
'Percentage (%)'
, fontsize=
], 
) * 100
True
, fmt=
'.1f'
, cmap=
})
'YlGnBu', 
plt.title(
'Percentage of Heart Disease Status (0/1) by Smoking 
History'
14)
plt.xlabel(
'Heart Disease Status (0=No, 1=Yes)'
, fontsize=
plt.ylabel(
'Smoking History'
, fontsize=
plt.tight_layout()
plt.show()
12)
Categorical vs Categorical (Crosstab Heatmap)
12)
# 6. Correlation Heatmap
if len
(num_cols) > 1:
    plt.figure(figsize=(
12, 8
))
    sns.heatmap(df[num_cols].corr(), annot=
fmt=
".2f")
    plt.title(
"Correlation Matrix")
    plt.show()
True
, cmap=
"coolwarm", 
#Statistical Tests
from
 scipy.stats 
import
 chi2_contingency, ttest_ind, f_oneway
#Chi-Square Test (Categorical vs Categorical) 
if len
(cat_cols) 
>= 2:
    chi_table =
 pd.crosstab(df[cat_cols[0
    chi2, p, dof, expected =
print(
f"\n
Chi-Square Test between {
{cat_cols[1]}
:")
print(
f"Chi2 = {
chi2
#t-test / ANOVA (Numerical vs Categorical) 
if len
(cat_cols) > 0 
    cat_var =
 cat_cols[0]
    num_var =
]], df[cat_cols[1
 chi2_contingency(chi_table)
cat_cols[0]}
:.2f}
, p-value = {p
:.4f}")
and len
(num_cols) > 0:
 num_cols[0]
    groups =
 [group[num_var].dropna() 
for
 name, group 
df.groupby(cat_var)]
if len
(groups) 
]])
 and 
in 
== 2:
        t_stat, p_val 
= ttest_ind(groups[0
equal_var=
False)
print(
f"\n
p={
p_val
:.4f}")
elif 
T-test for {
len
(groups) > 2:
        f_stat, p_val 
], groups[1
], 
num_var}
 by {
cat_var}
: t={
t_stat
= f_oneway(*
groups)
print(
f"\n
ANOVA for {
p={
p_val
:.4f}")
num_var}
 by {
cat_var}
Chi-Square Test between gender and location:
Chi2 = 110.91, p-value = 0.4046
