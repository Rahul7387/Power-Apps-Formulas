# 📊 GroupBy / Ungroup

> **Category:** Data Transform | **Works in:** Canvas Apps

---

## Overview

GroupBy groups rows of a table by one or more column values, placing the matching rows in a new subtable column. Ungroup reverses the grouping.

---

## Syntax

```plaintext
GroupBy( Table, "Column1" [, "Column2"], "GroupColumnName" )
Ungroup( Table, "GroupColumnName" )
```

---

## Simple Examples

### 1. Group products by category
```plaintext
GroupBy(Products, "Category", "CategoryItems")
```

### 2. Group sales by region
```plaintext
GroupBy(Sales, "Region", "RegionSales")
```

### 3. Flatten a grouping back
```plaintext
Ungroup(GroupBy(Products, "Category", "Items"), "Items")
```

---

## Complex Examples

### 4. Nested gallery — categories with items
```plaintext
// Outer gallery Items:
GroupBy(varProducts, "Category", "CategoryItems")
// Outer label: ThisItem.Category
// Inner gallery Items: ThisItem.CategoryItems
// Inner label: ThisItem.ProductName & " — " & Text(ThisItem.Price,"$#,##0")
```

### 5. Group + aggregate summary
```plaintext
ClearCollect(
    varRegionSummary,
    AddColumns(
        GroupBy(Sales, "Region", "RegionData"),
        "Revenue",    Sum(RegionData, Amount),
        "OrderCount", CountRows(RegionData),
        "AvgOrder",   Average(RegionData, Amount)
    )
)
```

### 6. Group by month for a timeline
```plaintext
ClearCollect(
    varMonthlyTasks,
    AddColumns(
        GroupBy(
            AddColumns(Tasks, "MonthYear",
                Text(DueDate, "[$-en-US]mmm yyyy")),
            "MonthYear", "MonthTasks"
        ),
        "TaskCount", CountRows(MonthTasks),
        "Overdue",   CountIf(MonthTasks, DueDate < Today())
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
| [`AddColumns`](./AddColumns.md) | Related function |
| [`ForAll`](./ForAll.md) | Related function |
| [`Distinct`](./Distinct.md) | Related function |
| [`ClearCollect`](./ClearCollect.md) | Related function |

---

## 🔗 Official Documentation
[GroupBy and Ungroup – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-groupby)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*