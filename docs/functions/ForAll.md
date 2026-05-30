# 🔁 ForAll

> **Category:** Iteration | **Works in:** Canvas Apps

---

## Overview

Iterates over each row in a table and runs a formula — used for bulk operations, bulk saves, and row-by-row transformations.

---

## Syntax

```plaintext
ForAll( Table, Formula )
```

---

## Simple Examples

### 1. Bulk status update
```plaintext
ForAll(Filter(Tasks, DueDate < Today()), Patch(Tasks, ThisRecord, { Status: "Overdue" }))
```

### 2. Collect from each row
```plaintext
ForAll(varEmployees, Collect(varEmailList, { Email: Email }))
```

### 3. Bulk Patch to data source
```plaintext
ForAll(varCart, Patch(OrderLines, Defaults(OrderLines), { ProductID: ProductID, Qty: Qty, UnitPrice: UnitPrice }))
```

---

## Complex Examples

### 4. Multi-table order save
```plaintext
Set(varHeader, Patch(Orders, Defaults(Orders), { CustomerID: varCustomer.ID, OrderDate: Today() }));
ForAll(varCart,
    Patch(OrderLines, Defaults(OrderLines), {
        OrderID:   varHeader.ID,
        ProductID: ProductID,
        Qty:       Qty,
        Price:     UnitPrice
    })
);
Notify("Order placed!", NotificationType.Success)
```

### 5. Build enriched summary collection
```plaintext
ClearCollect(varSummary, []);
ForAll(
    Distinct(varOrders, CustomerID),
    Collect(varSummary, {
        CustomerID:   Value,
        TotalOrders:  CountIf(varOrders, CustomerID = Value),
        TotalRevenue: Sum(Filter(varOrders, CustomerID = Value), Amount)
    })
)
```

### 6. Bulk approve with notification
```plaintext
ForAll(
    Filter(varPending, IsSelected = true),
    Patch(Requests, ThisRecord, {
        Status:     "Approved",
        ApprovedBy: User().Email,
        ApprovedOn: Now()
    })
);
ClearCollect(varPending, Filter(Requests, Status = "Pending"));
Notify("Requests approved.", NotificationType.Success)
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
| [`Collect`](./Collect.md) | Related function |
| [`ClearCollect`](./ClearCollect.md) | Related function |
| [`Filter`](./Filter.md) | Related function |

---

## 🔗 Official Documentation
[Sequence, ForAll – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-forall)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*