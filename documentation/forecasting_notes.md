# Forecasting Methodology & Notes

## Overview

The **Forecasting & Recommendations** page extends the historical performance analysis by providing a forward-looking view of Global Superstore sales.

The purpose of the forecast is to:

- Visualize the historical sales trajectory
- Estimate the direction of future sales
- Provide a high-level projection for management
- Connect historical performance with business recommendations
- Identify limitations and opportunities for more advanced forecasting

The forecast should be interpreted as a **directional business planning tool rather than a production forecasting model**.

---

# Historical Data

The dashboard analyzes Global Superstore transactions from:

```text
2011 - 2014
```

Historical sales performance is displayed at both monthly and annual levels.

The monthly sales trend demonstrates fluctuations in sales throughout the historical period, while the overall trend indicates increasing sales over time.

---

# Historical Growth Rate

The Forecasting & Recommendations page displays a:

```text
Historical Growth Rate: 24.1%
```

This KPI summarizes the historical annualized growth in sales across the analysis period.

The metric provides context for the historical trajectory of the business before examining the forecast.

> The Historical Growth Rate is a historical KPI and should not be interpreted as the assumed future annual growth rate of the Power BI forecast.

---

# Forecast Horizon

The dashboard presents a forecast covering:

```text
2015 - 2019
```

The forecast extends the historical sales trend beyond the available 2011-2014 transaction data.

---

# Projected Sales

The forecast indicates projected sales of approximately:

```text
$7.26M
```

by the end of the forecast horizon.

The KPI displayed on the report is labeled:

```text
Projected Sales
$7.26M
Based on Power BI Trend Forecast
```

This value represents the approximate endpoint shown by the forecast visualization.

---

# Forecast Visualization

The forecasting page contains two complementary views.

## Historical Monthly Sales Trend

The monthly sales chart displays historical sales performance from 2011 through 2014.

The visual includes a trend line to highlight the overall direction of historical sales despite month-to-month fluctuations.

### Purpose

This visual answers:

> How did sales behave during the historical period?

---

## Annual Sales Forecast

The annual forecast chart combines historical sales with the forward-looking projection.

The visual displays:

- Historical annual sales
- Projected future sales
- Forecast confidence interval

### Purpose

This visual answers:

> If historical patterns continue, what direction might future sales take?

---

# Forecast Interpretation

The forecast indicates continued sales growth under the assumptions of the trend-based approach used in the report.

The forecast should not be interpreted as a guaranteed future result.

Instead, the forecast provides management with a directional estimate that can support:

- Business planning
- Scenario discussions
- Performance target development
- Additional forecasting analysis

---

# Forecast Limitations

The forecast has several important limitations.

## Historical Dependence

The projection is based on historical performance.

Changes in future business conditions may cause actual performance to differ from the historical pattern.

---

## Seasonality

The current forecast implementation is not intended to provide a detailed seasonal forecasting model.

More advanced analysis could explicitly investigate recurring monthly, quarterly, or annual patterns.

---

## External Business Conditions

The current forecast does not incorporate external variables such as:

- Economic conditions
- Competitive activity
- Pricing changes
- Supply chain changes
- Customer demand shifts
- Market expansion or contraction

These variables could materially affect future performance.

---

## Limited Historical Window

The available dataset covers:

```text
2011 - 2014
```

A longer historical period could provide additional information for evaluating long-term trends and recurring patterns.

---

# Business Recommendations

Forecasting was combined with findings from the other dashboard pages to develop management recommendations.

## High Priority: Copier Profitability

### Finding

Copiers generate the highest profitability among the analyzed subcategories.

### Recommendation

Evaluate opportunities to expand:

- Promotion
- Inventory allocation
- Product availability

while continuing to monitor profitability.

---

## High Priority: Tables Profitability

### Finding

The Tables subcategory demonstrates negative profit margin and multiple table products appear among the lowest-performing products by profit.

### Recommendation

Review:

- Pricing
- Discount strategy
- Shipping costs
- Product-level profitability

before pursuing additional sales growth.

---

## Medium Priority: High-Growth Markets

### Finding

Canada and Southeast Asia demonstrate strong profit growth in the regional performance analysis.

### Recommendation

Evaluate opportunities for additional investment in these markets, including:

- Customer acquisition
- Marketing
- Product availability
- Product mix

---

## Medium Priority: Technology

### Finding

Technology remains the strongest overall category in the dashboard.

### Recommendation

Maintain focus on Technology while continuing to evaluate individual product and subcategory profitability.

---

# Future Forecasting Enhancements

The current approach provides a useful baseline for business reporting.

Future versions of the project could evaluate more advanced forecasting techniques.

## ARIMA (AutoRegressive Integrated Moving Average)

ARIMA could be evaluated for statistical time-series forecasting using historical sales patterns.

---

## Prophet

Prophet could be evaluated as an alternative forecasting approach for modeling time-series trends and seasonal behavior.

---

## Seasonal Decomposition

Future analysis could explicitly separate:

```text
Trend
Seasonality
Residual Variation
```

to better understand recurring patterns in sales.

---

## External Economic Indicators

Future forecasting models could incorporate external variables that may influence sales performance.

Potential variables could include:

- Economic indicators
- Market conditions
- Pricing changes
- Business expansion
- Customer demand indicators

---

# Relationship to the Python Project

This Power BI project was developed as the BI evolution of the original **Superstore Reporting Automation Solution** built in Python.

The Python project also included forecasting and business recommendations.

The Power BI implementation translates the forecasting and decision-support concepts into an interactive Business Intelligence reporting environment.

This demonstrates how the same underlying business problem can be approached using different analytical technologies.

---

# Forecasting Page Summary

The final forecasting workflow can be summarized as:

```text
Historical Sales
       ↓
Historical Growth Analysis
       ↓
Trend Forecast
       ↓
Projected Sales
       ↓
Forecast Limitations
       ↓
Business Recommendations
       ↓
Future Modeling Opportunities
```

The objective is not simply to predict future sales, but to connect forward-looking analysis with the findings identified throughout the broader Business Intelligence solution.

---

# Related Documentation

- data_model.md
- dax_measures.md
- business_insights.md

---

## Navigation

[Go to root README](../README.md)
