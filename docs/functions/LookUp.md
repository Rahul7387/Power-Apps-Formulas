# 🔎 LookUp

> **Category:** Data Read | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## Overview

`LookUp` searches a table and returns **a single record** matching a condition. If no record matches it returns `Blank()`. Use it wherever you need one specific row — finding a user profile, resolving an ID to a display name, or fetching a related record.

---

## Syntax

```plaintext
LookUp( Table, Condition [, ReduceFormula] )
```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `Table` | Table | ✅ Yes | Data source or table to search |
| `Condition` | Boolean | ✅ Yes | Expression that identifies the desired row |
| `ReduceFormula` | Any | ❌ Optional | Return only a specific field value instead of the whole record |

---

## Simple Examples

### 1. Find a record by ID
```plaintext
LookUp(Products, ID = varProductID)
```

### 2. Get a single field from a matching record
```plaintext
LookUp(Employees, Email = User().Email).Department
```

### 3. Find a record from gallery selection
```plaintext
LookUp(Orders, OrderNumber = Gallery_Orders.Selected.OrderNumber)
```

### 4. Use the 3rd argument to return just a value
```plaintext
// Returns just the manager's name, not the whole record
LookUp(Employees, EmployeeID = varEmpID, ManagerName)
```

### 5. Check if a record exists
```plaintext
If(IsBlank(LookUp(Users, Email = txtEmail.Text)), "Not found", "Found")
```

---

## Complex Examples

### 6. Upsert — update if exists, create if not
```plaintext
Set(varExisting, LookUp(UserProfiles, Email = User().Email));
If(
    IsBlank(varExisting),
    Patch(UserProfiles, Defaults(UserProfiles), {
        Email: User().Email, Name: User().FullName, CreatedOn: Now()
    }),
    Patch(UserProfiles, varExisting, { LastLogin: Now(), LoginCount: varExisting.LoginCount + 1 })
)
```

### 7. Resolve a lookup chain (manager of the manager)
```plaintext
// Get current employee
Set(varEmp, LookUp(Employees, Email = User().Email));
// Get their manager
Set(varManager, LookUp(Employees, EmployeeID = varEmp.ManagerID));
// Get the manager's manager
Set(varGrandManager, LookUp(Employees, EmployeeID = varManager.ManagerID))
```

### 8. LookUp inside a gallery label (per-row related data)
```plaintext
// Inside Gallery_Orders — show the customer name for each order
// Label Text property:
LookUp(Customers, ID = ThisItem.CustomerID).CompanyName
```

### 9. Get the maximum value record
```plaintext
// Find the product with the highest price in a category
LookUp(
    Sort(Filter(Products, Category = "Electronics"), Price, SortOrder.Descending),
    true   // first record after sort = highest price
)
```

### 10. Role-based access using LookUp
```plaintext
// App OnStart
Set(varUserProfile, LookUp(AppUsers, Email = User().Email));
Set(varCanApprove,  !IsBlank(LookUp(ApproverList, Email = User().Email)));
Set(varIsAdmin,     varUserProfile.Role = "Admin")

// Feature visibility:
// Approve button Visible: varCanApprove
// Admin panel Visible: varIsAdmin
```

---

## Filter vs LookUp

| | `Filter` | `LookUp` |
|-|----------|----------|
| Returns | Table (0+ rows) | Single record or value |
| Use in gallery `Items` | ✅ | ❌ |
| Access one value | `First(Filter(...)).Field` | `LookUp(...).Field` ✅ cleaner |
| Delegation | Same rules | Same rules |

---

## Best Practices

1. **Always guard with `IsBlank`** before accessing fields: `LookUp(...)?.Field` or `If(IsBlank(result), ..., result.Field)`.
2. **Use the 3rd parameter** when you only need one field — it's more readable than `.FieldName` after the call.
3. **Cache with `Set`** if you call the same `LookUp` many times on one screen.
4. **Avoid in galleries on large datasets** — per-row `LookUp` calls inside a gallery can cause many server round-trips; join in a collection instead.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Accessing `.Field` without `IsBlank` check | Error if no record found | Wrap with `If(!IsBlank(result), result.Field, "")` |
| Using `LookUp` in Gallery Items | Returns one record, not a table | Use `Filter` for gallery data sources |
| LookUp on a non-delegable column | Capped at row limit | Filter on indexed columns; look up by key |

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Filter`](./Filter.md) | Returns multiple rows; use when you need a table |
| [`First`](./First.md) | Get first row from Filter result |
| [`IsBlank`](./IsBlank.md) | Guard against no-match returns |
| [`Patch`](./Patch.md) | Use LookUp result as the base record to update |

---

## 🔗 Official Documentation
[Filter, Search, LookUp – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-filter-lookup)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*