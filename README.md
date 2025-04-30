# 📊 DAX Function Examples for `computer_store_inc` Data Model

This repository provides practical and well-documented **DAX examples** based on the `computer_store_inc` schema. The focus is on **Math & Statistical Functions**, both basic aggregations and iterator functions. These are useful for creating measures, matrix tables, and performance analysis in **Power BI**.

---

## 📁 Dataset Schema Overview

These examples are based on the following key tables from the model:

- `sales_fact`
- `product_dim`
- `customer_dim`
- `store_dim`
- `salesperson_dim`

For full schema details, see your data model.

---

## 📘 Covered DAX Categories

### ✅ Basic Aggregation Functions
- `SUM`, `AVERAGE`, `MAX`, `MIN`
- `DIVIDE`, `COUNT`, `COUNTA`, `COUNTROWS`, `DISTINCTCOUNT`

### ✅ Iterator Functions
- `SUMX`, `AVERAGEX`, `MAXX`, `MINX`, `RANKX`, `COUNTX`

---

## Example 1:  One DAX Measure to Show All Basic Aggregations

This DAX code creates a calculated table displaying essential metrics using common aggregation functions. Paste this into **Modeling > New Table** in Power BI to use in a Matrix or Table visual.

```dax
Sales Summary Table = 
UNION(
    SELECTCOLUMNS(
        {"Total Units Sold"},
        "Metric", "Total Units Sold",
        "Value", SUM('sales_fact'[units_sold])
    ),
    SELECTCOLUMNS(
        {"Average Unit Price"},
        "Metric", "Average Unit Price",
        "Value", 
        DIVIDE(
            SUM('sales_fact'[total_amount]), 
            SUM('sales_fact'[units_sold])
        )
    ),
    SELECTCOLUMNS(
        {"Max Sale Amount"},
        "Metric", "Max Sale Amount",
        "Value", MAX('sales_fact'[total_amount])
    ),
    SELECTCOLUMNS(
        {"Min Sale Amount"},
        "Metric", "Min Sale Amount",
        "Value", MIN('sales_fact'[total_amount])
    ),
    SELECTCOLUMNS(
        {"Customer Count"},
        "Metric", "Customer Count",
        "Value", COUNT('sales_fact'[customer_id])
    ),
    SELECTCOLUMNS(
        {"Non-Blank Customer Count"},
        "Metric", "Non-Blank Customer Count",
        "Value", COUNTA('sales_fact'[customer_id])
    ),
    SELECTCOLUMNS(
        {"Row Count"},
        "Metric", "Row Count",
        "Value", COUNTROWS('sales_fact')
    ),
    SELECTCOLUMNS(
        {"Unique Customer Count"},
        "Metric", "Unique Customer Count",
        "Value", DISTINCTCOUNT('sales_fact'[customer_id])
    )
)
``` 

## Example 2: Iterator-Based Sales Summary Table

```dax
Iterator Sales Summary = 
UNION(
    SELECTCOLUMNS(
        {"Total Revenue Per Product"},
        "Metric", "Total Revenue Per Product",
        "Value", 
        SUMX(
            VALUES('product_dim'[product_id]),
            CALCULATE(SUM('sales_fact'[total_amount]))
        )
    ),
    SELECTCOLUMNS(
        {"Average Revenue Per Store"},
        "Metric", "Average Revenue Per Store",
        "Value", 
        AVERAGEX(
            VALUES('store_dim'[store_id]),
            CALCULATE(SUM('sales_fact'[total_amount]))
        )
    ),
    SELECTCOLUMNS(
        {"Max Units Sold Per Customer"},
        "Metric", "Max Units Sold Per Customer",
        "Value", 
        MAXX(
            VALUES('customer_dim'[customer_id]),
            CALCULATE(SUM('sales_fact'[units_sold]))
        )
    ),
    SELECTCOLUMNS(
        {"Min Revenue Per Customer Type"},
        "Metric", "Min Revenue Per Customer Type",
        "Value", 
        MINX(
            VALUES('customer_dim'[customer_type]),
            CALCULATE(SUM('sales_fact'[total_amount]))
        )
    )
)

```

## Example 2: Ranking and Counting Summary Table

```dax
Rank and Count Summary = 
UNION(
    SELECTCOLUMNS(
        {"Sales Rank by Revenue"},
        "Metric", "Top Salesperson Rank",
        "Value", 
        RANKX(
            ALL('salesperson_dim'[salesperson_id]),
            CALCULATE(SUM('sales_fact'[total_amount])),
            ,
            DESC
        )
    ),
    SELECTCOLUMNS(
        {"Sales Count Per Region"},
        "Metric", "Sales Count Per Region (Total)",
        "Value", 
        COUNTX(
            VALUES('customer_dim'[region]),
            CALCULATE(DISTINCTCOUNT('sales_fact'[sale_id]))
        )
    )
)

```
