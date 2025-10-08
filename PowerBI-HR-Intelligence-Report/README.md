# 👥 HR Intelligence Report – Power BI Project

A portfolio case study analyzing workforce dynamics — including headcount, retention, and turnover trends — through interactive data visualizations built in Power BI.
The goal: equip HR teams and decision-makers with data-driven insights to manage workforce stability, improve retention, and understand attrition patterns.

## 📌 Problem Statement  
The organization needed a consolidated HR Intelligence Dashboard to monitor key workforce metrics — Headcount, Retention, and Turnover — across multiple dimensions.

The report was designed to help stakeholders:
1. Track Workforce Size → Understand total headcount trends over time and across departments, job levels, and cities.
2. Assess Stability → Measure retention performance by demographic groups, marital status, and job level.
3. Analyze Attrition → Evaluate turnover rates across departments and identify high-risk segments.
4. Identify Drivers → Examine termination types and reasons to uncover key factors influencing employee exits.
5. Strengthen Strategy → Leverage insights to develop targeted retention programs and improve workforce planning.
---

## 📂 Dataset

The dataset originates from three core source tables and was reshaped into a star schema using Power Query.

### 🧱 Source Tables
1. Employee Data Source – contains personal and demographic details: employee_id, gender, race, education, location, marital_status, employment_status, birth_date

2. Employment History Source – tracks career data and employment events: employee_id, hire_date, term_date, department, job_level, manager_hierarchy, term_type, term_reason

3. Date Table – custom calendar table for time intelligence functions (FIRSTDATE, LASTDATE, YEAR, Month, etc.)

### ⚙️ Power Query Transformations

1. Merged and normalized Employee Data and Employment History into a unified Employee Fact Table.
2. Removed Enable Load from source queries to optimize the model.
3. Created Reference Tables for key dimensions: Department Dim, Job Level Dim, Location Dim, Demographic Dim, Education Dim, Marital Status Dim, Termination Dim
4. Converted IDs (e.g., employee_id, manager_id) to Text type for relationship consistency.

### 📈 Data Summary
#### 1. Total Employees: 4,138
#### 2. Data Grain: Employee-level (1 record per employee per job history)

---
## 📊 Dashboard Pages

### **1. Cover Page**
![Cover Page](https://github.com/AnshMathur-1905/PowerBI-Projects/blob/main/PowerBI-HR-Intelligence-Report/Screenshots/01_CoverPage.png)

---
### **2. Employee Headcount**  
Key Metrics:
- Total Employees (as of selected year)
- Headcount by Department
- Headcount by Job Level
- Headcount by Demographics (Gender, Education, Marital Status, Age, Race)
- City-wise Headcount Map

![Employee Headcount](https://github.com/AnshMathur-1905/PowerBI-Projects/blob/main/PowerBI-HR-Intelligence-Report/Screenshots/02_Employee_Headcount.png)

---

### **3. Employee Retention**  
Key Metrics:
- Retention %
- Starting & Ending Headcount
- Retention by Department
- Retention by Job Level
- Retention Comparisons by demographics

![Employee Retention](https://github.com/AnshMathur-1905/PowerBI-Projects/blob/main/PowerBI-HR-Intelligence-Report/Screenshots/03_Employee_Retention.png)

---
### **4. Employee Turnover**  
Key Metrics:
- Turnover %
- Departing & Avg # of Employees
- Turnover by Year
- Employee Profiles → Demographic and job-level characteristics of leavers
- Termination Type & Reason 

![Employee Turnover](https://github.com/AnshMathur-1905/PowerBI-Projects/blob/main/PowerBI-HR-Intelligence-Report/Screenshots/04_Employee_Turnover.png)

---

## 📑 DAX Measures  

All KPIs were built with custom DAX. Full code is available in [`/dax/measures.md`](dax/measures.md). 

Key measures include:  
- **Total Employees**  
- **Retention %**  
- **Turnover %%**  
- **Headcount / Starting / Ending Headcount**  
- **Departing Employees**  

---

## 🛠️ Tools & Skills Used  

- **Power BI Desktop (2025)** – data modeling, measures, dashboards  
- **Power Query** – data transformation, column derivations, modeling  
- **DAX** – KPIs calculations
  
---

## 🚀 How to Use  

1. Clone or download this repository.
2. Open **HR-Intelligence-Report.pbix** in Power BI Desktop.
3. Explore dashboard pages using slicers and filters. 

---

