# 📋 ShowColumns / DropColumns

> **Category:** Data Transform | **Works in:** Canvas Apps

---

## Overview

ShowColumns returns a table containing only the specified columns. DropColumns returns a table with specified columns removed.

---

## Syntax

```plaintext
ShowColumns( Table, "Col1" [, "Col2", ...] )
DropColumns( Table, "Col1" [, "Col2", ...] )
```

---

## Simple Examples

### 1. Show only needed columns
```plaintext
ShowColumns(Employees, "ID", "Name", "Email", "Department")
```

### 2. Drop sensitive columns
```plaintext
DropColumns(Employees, "Salary", "SSN", "PerformanceRating")
```

### 3. Gallery with reduced columns
```plaintext
// Gallery Items:
DropColumns(Filter(Employees, IsActive=true), "Salary", "HomeAddress")
```

---

## Complex Examples

### 4. Clean export collection
```plaintext
ClearCollect(
    varExportData,
    ShowColumns(
        Filter(Orders, Month(OrderDate) = Month(Today())),
        "OrderNumber", "CustomerName", "Total", "Status", "OrderDate"
    )
)
```

### 5. ForAll with ShowColumns to map tables
```plaintext
ForAll(
    ShowColumns(varSourceData, "ProductID", "Qty", "UnitPrice"),
    Patch(OrderLines, Defaults(OrderLines), {
        ProductID: ProductID,
        Qty:       Qty,
        UnitPrice: UnitPrice,
        LineTotal: Qty * UnitPrice
    })
)
```

### 6. Hide sensitive data for non-admin users
```plaintext
// Gallery Items:
If(
    varIsAdmin,
    Employees,
    DropColumns(Employees, "Salary", "BankAccount", "TaxID")
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
| [`AddColumns`](./AddColumns.md) | Related function |
| [`ForAll`](./ForAll.md) | Related function |
| [`Filter`](./Filter.md) | Related function |
| [`Patch`](./Patch.md) | Related function |

---

## 🔗 Official Documentation
[ShowColumns, DropColumns – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-table-shaping)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*