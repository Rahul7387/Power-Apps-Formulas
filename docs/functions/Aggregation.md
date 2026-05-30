# 📈 Sum / Average / Min / Max

> **Category:** Aggregation | **Works in:** Canvas Apps

---

## Overview

Aggregate numeric or date column values across a table or filtered subset: Sum, Average, Min, Max.

---

## Syntax

```plaintext
Sum( Table, NumericFormula )
Average( Table, NumericFormula )
Min( Table, Column )
Max( Table, Column )
```

---

## Simple Examples

### 1. Cart total
```plaintext
Sum(varCart, Price * Qty)
```

### 2. Average order value
```plaintext
Average(Orders, TotalAmount)
```

### 3. Highest and lowest price
```plaintext
Max(Products, Price)
Min(Products, Price)
```

---

## Complex Examples

### 4. Full order summary calculation
```plaintext
Set(varSubtotal, Round(Sum(varCart, Price * Qty), 2));
Set(varTax,      Round(varSubtotal * 0.18, 2));
Set(varDiscount, If(varSubtotal >= 5000, Round(varSubtotal * 0.05, 2), 0));
Set(varTotal,    varSubtotal + varTax - varDiscount)
```

### 5. Regional KPI dashboard
```plaintext
Set(varKPI, {
    Revenue:   Sum(Filter(Sales, Region = ddlRegion.Selected.Value), Amount),
    AvgDeal:   Average(Filter(Sales, Region = ddlRegion.Selected.Value), Amount),
    TopDeal:   Max(Filter(Sales, Region = ddlRegion.Selected.Value), Amount),
    FirstSale: Min(Filter(Sales, Region = ddlRegion.Selected.Value), SaleDate)
})
```

### 6. Running total with AddColumns
```plaintext
ClearCollect(
    varRunning,
    AddColumns(
        Sort(varSales, SaleDate, SortOrder.Ascending),
        "RunningTotal",
        Sum(Filter(varSales, SaleDate <= ThisRecord.SaleDate), Amount)
    )
)
```

---

## Best Practices

- Validate inputs before using in write operations.
- Combine with related functions for complete workflows.
- Test with both empty and populated data sources.
- Consider delegation when working with large data sets.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Filter`](./Filter.md) | Related function |
| [`Round`](./Round.md) | Related function |
| [`Text`](./Text.md) | Related function |
| [`CountRows`](./CountRows.md) | Related function |

---

## 🔗 Official Documentation
[Sum, Min, Max, Average – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-numerics)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*