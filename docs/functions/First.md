# 1️⃣ First / Last

> **Category:** Data | **Works in:** Canvas Apps

---

## Overview

First returns the first record from a table; Last returns the final record. Commonly used after Filter or Sort to retrieve one specific row.

---

## Syntax

```plaintext
First( Table )
Last( Table )
```

---

## Simple Examples

### 1. First product in list
```plaintext
First(Products)
```

### 2. Latest order for user
```plaintext
First(Sort(Filter(Orders, CustomerEmail = User().Email), OrderDate, SortOrder.Descending))
```

### 3. Oldest open task
```plaintext
First(Sort(Filter(Tasks, Status = "Open"), DueDate, SortOrder.Ascending))
```

---

## Complex Examples

### 4. Safe First with IsEmpty guard
```plaintext
If(
    !IsEmpty(varSearchResults),
    Set(varTopResult, First(varSearchResults)),
    Notify("No results.", NotificationType.Warning)
)
```

### 5. Date range header from data
```plaintext
With(
    { sorted: Sort(varData, Date, SortOrder.Ascending) },
    Text(First(sorted).Date, "dd MMM") & " – " & Text(Last(sorted).Date, "dd MMM yyyy")
)
```

### 6. Get latest record after Patch (audit pattern)
```plaintext
Set(varNewRec, Patch(Events, Defaults(Events), { Title: txtTitle.Text, CreatedOn: Now() }));
// varNewRec IS the new record — equivalent to First(Sort(Events,CreatedOn,SortOrder.Descending))
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
| [`LookUp`](./LookUp.md) | Related function |
| [`IsEmpty`](./IsEmpty.md) | Related function |

---

## 🔗 Official Documentation
[First, Last – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-first-last)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*