# 🔄 ClearCollect

> **Category:** Collections | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## Overview

`ClearCollect` **clears all existing records** from a collection and then **adds new records** to it — equivalent to `Clear(col); Collect(col, data)` in one step. Use it whenever you want a fresh snapshot rather than appending.

---

## Syntax

```plaintext
ClearCollect( CollectionName, Record_or_Table [, ...] )
```

---

## Simple Examples

### 1. Load a data source into a collection
```plaintext
ClearCollect(varProducts, Products)
```

### 2. Filtered load
```plaintext
ClearCollect(varMyTasks, Filter(Tasks, AssignedTo = User().Email && Status <> "Closed"))
```

### 3. Reset a cart
```plaintext
ClearCollect(varCart, [])
```

### 4. Load static options
```plaintext
ClearCollect(varPriorityOptions,
    { Value: "High",   Colour: "Red"   },
    { Value: "Medium", Colour: "Orange"},
    { Value: "Low",    Colour: "Green" }
)
```

### 5. Refresh after Patch
```plaintext
Patch(Tasks, Defaults(Tasks), { Title: txtTitle.Text });
ClearCollect(varMyTasks, Filter(Tasks, AssignedTo = User().Email))
```

---

## Complex Examples

### 6. Multi-source merge into one collection
```plaintext
// App OnStart — combine two SharePoint lists into one local table
ClearCollect(varAllWork,
    AddColumns(Filter(Tasks,    AssignedTo = User().Email), "Source", "Task"),
    AddColumns(Filter(Projects, OwnerEmail  = User().Email), "Source", "Project")
)
```

### 7. Paginated load — fill collection in chunks
```plaintext
ClearCollect(varData, FirstN(BigList, 500));
If(CountRows(BigList) > 500,
    Collect(varData, LastN(BigList, CountRows(BigList) - 500))
)
```

### 8. Re-populate after delete
```plaintext
Remove(Products, Gallery_Products.Selected);
ClearCollect(varProducts, Filter(Products, IsActive = true));
Notify("Product deleted.", NotificationType.Success)
```

### 9. Enriched collection with AddColumns
```plaintext
ClearCollect(
    varEnrichedOrders,
    AddColumns(
        Filter(Orders, Status = "Pending"),
        "DaysOpen",    DateDiff(OrderDate, Today(), Days),
        "IsOverdue",   DueDate < Today(),
        "CustomerName",LookUp(Customers, ID = CustomerID).CompanyName
    )
)
```

### 10. Staged edit — copy record to collection for in-memory editing
```plaintext
// Edit button OnSelect
ClearCollect(varEditBuffer, Gallery1.Selected);
Navigate(EditScreen, ScreenTransition.Cover)

// On EditScreen — user edits varEditBuffer record in form
// Save button:
Patch(Projects, LookUp(Projects, ID = First(varEditBuffer).ID), First(varEditBuffer));
ClearCollect(varEditBuffer, []);
Navigate(ListScreen, ScreenTransition.UnCover)
```

---

## Best Practices

1. **Use in `App.OnStart`** for reference data (categories, statuses, lookup lists).
2. **Always `ClearCollect` before re-loading** — prevents duplicate rows building up.
3. **Combine with `AddColumns`** to enrich data without changing the source.
4. **Keep collections under 2000 rows** for good performance on mobile.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Collect`](./Collect.md) | Append without clearing |
| [`Remove`](./Remove.md) | Delete from collection |
| [`AddColumns`](./AddColumns.md) | Enrich data before collecting |

---

## 🔗 Official Documentation
[Collect and ClearCollect – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-clear-collect-clearcollect)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*