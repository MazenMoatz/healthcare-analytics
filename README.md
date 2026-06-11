# 🏥 Healthcare Analytics Dashboard
> A comprehensive 6-page Power BI dashboard analyzing 500K+ patient visits across financial performance, patient analytics, operational efficiency, insurance coverage, and doctor performance.

---

## 📊 Dashboard Pages

### 1. Executive Overview
High-level snapshot of the entire healthcare system.
- **$4.706bn** Total Revenue | **$2.039bn** Outstanding Amount | **$9.413K** Avg Revenue Per Visit
- **500K** Total Visits | **50K** Total Patients | **20.05%** Readmission Rate
- Total Revenue by Month (trend line)
- Total Visits by Status (Completed, Pending, No-show, Cancelled)
- Revenue by Hospital
- Filters: Visit Date, Hospital Name, Visit Type

![Executive Overview](Executive_Overview.png)

---

### 2. Financial Performance
Deep dive into billing, revenue, and payment behavior.
- **$5.042bn** Total Billed | **$4.706bn** Total Revenue | **$2.039bn** Outstanding Amount | **50.04%** Collection Rate
- Total Revenue and Outstanding Amount by Month (area chart)
- Total Revenue by Payment Method (Installment, Insurance, Cash, Waived)
- Total Visits by Payment Status (Paid, Unpaid, Partial, Disputed)
- Filters: Billing Date, Hospital Name, Payment Method

![Financial Performance](Financial_Performance.png)

---

### 3. Patient Analytics
Patient demographics, distribution, and readmission insights.
- **50K** Total Registered Patients | **47.64** Avg Patient Age | **20.05%** Readmission Rate | **100%** Insured Patients
- Total Registered Patients by City (Giza, Tanta, Suez, Luxor)
- Total Registered Patients by Gender
- Total Visits by Diagnosis (Hypertension, Renal Failure, Eczema)
- Total Registered Patients by Month (trend)
- Filters: Year/Quarter/Month, City, Gender

![Patient Analytics](Patient_Analytics.png)

---

### 4. Operational Efficiency
Visit operations, cancellations, and capacity utilization.
- **500K** Total Visits | **54.78** Avg Visit Duration | **16.62%** Cancelled Visit Rate | **21.63K** Bed Utilization
- Total Visits by Status (donut)
- Total Visits by Visit Type (Outpatient, Inpatient, Emergency)
- Total Visits by Month (line chart — peak in January & March)
- Filters: Visit Date, Department Name, Visit Type

![Operational Efficiency](Operational_Efficiency.png)

---

### 5. Insurance & Coverage
Insurance provider analysis and claims tracking.
- **$2.350bn** Total Insurance Covered | **$2.039bn** Outstanding Amount | **86.91%** Avg Coverage | **16.55%** Disputed Claims
- Total Insurance Covered by Provider Name
- Outstanding Amount by Provider Name
- Total Registered Patients by Plan Type (Corporate, Premium, Standard, Basic)
- Detailed table: Provider, Insurance Covered, Outstanding Amount, Disputed Claims
- Filters: Year/Quarter/Month, Provider Name, Plan Type

![Insurance & Coverage](Insurance___Coverage.png)

---

### 6. Doctor Performance
Doctor distribution, specialties, and workload analysis.
- **300** Total Doctors | **17.42** Avg Years Experience | **1.67K** Visits per Doctor
- Total Doctors by Gender (donut)
- Total Doctors by Specialty (Surgeon, Radiologist, Pediatrician, Nephrologist)
- Total Doctors by Hospital Name
- Filters: Hospital Name, Specialty, Visit Date

![Doctor Performance](Doctor_Performance.png)

---

## 🛠️ Tools & Technologies

| Tool | Usage |
|------|-------|
| Power BI Desktop | Dashboard development & visualization |
| Power Query (M) | Data transformation & cleaning |
| DAX | KPI calculations & measures |
| Microsoft Excel | Data source |

---

## 🗂️ Data Model

7 tables connected in a Star Schema:

| Table | Type | Description |
|-------|------|-------------|
| `Faact_Billing` | Fact | Billing transactions and payment records |
| `Fact_Visits` | Fact | Patient visit records |
| `Dim_Patient` | Dimension | Patient demographics |
| `Dim_Doctor` | Dimension | Doctor profiles and specialties |
| `Dim_Department` | Dimension | Hospital departments |
| `Dim_Hosptial` | Dimension | Hospital information |
| `Dim_Insurance` | Dimension | Insurance providers and plans |

