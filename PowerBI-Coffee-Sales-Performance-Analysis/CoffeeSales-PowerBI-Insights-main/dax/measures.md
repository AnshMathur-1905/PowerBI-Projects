# 📊 DAX Measures – The Grind Co. (Power BI)

This document contains all DAX measures used in this project.  
All measures as implemented in the report, grouped by purpose.

---
## _Measures

**% of Sales** 
```DAX
% of Sales =
DIVIDE(
    [Total Sales],
    CALCULATE([Total Sales], ALLSELECTED(Products_Dim))
)
```

**Average Order Size**
```DAX
Average Order Size =
DIVIDE([Total Sales], [Total Transactions], 0)
```

**Most Popular Seller**
```DAX
Most Popular Seller =
VAR Winners =
    FILTER ( ALLSELECTED('Products_Dim'), [Product Transaction Rank] = 1 )
RETURN
CONCATENATEX (
    Winners,
    'Products_Dim'[product_detail],
    ", ",
    'Products_Dim'[product_detail], ASC
)
```

**Product Sales Rank**
```DAX
Product Sales Rank =
RANKX(ALLSELECTED(Products_Dim), [Total Sales], , DESC, Dense)
```

**Product Transaction Rank**
```DAX
Product Transaction Rank =
RANKX(ALLSELECTED(Products_Dim), [Total Transactions], , DESC, Dense)
```

**Top Earner**
```DAX
Top Earner =
VAR Winners =
    FILTER(ALLSELECTED('Products_Dim'), [Product Sales Rank] = 1)
RETURN
CONCATENATEX(
    Winners,
    'Products_Dim'[product_detail],
    ", ",
    'Products_Dim'[product_detail], ASC
)
```


**Total Products**
```DAX
Total Products =
COUNT(Products_Dim[product_id])
```

**Total Sales**
```DAX
Total Sales =
SUM(Coffee_Sales_Fact[Total Sales])
```

**Total Stores**
```DAX
Total Stores =
COUNT(Stores_Dim[store_id])
```

**Total Transactions**
```DAX
Total Transactions =
COUNT(Coffee_Sales_Fact[transaction_id])
```

## _Formatting_Measures

**MAX AOS Marker Label**
```DAX
MAX AOS Marker Label =
"Max Order Size: " & FORMAT([Max AOS Marker], "$###,##0.00")
```

**Product_Category label (bar chart)**
```DAX
Product_Category label (bar chart) =
FORMAT([Total Sales] / 1000, "$####,##0K")
& " ("
& FORMAT([% of Sales], "###,##0.0%")
& ")"
```

**Top 10 Product Type Highlight**
```DAX
Top 10 Product Type Highlight =
IF(
    [Product Category Rank] <= 10,
    "#2F3E46",
    "#A0C29F"
)
```

## _Forecast_Measures
**Average Daily Sales**

```DAX
Average Daily Sales =
AVERAGEX(VALUES('Forecast Date Table'[Date]), [Total Sales])
```

**Average Daily Sales (H2 ALL SELECTED)**

```DAX
Average Daily Sales (H2 ALL SELECTED) =
CALCULATE([Average Daily Sales (H2)], ALLSELECTED('Forecast Date Table'[Date]))
```

**Average Daily Sales (H2)**

```DAX
Average Daily Sales (H2) =
CALCULATE(
    [Average Daily Sales],
    FILTER('Date', 'Date'[Month] = 4 || 'Date'[Month] = 5 || 'Date'[Month] = 6)
)
```

**Average Daily Sales 2**

```DAX
Average Daily Sales 2 =
CALCULATE([Average Daily Sales], ALLSELECTED('Forecast Date Table'[Date]))
```


**Average Daily Sales Switch**

```DAX
Average Daily Sales Switch =
SWITCH(
    TRUE(),
    [Total Sales] <> BLANK(), [Total Sales],
    [Average Daily Sales 2]
)
```
**Average Daily Sales Switch H2**

```DAX
Average Daily Sales Switch H2 =
SWITCH(
    TRUE(),
    [Total Sales] <> BLANK(), [Total Sales],
    [Average Daily Sales (H2 ALL SELECTED)]
)
```

**Forecast**

```DAX
Forecast =
CALCULATE(
    SUMX(
        VALUES('Forecast Date Table'[Date]),
        [Average Daily Sales Switch]
    ),
    FILTER(
        ALLSELECTED('Forecast Date Table'[Date]),
        'Forecast Date Table'[Date] <= MAX('Forecast Date Table'[Date])
    )
)
```

**Forecast 2**

```DAX
Forecast 2 =
CALCULATE(
    SUMX(
        VALUES('Forecast Date Table'[Date]),
        [Average Daily Sales Switch H2]
    ),
    FILTER(
        ALLSELECTED('Forecast Date Table'[Date]),
        'Forecast Date Table'[Date] <= MAX('Forecast Date Table'[Date])
    )
)
```

**Forecast Label**
```DAX
Forecast Label =
"2023 FY Forecast: " & FORMAT([Forecast], "$###,##0")
```

**Forecast Marker**
```DAX
Forecast Marker =
SWITCH(
    TRUE(),
    MAX('Forecast Date Table'[Month Num]) = 12, [Forecast],
    BLANK()
)
```
## _Nested_Measures
**Max AOS Marker**
```DAX
Max AOS Marker =
SWITCH(
    TRUE(),
    [Average Order Size] = [Max Avg Order Size (Hour)], [Average Order Size],
    BLANK()
)
```

**Max Avg Order Size (Hour)**
```DAX
Max Avg Order Size (Hour) =
MAXX(
    ALLSELECTED(Coffee_Sales_Fact[Hour]),
    [Average Order Size]
)
```


**Non-Peak Hour Sales**
```DAX
Non-Peak Hour Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        Coffee_Sales_Fact,
        Coffee_Sales_Fact[Hour] < 7 || Coffee_Sales_Fact[Hour] > 19
    )
)
```

**Non-Peak Hour Sales(%)**
```DAX
Non-Peak Hour Sales(%) =
DIVIDE([Non-Peak Hour Sales], [Total Sales], 0)
```

**Product Category Rank**
```DAX
Product Category Rank =
CALCULATE(
    RANKX(ALL(Products_Dim[product_type]), [Total Sales], , DESC, Dense),
    ALL(Products_Dim[product_category])
)
```




