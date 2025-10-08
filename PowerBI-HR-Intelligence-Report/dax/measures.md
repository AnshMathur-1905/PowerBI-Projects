# 🧠 DAX Measures – HR Intelligence Report

This folder contains all DAX measures used in the **Power BI HR Intelligence Report**, structured by category.

Each measure is formatted and documented for clarity.

---
## 📊 KPI & Core Calculations
### 1️⃣ Total Employees
```DAX
Total Employees = DISTINCTCOUNT('Employee Fact'[employee_id])
```
### 2️⃣ Headcount
```DAX
Headcount =
CALCULATE(
    [Total Employees],
    FILTER(
        'Employee Fact',
        'Employee Fact'[hire_date] <= LASTDATE('Date'[Date]) &&
        (
            'Employee Fact'[term_date] > LASTDATE('Date'[Date]) ||
            'Employee Fact'[term_date] = BLANK()
        )
    )
)
```
### 3️⃣ Starting Headcount
```DAX
Starting Headcount =
CALCULATE(
    [Total Employees],
    FILTER(
        'Employee Fact',
        'Employee Fact'[hire_date] < FIRSTDATE('Date'[Date]) &&
        (
            'Employee Fact'[term_date] = BLANK() ||
            'Employee Fact'[term_date] >= FIRSTDATE('Date'[Date])
        )
    )
)
```
### 4️⃣ Ending Headcount
```DAX
Ending Headcount =
CALCULATE(
    [Total Employees],
    FILTER(
        'Employee Fact',
        'Employee Fact'[hire_date] < FIRSTDATE('Date'[Date]) &&
        (
            'Employee Fact'[term_date] = BLANK() ||
            'Employee Fact'[term_date] >= LASTDATE('Date'[Date])
        )
    )
)
```
### 5️⃣ Departing Employees
```DAX
Departing Employees =
CALCULATE(
    [Total Employees],
    FILTER(
        'Employee Fact',
        'Employee Fact'[term_date] >= FIRSTDATE('Date'[Date]) &&
        'Employee Fact'[term_date] <= LASTDATE('Date'[Date])
    )
)
```
### 6️⃣ Avg # of Employees
```DAX
Avg # of employees = ([Starting Headcount] + [Headcount]) / 2
```
### 7️⃣ Retention %
```DAX
Retention % = DIVIDE([Ending Headcount], [Starting Headcount], 0)
```
### 8️⃣ Turnover %
```DAX
Turnover % = DIVIDE([Departing Employees], [Avg # of employees], 0)
```

## 🏷️ Text & Title Measures
Below are measures for dynamic titles and label formatting used in KPI cards and visuals.

### 1️⃣ Avg # of Employees Text
```DAX
Avg # of Employees Text =
"Avg # of Employees: " & FORMAT([Avg # of employees], "###,##0")
```
### 2️⃣ Current Headcount Title 
```DAX
Current Headcount Ttile =
"Employee Headcount (as of " & MAX('Date'[Year]) & ")"
```
### 3️⃣ Departing Employees Text
```DAX
Departing Employees Text =
"Departing Employees: " & FORMAT([Departing Employees], "###,##0")
```
### 4️⃣ Starting Headcount Text
```DAX
Starting Headcount Text =
"Starting Headcount: " & FORMAT([Starting Headcount], "###,##0")
```
### 5️⃣ Ending Headcount Text
```DAX
Ending Headcount Text =
"Ending Headcount: " & FORMAT([Ending Headcount], "###,##0")
```
### 6️⃣ Retention Title =
```DAX
"Employee Retention (" & MIN('Date'[Year]) & " - " & MAX('Date'[Year]) & ")"
```
### 7️⃣ Turnover Title
```DAX
Turnover Title =
"Employee Turnover (" & MIN('Date'[Year]) & " - " & MAX('Date'[Year]) & ")"
```
### 8️⃣ Parameter Title
```DAX
Parameter Title =
"Select Field to Compare Retention Rate (" &
MIN('Date'[Year]) & " - " & MAX('Date'[Year]) & ")"
```
### 9️⃣ Slope Chart Title
```DAX
Slope Chart Title =
"Retention Change (" &
MIN('Date'[Year]) & " & " & MAX('Date'[Year]) &
") by " & MIN(Parameter[Parameter])
```
## ⚙️ Supporting Logic
Helper measures used for dynamic calculations and chart interactions.

### 1️⃣ Min-Max Retention
```DAX
Min-Max Retention =
SWITCH(
    TRUE(),
    SELECTEDVALUE('Date'[Year]) =
        CALCULATE(MIN('Date'[Year]), ALLSELECTED('Date')), [Retention %],
    SELECTEDVALUE('Date'[Year]) =
        CALCULATE(MAX('Date'[Year]), ALLSELECTED('Date')), [Retention %],
    BLANK()
)
```
