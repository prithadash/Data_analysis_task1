# Patient Health Monitoring and Analysis

This project analyzes patient health data and visualizes critical metrics such as heart rate, blood pressure, BMI, and health risk levels. The code processes the data, identifies abnormalities, and generates insightful visualizations for better understanding.

---

## Features

### 1. **Data Analysis**
- **Missing Value Detection**: Identifies empty fields in the dataset.
- **Statistical Metrics**: Calculates mean, minimum, and maximum for critical metrics such as Systolic BP, Diastolic BP, and Heart Rate.

### 2. **Health Metrics Calculation**
- **BMI Calculation**: Computes BMI for each patient using their height and weight.
- **Health Risk Classification**: Categorizes patients into `High Risk`, `Moderate Risk`, or `Low Risk` based on BMI and blood pressure thresholds.

### 3. **Abnormalities Detection**
- Identifies:
  - Patients with abnormal blood pressure (`SystolicBP > 140` or `DiastolicBP > 90`).
  - Patients with abnormal heart rates (`HeartRate > 100 bpm`).

### 4. **Data Visualization**
- **Trends in Health Metrics**: Visualizes changes in heart rate and blood pressure over time for all patients.
- **Average BMI Comparison**: Displays average BMI for each patient using a bar chart.
- **Health Risk Distribution**: Illustrates the proportion of patients in each health risk category using a pie chart.

---

## Code Walkthrough

### Dependencies
```python
import pandas as pd
import matplotlib.pyplot as plt
```

### Data Processing
1. **Load Dataset**: 
   ```python
   df = pd.read_csv("health/Pritha_Das_Patient_Health_Monitoring_and_Analysis.csv")
   ```
2. **Handle Missing Values**:
   ```python
   empty_fields = df.isnull().any().any()
   ```
3. **Compute Statistics**:
   ```python
   metrics = ["SystolicBP", "DiastolicBP", "HeartRate( bpm)"]
   print(df[metrics].agg(["mean", "min", "max"]))
   ```

### BMI Calculation
```python
df["BMI"] = df["Weight(kg)"] / ((df["Height(cm)"] / 100) ** 2)
```

### Health Risk Assessment
```python
def health_risk(row):
    if row["BMI"] < 18.5 or row["BMI"] > 30 or row["SystolicBP"] > 140 or row["DiastolicBP"] > 90:
        return "High Risk"
    elif 25 <= row["BMI"] <= 30 or 130 <= row["SystolicBP"] <= 140 or 80 <= row["DiastolicBP"] <= 90:
        return "Moderate Risk"
    else:
        return "Low Risk"

df["HealthRisk"] = df.apply(health_risk, axis=1)
```

### Visualization
1. **Trends in Health Metrics**:
   ```python
   unique_patients = df["PatientID"].unique()
   for patient_id in unique_patients:
       patient_data = df[df["PatientID"] == patient_id]
       plt.plot(patient_data["Date"], patient_data["HeartRate"], marker="o", label=f"Heart Rate - Patient {patient_id}")
       plt.plot(patient_data["Date"], patient_data["SystolicBP"], marker="o", label=f"Systolic BP - Patient {patient_id}")
       plt.plot(patient_data["Date"], patient_data["DiastolicBP"], marker="o", label=f"Diastolic BP - Patient {patient_id}")
   plt.show()
   ```

2. **Average BMI Bar Chart**:
   ```python
   avg_bmi = df.groupby("PatientID")["BMI"].mean()
   avg_bmi.plot(kind="bar", figsize=(10, 6), color="skyblue")
   plt.show()
   ```

3. **Health Risk Distribution Pie Chart**:
   ```python
   risk_distribution = df["HealthRisk"].value_counts()
   risk_distribution.plot(kind="pie", autopct="%1.1f%%", figsize=(8, 8), startangle=140)
   plt.show()
   ```

---

## Visual Outputs
1. **Trends in Health Metrics**: Line chart showing changes in heart rate, systolic, and diastolic blood pressure over time.
2. **Average BMI**: Bar chart comparing the average BMI of different patients.
3. **Health Risk Distribution**: Pie chart visualizing the percentage of patients in each risk group.

---

## Usage
1. Clone the repository:
   ```bash
   git clone https://github.com/prithadash/Data_analysis_task1.git
   ```
