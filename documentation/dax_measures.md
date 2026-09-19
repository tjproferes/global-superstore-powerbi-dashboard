# DAX Measures Documentation

## Overview

This document contains the primary DAX measures used in the **Global Superstore Performance Dashboard**.

Measures were designed as reusable business calculations so the same KPI logic could be used consistently across all report pages.

The primary transaction table used by these measures is:

```text
Fact_Sales
```

The model also uses:

```text
Dim_Date
Dim_Customer
Dim_Product
Dim_Geography
```

---

# Executive KPI Measures

## Total Sales

**Purpose:**  
Calculates total sales revenue within the current filter context.

```DAX
Total Sales =
SUM(Fact_Sales[Sales])
```

**Used for:**

- Executive KPI reporting
- Regional analysis
- Product analysis
- Sales trends
- Forecasting

---

## Total Profit

**Purpose:**  
Calculates total profit within the current filter context.

```DAX
Total Profit =
SUM(Fact_Sales[Profit])
```

**Used for:**

- Executive KPI reporting
- Regional profitability analysis
- Product profitability analysis
- Performance monitoring

---

## Total Orders

**Purpose:**  
Calculates the number of unique customer orders.

```DAX
Total Orders =
DISTINCTCOUNT(Fact_Sales[Order ID])
```

Using `DISTINCTCOUNT` prevents orders containing multiple product lines from being counted multiple times.

---

## Total Customers

**Purpose:**  
Calculates the number of unique customers represented by the current report filters.

```DAX
Total Customers =
DISTINCTCOUNT(Fact_Sales[Customer ID])
```

Using the customer identifier from `Fact_Sales` allows the measure to respond dynamically to filters such as:

- Year
- Region
- Category
- Product

---

## Units Sold

**Purpose:**  
Calculates the total quantity of products sold.

```DAX
Units Sold =
SUM(Fact_Sales[Quantity])
```

---

## Profit Margin %

**Purpose:**  
Measures the percentage of sales retained as profit.

```DAX
Profit Margin % =
DIVIDE(
    [Total Profit],
    [Total Sales]
)
```

Using `DIVIDE()` provides safe handling when Total Sales is zero or blank.

**Format:** Percentage

---

## Average Order Value

**Purpose:**  
Calculates average sales generated per unique order.

```DAX
Average Order Value =
DIVIDE(
    [Total Sales],
    [Total Orders]
)
```

**Format:** Currency

---

# Growth Measures

## Previous Month Sales

**Purpose:**  
Calculates Total Sales for the previous month.

```DAX
Previous Month Sales =
CALCULATE(
    [Total Sales],
    DATEADD(
        Dim_Date[Date],
        -1,
        MONTH
    )
)
```

---

## Total Sales MoM %

**Purpose:**  
Calculates month-over-month sales growth.

```DAX
Total Sales MoM % =
VAR PreviousSales =
    [Previous Month Sales]

RETURN
    DIVIDE(
        [Total Sales] - PreviousSales,
        PreviousSales
    )
```

**Interpretation:**

```text
Positive value = Sales increased
Negative value = Sales declined
```

**Format:** Percentage

---

## Previous Month Profit

**Purpose:**  
Calculates Total Profit for the previous month.

```DAX
Previous Month Profit =
CALCULATE(
    [Total Profit],
    DATEADD(
        Dim_Date[Date],
        -1,
        MONTH
    )
)
```

---

## Total Profit MoM %

**Purpose:**  
Calculates month-over-month profit growth.

```DAX
Total Profit MoM % =
VAR PreviousProfit =
    [Previous Month Profit]

RETURN
    DIVIDE(
        [Total Profit] - PreviousProfit,
        PreviousProfit
    )
```

**Interpretation:**

```text
Positive value = Profit increased
Negative value = Profit declined
```

**Format:** Percentage

---

# Performance Monitoring

## Performance Status

**Purpose:**  
Classifies performance according to profit growth.

Example implementation:

```DAX
Performance Status =
SWITCH(
    TRUE(),
    [Total Profit MoM %] >= 0.05, "Strong Growth",
    [Total Profit MoM %] >= 0, "Stable",
    "Decline"
)
```

### Classification Framework

| Status | Rule |
|---|---|
| Strong Growth | Profit growth ≥ 5% |
| Stable | Profit growth ≥ 0% and < 5% |
| Decline | Profit growth < 0% |

