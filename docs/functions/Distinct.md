# 🎯 Distinct

> **Category:** Data | **Works in:** Canvas Apps

---

## Overview

Returns a one-column table of unique values from a specified column — ideal for building dropdown lists and counting unique entries.

---

## Syntax

```plaintext
Distinct( Table, Column )
```

---

## Simple Examples

### 1. Unique category list for dropdown
```plaintext
Distinct(Products, Category)
```

### 2. Sorted unique dropdown
```plaintext
Sort(Distinct(Products, Category), Value, SortOrder.Ascending)
```

### 3. Count unique customers
```plaintext
CountRows(Distinct(Orders, CustomerID))
```

---

## Complex Examples

### 4. Dropdown with 'All' option prepended
```plaintext
// Dropdown Items:
Table({Value: "All"}) /* combined with */ Distinct(Products, Category)
// Better pattern using ClearCollect:
ClearCollect(varCategoryOptions,
    [{Value: "All"}],
    Sort(Distinct(Products, Category), Value)
)
```

### 5. Category summary using Distinct + ForAll
```plaintext
ClearCollect(varCatSummary, []);
ForAll(
    Distinct(Products, Category),
    Collect(varCatSummary, {
        Category:    Value,
        ItemCount:   CountIf(Products, Category = Value),
        AvgPrice:    Average(Filter(Products, Category = Value), Price),
        TotalStock:  Sum(Filter(Products, Category = Value), StockQty)
    })
)
```

### 6. Unique month list for a timeline filter
```plaintext
Sort(
    Distinct(
        AddColumns(Orders, "MonthYear", Text(OrderDate, "[$-en-US]mmm yyyy")),
        MonthYear
    ),
    Value, SortOrder.Ascending
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
| [`Sort`](./Sort.md) | Related function |
| [`AddColumns`](./AddColumns.md) | Related function |
| [`CountRows`](./CountRows.md) | Related function |

---

## 🔗 Official Documentation
[Distinct – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-distinct)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*