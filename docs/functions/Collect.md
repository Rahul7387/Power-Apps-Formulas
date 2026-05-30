# 📥 Collect

> **Category:** Collections | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## Overview

`Collect` appends one or more records to a **collection** — an in-memory table that lives in the app session. If the collection does not yet exist, `Collect` creates it automatically. Collections are perfect for shopping carts, multi-select lists, offline caches, and staged data before a bulk save.

---

## Syntax

```plaintext
Collect( CollectionName, Record_or_Table [, Record_or_Table2, ...] )
```

## Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `CollectionName` | ✅ | Name of the collection to append to |
| `Record_or_Table` | ✅ | A record `{ }` or table to add |

---

## Simple Examples

### 1. Add one item to a cart
```plaintext
Collect(varCart, { ProductID: Gallery_Products.Selected.ID, Name: Gallery_Products.Selected.Name, Price: Gallery_Products.Selected.Price, Qty: 1 })
```

### 2. Log an action
```plaintext
Collect(varActivityLog, { Action: "Login", User: User().Email, Timestamp: Now() })
```

### 3. Build a static list
```plaintext
Collect(varStatuses, { Value: "Draft" }, { Value: "Active" }, { Value: "Closed" })
```

### 4. Load from data source
```plaintext
// App OnStart
Collect(varProducts, Filter(Products, IsActive = true))
```

### 5. Add selected gallery item to a list
```plaintext
Collect(varSelectedItems, Gallery1.Selected)
```

---

## Complex Examples

### 6. Shopping cart with duplicate guard
```plaintext
// "Add to Cart" button
If(
    !IsBlank(LookUp(varCart, ProductID = Gallery_Products.Selected.ID)),
    Notify("Already in cart!", NotificationType.Warning),
    Collect(varCart, {
        ProductID: Gallery_Products.Selected.ID,
        Name:      Gallery_Products.Selected.Name,
        Price:     Gallery_Products.Selected.Price,
        Qty:       1
    });
    Set(varCartTotal, Sum(varCart, Price * Qty));
    Notify(Gallery_Products.Selected.Name & " added!", NotificationType.Success)
)
```

### 7. Multi-select pattern using a collection
```plaintext
// Checkbox OnCheck (inside gallery)
If(
    Self.Value,
    Collect(varSelected, { ID: ThisItem.ID, Title: ThisItem.Title }),
    Remove(varSelected, LookUp(varSelected, ID = ThisItem.ID))
)

// "Delete Selected" button
ForAll(varSelected, Remove(Tasks, LookUp(Tasks, ID = ID)));
ClearCollect(varSelected, []);
Notify("Deleted " & CountRows(varSelected) & " items.", NotificationType.Success)
```

### 8. Offline cache with incremental load
```plaintext
// App OnStart — load all pages
Collect(varAllRecords, FirstPage);
Set(varPage, 2);
While(CountRows(varLastPage) = 100,
    Set(varLastPage, Filter(BigTable, PageNumber = varPage));
    Collect(varAllRecords, varLastPage);
    Set(varPage, varPage + 1)
)
```

### 9. Build a line-items collection for an order form
```plaintext
// "Add Line" button
Collect(varOrderLines, {
    RowID:       CountRows(varOrderLines) + 1,
    ProductID:   ddlProduct.Selected.ID,
    ProductName: ddlProduct.Selected.Name,
    Qty:         Value(txtQty.Text),
    UnitPrice:   ddlProduct.Selected.Price,
    LineTotal:   Value(txtQty.Text) * ddlProduct.Selected.Price
})
```

### 10. Save entire collection to data source on submit
```plaintext
// "Submit Order" button
Set(varOrderHeader, Patch(Orders, Defaults(Orders), {
    CustomerID: varSelectedCustomer.ID,
    OrderDate:  Today(),
    CreatedBy:  User().Email
}));
ForAll(varOrderLines,
    Patch(OrderLines, Defaults(OrderLines), {
        OrderID:    varOrderHeader.ID,
        ProductID:  ProductID,
        Qty:        Qty,
        UnitPrice:  UnitPrice,
        LineTotal:  LineTotal
    })
);
ClearCollect(varOrderLines, []);
Notify("Order " & varOrderHeader.ID & " submitted!", NotificationType.Success);
Navigate(OrdersScreen, ScreenTransition.UnCover)
```

---

## Collect vs ClearCollect

| | `Collect` | `ClearCollect` |
|-|-----------|----------------|
| Existing data | **Appends** | **Cleared first** |
| Use when | Adding incrementally | Refreshing/replacing |
| Cart pattern | ✅ Add item by item | Reset cart: `ClearCollect(varCart,[])` |

---

## Best Practices

1. **Use `ClearCollect` to refresh** — never `Collect` when you want a fresh snapshot.
2. **Add a schema-defining first row in `OnStart`** so controls bound to the collection know the column types.
3. **Limit collection size** — large collections (1000+ rows) impact performance on mobile.
4. **Collections do not persist** across app sessions — save important data to a data source.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`ClearCollect`](./ClearCollect.md) | Clear and re-populate a collection |
| [`Remove`](./Remove.md) | Delete a record from a collection |
| [`ForAll`](./ForAll.md) | Iterate and save collection to data source |
| [`Patch`](./Patch.md) | Bulk-save collection to data source |

---

## 🔗 Official Documentation
[Collect and ClearCollect – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-clear-collect-clearcollect)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*