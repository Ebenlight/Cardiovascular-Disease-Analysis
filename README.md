# Cardiovascular-Disease-Analysis
This dataset contains records of 1,000 patients to analyze cardiovascular disease risk. Features include age, sex, chest pain type, resting blood pressure, cholesterol, fasting blood sugar, maximum heart rate, exercise-induced angina, number of major vessels, and a target indicating presence (1) or absence (0) of heart disease.
##  Project Overview

This project analyzes a cardiovascular disease dataset to identify key risk factors associated with heart disease.  
The objective was to explore patient health metrics and determine which variables strongly influence the presence of heart disease (`target = 1`).

---

## Analysis Problem

Heart disease remains one of the leading causes of mortality worldwide.  
This analysis aims to uncover patterns that could support early detection and better risk assessment.

---

## Data Cleaning & Preparation

- Checked and handled missing values  
- Removed inconsistencies in categorical fields  
- Standardized column naming conventions  
- Converted appropriate columns to correct data types  
- Validated dataset integrity before analysis  

---

##  Exploratory Analysis Questions

- What proportion of patients have heart disease?
- Does age significantly impact heart disease occurrence?
- Which chest pain type is most associated with heart disease?
- How do blood pressure and cholesterol influence risk?
- Does the number of major blood vessels increase likelihood?

---

## sql
-- View all records
USE Cardiovascular;
SELECT * FROM Cardiovascular;

-- Proportion of patients with and without heart disease
SELECT
    target,
    COUNT(*) AS patientcount,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM cardiovascular), 2) AS percentage
FROM cardiovascular
GROUP BY target;

-- Age vs heart disease
SELECT
    target,
    AVG(age) AS Average_patient,
    MIN(age) AS Youngest_patient,
    MAX(age) AS Oldest_patient
FROM cardiovascular
GROUP BY target;

SELECT
    CASE
        WHEN age < 40 THEN 'Under 40'
        WHEN age BETWEEN 40 AND 49 THEN '40s'
        WHEN age BETWEEN 50 AND 59 THEN '50s'
        WHEN age BETWEEN 60 AND 69 THEN '60s'
        ELSE '70+'
    END AS Age_group,
    COUNT(*) AS total_patients,
    SUM(target) AS Heart_disease_cases,
    ROUND(SUM(target) * 100.0 / COUNT(*), 2) AS Disease_percentage
FROM cardiovascular
GROUP BY Age_group
ORDER BY Age_group DESC;

-- Chest pain type vs heart disease
SELECT chestpain,
    COUNT(*) AS total_patients,
    SUM(target) AS disease_cases,
    ROUND(SUM(target) * 100.0 / COUNT(*), 2) AS prevalence
FROM cardiovascular
GROUP BY chestpain
ORDER BY prevalence DESC;

-- Max heart rate & exercise-induced angina
SELECT target,
    AVG(maxheartrate) AS Avg_max_heartrate,
    ROUND(SUM(exerciseangia) * 100.0 / COUNT(*), 2) AS Angina_prevalence
FROM cardiovascular
GROUP BY target;

-- Risk profile based on BP & cholesterol
SELECT
    CASE
        WHEN restingBP > 130 AND serumcholestrol > 240 THEN 'High BP and Chol'
        WHEN restingBP > 130 OR serumcholestrol > 240 THEN 'One High Marker'
        ELSE 'Both Markers Normal'
    END AS Risk_profile,
    COUNT(*) AS patient_count,
    ROUND(AVG(target) * 100,2) AS heart_disease_rate
FROM cardiovascular
GROUP BY Risk_profile
ORDER BY heart_disease_rate DESC;

-- Oldpeak, major vessels, and heart disease
SELECT noofmajorvessels,
    CASE
        WHEN oldpeak = 0 THEN 'No Depression'
        WHEN oldpeak > 0 AND oldpeak < 2.0 THEN 'Mild'
        ELSE 'Severe'
    END AS Depression_Severity,
    COUNT(*) AS total_patients,
    ROUND(AVG(target) * 100,2) AS heart_disease_rate
FROM cardiovascular
GROUP BY noofmajorvessels, Depression_Severity
ORDER BY noofmajorvessels DESC, heart_disease_rate DESC;

-- Overall heart disease prevalence
SELECT ROUND(AVG(target) * 100, 2) AS Prevalence
FROM cardiovascular;
---