> ⚠️ **Note:** `Faact_Billing` has no direct relationship to `Dim_Hosptial` — resolved using `TREATAS()` DAX workaround.

---

## 🧮 DAX Measures

| # | Measure | Formula |
|---|---------|---------|
| 1 | Total Visits | `COUNTROWS(Fact_Visits)` |
| 2 | Total Patients | `COUNTROWS(Dim_Patient)` |
| 3 | Total Revenue | `SUM(Faact_Billing[amount_paid])` |
| 4 | Total Billed | `SUM(Faact_Billing[billed_amount])` |
| 5 | Outstanding Amount | `SUM(Faact_Billing[outstanding_amount])` |
| 6 | Collection Rate % | `DIVIDE([Total Revenue], [Total Billed])` |
| 7 | Avg Revenue Per Visit | `DIVIDE([Total Revenue], [Total Visits])` |
| 8 | Readmission Rate % | `DIVIDE(COUNTROWS(FILTER(Fact_Visits, Fact_Visits[is_readmission] = TRUE())), [Total Visits])` |
| 9 | Cancelled Visit Rate % | `DIVIDE(COUNTROWS(FILTER(Fact_Visits, Fact_Visits[status] = "Cancelled")), [Total Visits])` |
| 10 | Avg Visit Duration | `AVERAGE(Fact_Visits[visit_duration])` |
| 11 | Total Doctors | `COUNTROWS(Dim_Doctor)` |
| 12 | Avg Years Experience | `AVERAGE(Dim_Doctor[years_experience])` |
| 13 | Visits per Doctor | `DIVIDE([Total Visits], [Total Doctors])` |
| 14 | Total Insurance Covered | `SUM(Dim_Insurance[insurance_covered])` |
| 15 | Avg Coverage % | `AVERAGE(Dim_Insurance[coverage_pct])` |
| 16 | Disputed Claims % | `DIVIDE(COUNTROWS(FILTER(Faact_Billing, Faact_Billing[payment_status] = "Disputed")), [Total Visits])` |
| 17 | Insured Patients % | `DIVIDE(COUNTROWS(FILTER(Dim_Patient, Dim_Patient[is_insured] = TRUE())), [Total Patients])` |
| 18 | Avg Patient Age | `AVERAGE(Dim_Patient[age])` |

---

## ❓ Business Questions Answered

**Financial**
- What is the total revenue generated across all hospitals?
- Which payment method contributes the most revenue?
- What percentage of billed amounts remain outstanding?
- How does revenue trend month over month?

**Patient Analytics**
- Which cities have the highest number of registered patients?
- What is the gender distribution of patients?
- Which diagnoses are most common across visits?
- How does patient registration trend throughout the year?

**Operational**
- What percentage of visits are cancelled or result in no-shows?
- What is the distribution between Outpatient, Inpatient, and Emergency visits?
- Which months have the highest patient visit volume?
- What is the average visit duration across departments?

**Insurance & Coverage**
- Which insurance provider covers the most patients?
- Which provider has the highest outstanding amount?
- What is the distribution of patients by insurance plan type?
- What percentage of claims are disputed?

**Doctor Performance**
- Which hospital has the most doctors?
- What is the most common specialty among doctors?
- How many visits does each doctor handle on average?
- What is the gender distribution among doctors?

---

## 💡 Key Insights

- **Collection Rate of 50.04%** — only half of billed amounts are collected, indicating a significant revenue leakage
- **20.05% Readmission Rate** — 1 in 5 patients returns, suggesting gaps in post-discharge care
- **16.62% Cancelled Visit Rate** — operational inefficiency that could be reduced with better scheduling
- **Maadi International Clinic** leads with 102 doctors and highest revenue ($315M+)
- **Outpatient visits dominate** at ~40% of total visit types
- **86.91% Avg Insurance Coverage** — strong insurance penetration across the patient base
- Revenue is **relatively flat month-over-month** with a slight dip in April

---

## 📁 Repository Structure

```
healthcare-analytics/
├── README.md
├── Executive_Overview.png
├── Financial_Performance.png
├── Patient_Analytics.png
├── Operational_Efficiency.png
├── Insurance___Coverage.png
└── Doctor_Performance.png
blue?logo=linkedin)](https://www.linkedin.com/in/mazen-moataz-098223349/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?logo=github)](https://github.com/MazenMoatz)
[![Email](https://img.shields.io/badge/Email-Contact-red?logo=gmail)](mailto:mazenmotz@gmail.com)
