# 🔃 Sort / SortByColumns

> **Category:** Data | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## Overview

`Sort` returns a copy of a table ordered by a single column. `SortByColumns` allows sorting by **multiple columns** simultaneously, with independent sort directions per column.

---

## Syntax

```plaintext
Sort( Table, Column [, SortOrder.Ascending | SortOrder.Descending] )

SortByColumns( Table, "Column1" [, SortOrder1 [, "Column2", SortOrder2, ...]] )
```

---

## Simple Examples

### 1. Sort A to Z
```plaintext
Sort(Products, Name, SortOrder.Ascending)
```

### 2. Sort newest first
```plaintext
Sort(Orders, CreatedDate, SortOrder.Descending)
```

### 3. Dynamic sort direction toggle
```plaintext
Sort(Tasks, Title, If(varSortAsc, SortOrder.Ascending, SortOrder.Descending))
```

### 4. Sort by multiple columns
```plaintext
SortByColumns(Employees, "Department", SortOrder.Ascending, "LastName", SortOrder.Ascending)
```

### 5. Sort a collection
```plaintext
Sort(varCart, Price, SortOrder.Descending)
```

---

## Complex Examples

### 6. User-controlled column sort (click header to toggle)
```plaintext
// Column header button OnSelect
If(
    varSortCol = "Name",
    Set(varSortAsc, !varSortAsc),
    Set(varSortCol, "Name"); Set(varSortAsc, true)
)

// Gallery Items
Sort(
    Filter(Products, IsActive = true),
    Switch(varSortCol,
        "Name",  Name,
        "Price", Price,
        "Date",  CreatedDate,
        Name
    ),
    If(varSortAsc, SortOrder.Ascending, SortOrder.Descending)
)
```

### 7. Sort then filter chain
```plaintext
// Gallery Items — filter first (delegable), then sort
Sort(
    Filter(
        Tasks,
        AssignedTo = User().Email &&
        (IsBlank(txtSearch.Text) || StartsWith(Title, txtSearch.Text))
    ),
    DueDate,
    SortOrder.Ascending
)
```

### 8. Priority sort (custom order)
```plaintext
// Sort by a custom priority order: Critical > High > Medium > Low
SortByColumns(
    AddColumns(Tasks, "PriorityOrder",
        Switch(Priority, "Critical", 1, "High", 2, "Medium", 3, "Low", 4, 99)
    ),
    "PriorityOrder", SortOrder.Ascending,
    "DueDate",       SortOrder.Ascending
)
```

### 9. Rank leaderboard
```plaintext
// Leaderboard gallery Items
AddColumns(
    Sort(SalesReps, TotalRevenue, SortOrder.Descending),
    "Rank", CountRows(Filter(SalesReps, TotalRevenue > ThisRecord.TotalRevenue)) + 1
)
```

### 10. Sort dropdown list
```plaintext
// Dropdown Items — sorted unique categories
Sort(Distinct(Products, Category), Value, SortOrder.Ascending)
```

---

## Best Practices

1. **Filter before Sort** — reduce rows first for better performance.
2. **Use `SortByColumns` for multi-column sorts** — cleaner than nested `Sort`.
3. **Add `AddColumns` for custom sort keys** (priority tiers, numeric rank).
4. **`Sort` is delegable on most data sources** for indexed columns.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Filter`](./Filter.md) | Reduce rows before sorting |
| [`Search`](./Search.md) | Search + sort pipeline |
| [`AddColumns`](./AddColumns.md) | Add computed sort key |
| [`Distinct`](./Distinct.md) | Sort unique values for dropdowns |

---

## 🔗 Official Documentation
[Sort and SortByColumns – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-sort)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*