This measure supports the **Opportunities & Performance Monitoring** page by converting a numeric growth measure into an executive-friendly performance classification.

---

# Historical Growth

## Historical Growth Rate

**Purpose:**  
Measures annualized historical sales growth between the beginning and end of the analysis period.

```DAX
Historical Growth Rate =
VAR FirstYearSales =
    CALCULATE(
        [Total Sales],
        FILTER(
            ALL(Dim_Date),
            Dim_Date[Year] = 2011
        )
    )

VAR LastYearSales =
    CALCULATE(
        [Total Sales],
        FILTER(
            ALL(Dim_Date),
            Dim_Date[Year] = 2014
        )
    )

RETURN
    POWER(
        DIVIDE(
            LastYearSales,
            FirstYearSales
        ),
        1 / 3
    ) - 1
```

**Format:** Percentage

This measure is displayed on the **Forecasting & Recommendations** page as the Historical Growth Rate KPI.

---

# Dynamic Report Titles

Dynamic title measures allow report titles to respond to the user's filter selections.

## Sales Trend Title

Example:

```DAX
Sales Trend Title =
"Sales Trend ("
    & MIN(Dim_Date[Year])
    & " - "
    & MAX(Dim_Date[Year])
    & ")"
```

Example output:

```text
Sales Trend (2011 - 2014)
```

---

## Profit Trend Title

```DAX
Profit Trend Title =
"Profit Trend ("
    & MIN(Dim_Date[Year])
    & " - "
    & MAX(Dim_Date[Year])
    & ")"
```

Example output:

```text
Profit Trend (2011 - 2014)
```

---

# Conditional Formatting

Conditional formatting measures were used to make performance indicators easier to interpret.

## Sales Growth Color

Example:

```DAX
Sales Growth Color =
IF(
    [Total Sales MoM %] >= 0,
    "#008000",
    "#C00000"
)
```

This allows positive growth to appear green and negative growth to appear red.

---

## Profit Growth Color

Example:

```DAX
Profit Growth Color =
IF(
    [Total Profit MoM %] >= 0,
    "#008000",
    "#C00000"
)
```

---

# Forecasting Measures

The forecast displayed in the report uses the Power BI trend forecasting functionality rather than a custom DAX forecasting model.

The Forecasting & Recommendations page displays:

```text
Historical Growth Rate
Forecast Horizon
Projected Sales
```

The projected sales KPI represents the endpoint displayed by the forecast and should therefore be interpreted together with the forecasting assumptions and limitations documented in:

```text
forecasting_notes.md
```

---

# Measure Design Principles

The DAX layer was designed around several principles.

## Reusability

Base measures such as:

```DAX
[Total Sales]
[Total Profit]
[Total Orders]
[Total Customers]
```

are reused inside more advanced calculations rather than repeatedly recalculating the same business logic.

For example:

```DAX
Profit Margin % =
DIVIDE(
    [Total Profit],
    [Total Sales]
)
```

---

## Filter Context

Measures were designed to respond dynamically to filters from the dimension tables.

Examples include:

```text
Dim_Date
Dim_Customer
Dim_Product
Dim_Geography
```

This allows the same measures to calculate results for different:

- Years
- Regions
- Markets
- Categories
- Sub-Categories
- Products
- Customer segments

---

## Measure-First KPI Design

Business KPIs were implemented as measures rather than static calculated columns.

This allows KPI values to respond dynamically to report filters, slicers, and visual context.

---

# Measures Used Across Report Pages

## Executive Summary

```text
Total Sales
Total Profit
Total Orders
Total Customers
Units Sold
Profit Margin %
Total Sales MoM %
Total Profit MoM %
```

## Regional Performance

```text
Total Sales
Total Profit
Profit Margin %
Total Sales MoM %
Total Profit MoM %
```

## Product & Category Analysis

```text
Total Sales
Total Profit
Profit Margin %
```

## Opportunities & Performance Monitoring

```text
Total Sales
Total Profit
Profit Margin %
Total Profit MoM %
Performance Status
```

## Forecasting & Recommendations

```text
Total Sales
Historical Growth Rate
```

---

# Related Documentation

- data_model.md
- business_insights.md
- forecasting_notes.md

---

[Go to root README](../README.md)
