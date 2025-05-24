# 🩺 Healthcare Data Analysis Project

## 📊 Overview

This project presents a comprehensive data analysis of a simulated healthcare database using **MySQL**. By addressing real-world healthcare challenges, the project extracts actionable insights aimed at improving patient care, hospital efficiency, and financial planning. The analysis is based on data from patients, admissions, treatments, billing, medications, and insurance.

---

## 🌟 Business Problem

The healthcare industry often lacks data-driven insights to make informed strategic decisions. This impacts:

- Patient outcomes
- Operational efficiency
- Cost management

Healthcare providers struggle to optimize resources, understand treatment efficacy, and control costs due to insufficient analysis of clinical and operational data.

---

## ✅ Project Objective

Use relational healthcare data to:

- Improve **clinical outcomes**
- Enhance **hospital efficiency**
- Optimize **financial performance**

All queries are written in **MySQL** and can be run on a structured healthcare database.

---

## 🧪 Top 10 Case Studies (with SQL)

### 1. **Common Conditions in Elderly Patients**
Identify top health issues for patients aged 60+.
```sql
SELECT c.condition_name, COUNT(*) AS total_cases
FROM Admissions a
JOIN Patients p ON a.patient_id = p.patient_id
JOIN Conditions c ON a.condition_id = c.condition_id
WHERE p.age >= 60
GROUP BY c.condition_name
ORDER BY total_cases DESC
LIMIT 1;
```
### 2. **High-Billing Medical Conditions**
Find conditions with the highest average billing.

```sql
SELECT c.condition_name, ROUND(AVG(a.billing_amount), 2) AS avg_billing
FROM Admissions a
JOIN Conditions c ON a.condition_id = c.condition_id
GROUP BY c.condition_name
ORDER BY avg_billing DESC
LIMIT 1;
```
### 3. **Top Performing Doctors**
Identify doctors with the highest patient load.

```sql
SELECT d.name AS doctor_name, COUNT(*) AS total_patients
FROM Admissions a
JOIN Doctors d ON a.doctor_id = d.doctor_id
GROUP BY d.name
ORDER BY total_patients DESC
LIMIT 5;
```
### 4. **Average Hospital Billing**
Compare billing trends across hospitals.

```sql
SELECT h.name AS hospital_name, ROUND(AVG(a.billing_amount), 2) AS avg_billing
FROM Admissions a
JOIN Hospitals h ON a.hospital_id = h.hospital_id
GROUP BY h.name
ORDER BY avg_billing DESC;
```
### 5. **Emergency Care Leaders**
Spot hospitals with the most emergency admissions.

```sql
SELECT h.name AS hospital_name, COUNT(*) AS emergency_admissions
FROM Admissions a
JOIN Hospitals h ON a.hospital_id = h.hospital_id
WHERE a.admission_type = 'Emergency'
GROUP BY h.name
ORDER BY emergency_admissions DESC
LIMIT 1;
```
### 6. **Insurance Billing Analysis**
Compare billing averages across insurance providers.

```sql
SELECT ip.provider_name, ROUND(AVG(a.billing_amount), 2) AS avg_billing
FROM Admissions a
JOIN Insurance_Providers ip ON a.provider_id = ip.provider_id
GROUP BY ip.provider_name
ORDER BY avg_billing DESC;
```
### 7. **Condition-Wise Healthcare Spending**
Find the most expensive conditions overall.

```sql
SELECT c.condition_name, ROUND(SUM(a.billing_amount), 2) AS total_spending
FROM Admissions a
JOIN Conditions c ON a.condition_id = c.condition_id
GROUP BY c.condition_name
ORDER BY total_spending DESC
LIMIT 1;
```
### 8. **Diabetes Medication Trends**
Identify top medications for diabetic patients.

```sql
SELECT a.medication, COUNT(*) AS count
FROM Admissions a
JOIN Conditions c ON a.condition_id = c.condition_id
WHERE c.condition_name = 'Diabetes'
GROUP BY a.medication
ORDER BY count DESC
LIMIT 3;
```
### 9. **Medication Patterns by Demographics**
Medication usage by gender:

```sql
SELECT p.gender, a.medication, COUNT(*) AS count
FROM Admissions a
JOIN Patients p ON a.patient_id = p.patient_id
GROUP BY p.gender, a.medication
ORDER BY p.gender, count DESC;
Medication usage by age group:

```sql
SELECT
  CASE
    WHEN p.age < 30 THEN 'Under 30'
    WHEN p.age BETWEEN 30 AND 59 THEN '30-59'
    ELSE '60 and above'
  END AS age_group,
  a.medication,
  COUNT(*) AS count
FROM Admissions a
JOIN Patients p ON a.patient_id = p.patient_id
GROUP BY age_group, a.medication
ORDER BY age_group, count DESC;
```
### 10. **Hospital Stay Duration Analysis**
Average stay duration per condition.

```sql
SELECT
  c.condition_name,
  ROUND(AVG(DATEDIFF(a.discharge_date, a.admission_date)), 2) AS avg_stay_days
FROM Admissions a
JOIN Conditions c ON a.condition_id = c.condition_id
GROUP BY c.condition_name
ORDER BY avg_stay_days DESC;
```
### 🔍Key Insights: ###
```
1.Chronic conditions like hypertension or diabetes often top the list—indicating the need for geriatric-focused care programs
2.Conditions like cancer or cardiovascular diseases are the costliest, helping hospitals prioritize budgeting and insurance coverage strategies.
3.Identifies high-performing doctors who could lead training, research, or mentorship programs
4. Highlights hospitals with high average costs—useful for benchmarking and auditing
5.Shows which hospitals need more support in emergency readiness and staffing
6.Evaluates insurer cost effectiveness, guiding better partnerships and policy design
7.Helps identify cost drivers in the system, useful for policy reform or preventive campaigns
8.Top drugs (e.g., Metformin, Insulin) are revealed—helping with pharmacy inventory and supplier contracts.
9.Medication choices vary across age and gender—supports personalized medicine initiatives.
10. Pinpoints conditions with long stays—helps optimize bed availability and discharge planning.
