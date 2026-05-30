# 🔍 Filter

> **Category:** Data Read | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## 📋 Table of Contents
- [Overview](#overview)
- [Syntax](#syntax)
- [Parameters](#parameters)
- [Simple Examples](#simple-examples)
- [Complex Examples](#complex-examples)
- [Delegation](#delegation)
- [Filter vs Search vs LookUp](#filter-vs-search-vs-lookup)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)
- [Related Functions](#related-functions)

---

## Overview

`Filter` returns a **table containing only the rows that satisfy one or more conditions**. It is the primary function for showing a subset of data — filtering a gallery by status, user, date range, or any combination of columns.

```
Full Table                    Filter Result
──────────────────            ──────────────────
ID  Name     Status           ID  Name     Status
1   Widget A Active    ──►    1   Widget A Active
2   Widget B Inactive         3   Widget C Active
3   Widget C Active
4   Widget D Inactive
```

---

## Syntax

```plaintext
Filter( Table, Condition1 [, Condition2, ... ] )
```

Multiple conditions are combined with **AND** (all must be true).
Use `||` (OR) inside a single condition for OR logic.

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `Table` | Table / Data Source | ✅ Yes | The data source or table to filter |
| `Condition` | Boolean Formula | ✅ Yes | Expression that evaluates to `true` or `false` per row |

---

## Simple Examples

### 1. Show only active records
```plaintext
// Gallery Items property
Filter(Products, Status = "Active")
```

### 2. Filter by current user
```plaintext
Filter(Tasks, AssignedTo = User().Email)
```

### 3. Filter by date (records from last 30 days)
```plaintext
Filter(Orders, OrderDate >= DateAdd(Today(), -30, Days))
```

### 4. Multiple AND conditions
```plaintext
Filter(Invoices, Status = "Unpaid" && DueDate < Today())
```

### 5. Filter with StartsWith (delegable)
```plaintext
// Gallery Items — live search
Filter(Customers, StartsWith(Name, txtSearch.Text))
```

---

## Complex Examples

### 6. Combined search input + dropdown filter
```plaintext
// Gallery Items property — handles blank search and "All" category
Filter(
    Products,
    (IsBlank(txtSearch.Text) || StartsWith(Name, txtSearch.Text)) &&
    (ddlCategory.Selected.Value = "All" || Category = ddlCategory.Selected.Value) &&
    (tglActiveOnly.Value = false || Status = "Active")
)
```

### 7. Date range filter with two date pickers
```plaintext
Filter(
    SalesOrders,
    OrderDate >= dpStartDate.SelectedDate &&
    OrderDate <= dpEndDate.SelectedDate &&
    (IsBlank(ddlRep.Selected.Value) || SalesRep = ddlRep.Selected.Value)
)
```

### 8. Filter then aggregate
```plaintext
// KPI card — total revenue for selected region
Sum(
    Filter(Sales, Region = ddlRegion.Selected.Value && Year(SaleDate) = Year(Today())),
    Revenue
)

// Count open tickets assigned to current user
CountRows(Filter(Tickets, AssignedTo = User().Email && Status <> "Closed"))
```

### 9. Multi-value filter using In operator
```plaintext
// Show records where status is any of a user-selected set
// varSelectedStatuses is a collection: [{Value:"Open"},{Value:"Pending"}]
Filter(
    Tickets,
    Status in varSelectedStatuses.Value
)
```

### 10. Nested Filter inside ClearCollect for offline use
```plaintext
// App OnStart — cache user's data locally for performance
ClearCollect(
    varMyTasks,
    Filter(
        Tasks,
        AssignedTo = User().Email &&
        Status <> "Archived" &&
        DueDate >= Today() - 7
    )
);
ClearCollect(
    varMyProjects,
    Filter(Projects, OwnerEmail = User().Email || TeamEmails in User().Email)
)
```

### 11. Filter across a relationship (Dataverse)
```plaintext
// Show orders belonging to the currently selected customer
Filter(
    Orders,
    Customer.Email = varSelectedCustomer.Email &&
    OrderStatus in ["Pending", "Processing"]
)
```

### 12. Performance-optimised Filter + Sort + Search chain
```plaintext
// Gallery Items — filter first (delegable), then sort
Sort(
    Filter(
        Employees,
        Department = ddlDept.Selected.Value &&
        IsActive = true
    ),
    LastName,
    SortOrder.Ascending
)
```

---

## Delegation

Delegation determines whether the condition is evaluated **on the server** (handles all rows) or **on the device** (capped at 500–2000 rows).

| Operator / Function | SharePoint | Dataverse | SQL |
|--------------------|------------|-----------|-----|
| `=`, `<>`, `<`, `>`, `<=`, `>=` | ✅ | ✅ | ✅ |
| `StartsWith` | ✅ | ✅ | ✅ |
| `EndsWith` | ❌ | ✅ | ✅ |
| `In` (membership) | ⚠️ Partial | ✅ | ✅ |
| `Search` | ❌ | ❌ | ❌ |
| `IsBlank` | ✅ | ✅ | ✅ |
| `And` / `Or` | ✅ | ✅ | ✅ |

> ⚠️ A yellow triangle in Power Apps Studio means the condition is **not delegable**. Results will be capped. Fix by using `StartsWith` instead of `Search`, or load data into a collection first.

---

## Filter vs Search vs LookUp

| Feature | `Filter` | `Search` | `LookUp` |
|---------|----------|----------|----------|
| Returns | Table (0+ rows) | Table (0+ rows) | Single record |
| Column types | Any | Text only | Any |
| Delegable | Mostly yes | ❌ No | Mostly yes |
| Use for gallery | ✅ | ✅ (small data) | ❌ |
| Use for one value | Via `First(Filter(...))` | ❌ | ✅ |

---

## Best Practices

1. **Filter before Sort** — filtering reduces rows first, making sort faster.
2. **Use `StartsWith` instead of `Search`** for delegable live-search on large lists.
3. **Cache in a collection** with `ClearCollect` when you need non-delegable operations.
4. **Guard against blank search inputs** with `IsBlank(txtSearch.Text) ||` — so an empty search shows all records.
5. **Avoid chaining multiple non-delegable functions** — each non-delegable step truncates at the row limit.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Using `Search` inside `Filter` on SharePoint with 5000+ rows | Results capped at 500 | Use `StartsWith` or load to collection |
| `Filter(DS, Col = "")` to find blanks | Misses true blank/null values | Use `IsBlank(Col)` |
| No guard for blank dropdown | Filter returns 0 rows when "All" is selected | Add `ddl.Selected.Value = "All" \|\|` condition |
| Filtering on a calculated column | Non-delegable | Filter on a real source column; calculate after |

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Search`](./Search.md) | Text-based search across columns (non-delegable) |
| [`LookUp`](./LookUp.md) | Return a single matching record |
| [`Sort`](./Sort.md) | Sort the result of `Filter` |
| [`CountIf`](./CountRows.md) | Count filtered rows |
| [`ClearCollect`](./ClearCollect.md) | Cache filtered results locally |
| [`IsBlank`](./IsBlank.md) | Use inside filter conditions |

---

## 🔗 Official Documentation
[Filter, Search, LookUp – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-filter-lookup)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*
