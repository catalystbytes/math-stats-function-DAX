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

## 🧩 Example 1: Basic Aggregations – One-Page DAX

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
