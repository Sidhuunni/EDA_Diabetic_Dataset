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
