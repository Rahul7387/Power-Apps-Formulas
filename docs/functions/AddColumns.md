# ➕ AddColumns

> **Category:** Data Transform | **Works in:** Canvas Apps

---

## Overview

Returns a copy of a table with one or more new calculated columns added, without modifying the underlying data source.

---

## Syntax

```plaintext
AddColumns( Table, "ColName", Formula [, "ColName2", Formula2, ...] )
```

---

## Simple Examples

### 1. Add a computed total column
```plaintext
AddColumns(OrderItems, "LineTotal", Qty * UnitPrice)
```

### 2. Add a full name column
```plaintext
AddColumns(Employees, "FullName", FirstName & " " & LastName)
```

### 3. Add a boolean flag
```plaintext
AddColumns(Tasks, "IsOverdue", DueDate < Today())
```

---

## Complex Examples

### 4. Multi-column enrichment with cache
```plaintext
ClearCollect(
    varEnrichedOrders,
    AddColumns(
        Filter(Orders, Status = "Pending"),
        "CustomerName",  LookUp(Customers, ID = CustomerID).Name,
        "DaysOpen",      DateDiff(OrderDate, Today(), Days),
        "IsOverdue",     DueDate < Today(),
        "StatusBadge",   If(DueDate < Today(), "🔴 Overdue", "🟢 On Track"),
        "TaxAmount",     Round(Total * 0.18, 2)
    )
)
```

### 5. Custom sort key for priority ordering
```plaintext
Sort(
    AddColumns(Tasks,
        "PriorityNum", Switch(Priority, "Critical",1, "High",2, "Medium",3, 4)
    ),
    PriorityNum, SortOrder.Ascending,
    DueDate,     SortOrder.Ascending
)
```

### 6. Prepare data for ForAll bulk save
```plaintext
ForAll(
    AddColumns(
        Filter(varSelectedItems, IsSelected = true),
        "Status",     "Approved",
        "ApprovedBy", User().Email,
        "ApprovedOn", Now()
    ),
    Patch(Requests, LookUp(Requests, ID = ID), {
        Status:     Status,
        ApprovedBy: ApprovedBy,
        ApprovedOn: ApprovedOn
    })
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
| [`ForAll`](./ForAll.md) | Related function |
| [`GroupBy`](./GroupBy.md) | Related function |
| [`ClearCollect`](./ClearCollect.md) | Related function |

---

## 🔗 Official Documentation
[AddColumns, DropColumns, ShowColumns – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-table-shaping)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*