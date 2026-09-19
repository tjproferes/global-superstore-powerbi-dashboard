# Data Model Documentation

## Overview

The Global Superstore Performance Dashboard uses a **star schema** to separate transactional sales data from descriptive business dimensions.

The model consists of one central fact table and four dimension tables:

- `Fact_Sales`
- `Dim_Date`
- `Dim_Customer`
- `Dim_Product`
- `Dim_Geography`

This structure supports filtering and analysis across time, customers, products, and geographic locations while keeping transactional measures centralized in the fact table.

---

## Data Model Diagram

![](images/data_model.png)

---

# Model Structure

```text
                     Dim_Date
                        |
                        |
Dim_Customer ------ Fact_Sales ------ Dim_Product
                        |
                        |
                  Dim_Geography
```

The `Fact_Sales` table contains transactional records and numeric measures.

Dimension tables contain descriptive attributes used for filtering, grouping, and report navigation.

---

# Fact Table

## Fact_Sales

`Fact_Sales` represents the transactional grain of the model.

Each row represents an individual sales/order-line transaction from the Global Superstore dataset.

### Key Columns

| Column | Purpose |
|---|---|
| Row ID | Identifies the individual transaction row |
| Order ID | Identifies the customer order |
| Order Date | Connects sales activity to `Dim_Date` |
| Ship Date | Stores the shipment date |
| Customer ID | Connects the transaction to `Dim_Customer` |
| Product ID | Connects the transaction to `Dim_Product` |
| GeographyKey | Connects the transaction to `Dim_Geography` |
| Order Priority | Describes the priority assigned to the order |
| Sales | Transaction sales amount |
| Profit | Transaction profit amount |
| Quantity | Number of units sold |
| Discount | Discount associated with the transaction |
| Shipping Cost | Shipping cost associated with the transaction |

### Fact Table Measures

The numeric fields in `Fact_Sales` support business calculations such as:

- Total Sales
- Total Profit
- Units Sold
- Total Orders
- Total Customers
- Profit Margin %
- Average Order Value
- Sales Growth %
- Profit Growth %

These KPIs are implemented as DAX measures rather than duplicated as calculated values in the fact table.

---

# Dimension Tables

## Dim_Customer

`Dim_Customer` contains descriptive customer attributes.

### Columns

| Column | Purpose |
|---|---|
| Customer ID | Customer key used to relate to `Fact_Sales` |
| Customer Name | Customer descriptive name |
| Segment | Customer business segment |

### Relationship

```text
Dim_Customer[Customer ID]
        1
        |
        *
Fact_Sales[Customer ID]
```

This relationship allows sales and profitability measures to be analyzed by customer and customer segment.

---

## Dim_Product

`Dim_Product` contains product hierarchy and descriptive product information.

### Columns

| Column | Purpose |
|---|---|
| Product ID | Product key used to relate to `Fact_Sales` |
| Product Name | Product descriptive name |
| Category | High-level product category |
| Sub-Category | Detailed product classification |

### Relationship

```text
Dim_Product[Product ID]
        1
        |
        *
Fact_Sales[Product ID]
```

This dimension supports analysis by:

- Product
- Category
- Sub-Category

It is used extensively within the Product & Category Analysis and Opportunities & Performance Monitoring report pages.

---

## Dim_Geography

`Dim_Geography` contains geographic attributes used for regional and market analysis.

### Columns

| Column | Purpose |
|---|---|
| GeographyKey | Surrogate key used to relate geography to `Fact_Sales` |
| Market | Global business market |
| Region | Reporting region |
| Country | Country |
| State | State or geographic subdivision |
| City | City |

### Geography Key

The original dataset did not contain a single geography identifier suitable for use as a dimension key.

A `GeographyKey` was therefore created during Power Query transformation.

The geography dimension was created using distinct combinations of:

```text
Market
Region
Country
State
City
```

An index was assigned to each unique geographic combination to create the surrogate `GeographyKey`.

The resulting key was then merged back into `Fact_Sales`.

### Relationship

```text
Dim_Geography[GeographyKey]
        1
        |
        *
Fact_Sales[GeographyKey]
```

This dimension supports analysis across:

- Markets
- Regions
