# 🔢 CountRows / CountIf

> **Category:** Aggregation | **Works in:** Canvas Apps

---

## Overview

CountRows returns the total number of rows in a table. CountIf counts rows that match a condition.

---

## Syntax

```plaintext
CountRows( Table )
CountIf( Table, Condition )
Count( Column )    // non-blank numeric
CountA( Column )   // non-blank any type
```

---

## Simple Examples

### 1. Total records
```plaintext
CountRows(Products)
```

### 2. Count matching rows
```plaintext
CountIf(Tasks, Status = "Open" && AssignedTo = User().Email)
```

### 3. Badge with cart count
```plaintext
Text(CountRows(varCart), "0") & " item(s)"
```

---

## Complex Examples

### 4. Dashboard KPI counters
```plaintext
Set(varStats, {
    Total:      CountRows(Filter(Tasks, AssignedTo = User().Email)),
    Open:       CountIf(Tasks, AssignedTo = User().Email && Status = "Open"),
    Overdue:    CountIf(Tasks, AssignedTo = User().Email && DueDate < Today()),
    DoneToday:  CountIf(Tasks, AssignedTo = User().Email && Status="Done" && DateDiff(CompletedDate, Today(), Days) = 0)
})
```

### 5. Pagination — page count
```plaintext
Set(varPageSize, 20);
Set(varTotalPages, RoundUp(CountRows(varFilteredData) / varPageSize, 0));
"Showing page " & varCurrentPage & " of " & varTotalPages
```

### 6. Unread notification badge
```plaintext
// App header badge Text:
CountIf(Notifications, RecipientEmail = User().Email && IsRead = false)
// Badge Visible:
CountIf(Notifications, RecipientEmail = User().Email && IsRead = false) > 0
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
| [`IsEmpty`](./IsEmpty.md) | Related function |
| [`Aggregation`](./Aggregation.md) | Related function |
| [`Sum`](./Sum.md) | Related function |

---

## 🔗 Official Documentation
[CountRows, CountIf – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-table-counts)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*