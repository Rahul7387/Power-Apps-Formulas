# 🔄 Refresh

> **Category:** Data | **Works in:** Canvas Apps

---

## Overview

Forces a reload of data from the connected data source, clearing the local Power Apps cache.

---

## Syntax

```plaintext
Refresh( DataSource )
```

---

## Simple Examples

### 1. Refresh a SharePoint list
```plaintext
Refresh(SharePointList)
```

### 2. Refresh after Patch
```plaintext
Patch(Tasks, Gallery1.Selected, { Status: "Done" });
Refresh(Tasks)
```

### 3. Refresh + recollect local cache
```plaintext
Refresh(Products);
ClearCollect(varProducts, Filter(Products, IsActive = true))
```

---

## Complex Examples

### 4. Stale-data check on screen visible
```plaintext
If(
    DateDiff(varLastRefresh, Now(), Minutes) > 5,
    Refresh(Projects);
    ClearCollect(varProjects, Projects);
    Set(varLastRefresh, Now())
)
```

### 5. Pull-to-refresh pattern
```plaintext
// Refresh icon button OnSelect
UpdateContext({ isRefreshing: true });
Refresh(Tasks);
ClearCollect(varMyTasks, Filter(Tasks, AssignedTo = User().Email));
UpdateContext({ isRefreshing: false });
Notify("Tasks updated.", NotificationType.Information)
```

### 6. After bulk update, refresh all related sources
```plaintext
ForAll(varSelected, Patch(Orders, ThisRecord, { Status: "Shipped" }));
Refresh(Orders);
Refresh(Inventory);
ClearCollect(varOrders, Filter(Orders, CustomerID = varCustomer.ID));
Notify("Orders shipped and inventory updated.", NotificationType.Success)
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
| [`Patch`](./Patch.md) | Related function |
| [`ClearCollect`](./ClearCollect.md) | Related function |
| [`Filter`](./Filter.md) | Related function |

---

## 🔗 Official Documentation
[Refresh – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-refresh)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*