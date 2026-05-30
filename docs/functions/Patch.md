# 🔧 Patch

> **Category:** Data Write | **Complexity:** ⭐⭐⭐ Advanced | **Works in:** Canvas Apps

---

## 📋 Table of Contents
- [Overview](#overview)
- [Syntax](#syntax)
- [Parameters](#parameters)
- [Simple Examples](#simple-examples)
- [Complex Examples](#complex-examples)
- [Patch vs SubmitForm](#patch-vs-submitform)
- [Error Handling](#error-handling)
- [Delegation & Limits](#delegation--limits)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)
- [Related Functions](#related-functions)

---

## Overview

`Patch` is one of the most powerful functions in Power Fx. It **creates or modifies records** in a data source directly — without needing a Form control. It gives you full programmatic control over which fields to write, making it ideal for background saves, bulk updates, and complex multi-step workflows.

Unlike `SubmitForm`, which is tied to a Form control on screen, `Patch` can be called from any button, timer, or event — and can even return the created/updated record for immediate use.

```
┌─────────────────────────────────────────────────────────────────┐
│  Patch(DataSource, BaseRecord, ChangeRecord)                    │
│                                                                 │
│  DataSource  ──► WHERE to write (SharePoint, Dataverse, SQL)   │
│  BaseRecord  ──► WHICH record (Defaults = NEW, existing = UPDATE│
│  ChangeRecord──► WHAT to write (field: value pairs)            │
└─────────────────────────────────────────────────────────────────┘
```

---

## Syntax

```plaintext
// Create or update a single record
Patch( DataSource, BaseRecord, ChangeRecord [, AdditionalChangeRecord, ...] )

// Bulk update — modify multiple records at once
Patch( DataSource, TableOfChanges )
```

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `DataSource` | Data Source | ✅ Yes | The table/list to write to (SharePoint list, Dataverse table, SQL table, etc.) |
| `BaseRecord` | Record | ✅ Yes | The record to update. Use `Defaults(DataSource)` to **create a new** record, or an existing record reference to **update** |
| `ChangeRecord` | Record | ✅ Yes | A record literal `{ Field: Value }` with the fields to write |
| `AdditionalChangeRecord` | Record | ❌ Optional | Extra change records — merged left to right |
| `TableOfChanges` | Table | ✅ (bulk form) | A table of records to patch in bulk |

### BaseRecord Decision Guide

```
Do you want to CREATE a new record?
  └─► Use: Defaults(DataSource)

Do you want to UPDATE an existing record?
  ├─► From a gallery:      Gallery1.Selected
  ├─► From a variable:     varCurrentRecord
  ├─► From a lookup:       LookUp(Tasks, ID = varID)
  └─► From form last save: EditForm1.LastSubmit
```

---

## Simple Examples

### 1. Create a new record
```plaintext
// OnSelect of a "Save" button
Patch(
    Tasks,
    Defaults(Tasks),
    {
        Title: txtTitle.Text,
        AssignedTo: User().Email,
        Status: "Active"
    }
)
```

### 2. Update the gallery-selected record
```plaintext
// Change the status of whichever item is selected in the gallery
Patch(
    Products,
    Gallery1.Selected,
    { Status: "Discontinued" }
)
```

### 3. Update a single field only
```plaintext
// Mark a task as complete — touch only the Status field
Patch(Tasks, thisTask, { Status: "Completed" })
```

### 4. Patch with immediate notification
```plaintext
Patch(Customers, Gallery1.Selected, { Priority: "High" });
Notify("Priority updated!", NotificationType.Success)
```

### 5. Patch and navigate away
```plaintext
Patch(
    Feedback,
    Defaults(Feedback),
    { Comment: txtComment.Text, SubmittedOn: Now() }
);
Navigate(ThankYouScreen, ScreenTransition.Fade)
```

---

## Complex Examples

### 6. Capture the new record (get the auto-generated ID)
```plaintext
// After Patch, varNewRecord holds the full created record including ID
Set(
    varNewRecord,
    Patch(
        Invoices,
        Defaults(Invoices),
        {
            CustomerName: txtCustomer.Text,
            InvoiceDate:  Today(),
            Total:        Sum(varLineItems, Price * Qty)
        }
    )
);
Notify("Invoice #" & varNewRecord.ID & " created!", NotificationType.Success);
Navigate(InvoiceDetailScreen, ScreenTransition.Cover)
```

### 7. Multi-step form saved across two tables atomically
```plaintext
// Save header record, then use its ID for the child line items
Set(
    varOrderHeader,
    Patch(
        Orders,
        Defaults(Orders),
        {
            CustomerID:  varSelectedCustomer.ID,
            OrderDate:   Today(),
            CreatedBy:   User().Email,
            Status:      "Draft"
        }
    )
);
// Now save every line item, linking to the new header ID
ForAll(
    varCart,
    Patch(
        OrderLines,
        Defaults(OrderLines),
        {
            OrderID:    varOrderHeader.ID,
            ProductID:  ProductID,
            Qty:        Qty,
            UnitPrice:  UnitPrice,
            LineTotal:  Qty * UnitPrice
        }
    )
);
Notify("Order " & varOrderHeader.OrderNumber & " placed!", NotificationType.Success);
Navigate(OrderConfirmationScreen, ScreenTransition.Cover, { NewOrder: varOrderHeader })
```

### 8. Conditional create-or-update (upsert pattern)
```plaintext
// Check if a profile already exists for the current user; create if not, update if yes
Set(
    varExistingProfile,
    LookUp(UserProfiles, Email = User().Email)
);
If(
    IsBlank(varExistingProfile),
    // CREATE — no profile found
    Patch(
        UserProfiles,
        Defaults(UserProfiles),
        {
            Email:       User().Email,
            DisplayName: User().FullName,
            Role:        "User",
            CreatedOn:   Now()
        }
    ),
    // UPDATE — profile exists, update last login only
    Patch(
        UserProfiles,
        varExistingProfile,
        { LastLogin: Now(), LoginCount: varExistingProfile.LoginCount + 1 }
    )
)
```

### 9. Bulk update — mark all selected items as shipped
```plaintext
// varSelectedOrders is a collection of orders checked by the user
Patch(
    Orders,
    AddColumns(
        varSelectedOrders,
        "Status",        "Shipped",
        "ShippedDate",   Today(),
        "ShippedBy",     User().Email
    )
);
Notify(CountRows(varSelectedOrders) & " orders marked as Shipped.", NotificationType.Success);
ClearCollect(varSelectedOrders, []);   // clear selection
Refresh(Orders)
```

### 10. Patch with validation and rollback pattern
```plaintext
// Validate first, then Patch, and handle the error if Patch fails
If(
    IsBlank(txtProductName.Text),
    Notify("Product name is required.", NotificationType.Error),
    Value(txtStock.Text) < 0,
    Notify("Stock cannot be negative.", NotificationType.Error),
    // All validations passed — attempt the save
    Set(
        varSaveResult,
        Patch(
            Products,
            Coalesce(varEditRecord, Defaults(Products)),
            {
                Name:        txtProductName.Text,
                SKU:         Upper(Trim(txtSKU.Text)),
                Stock:       Value(txtStock.Text),
                Price:       Value(txtPrice.Text),
                Category:    ddlCategory.Selected.Value,
                LastUpdated: Now(),
                UpdatedBy:   User().Email
            }
        )
    );
    If(
        IsError(varSaveResult),
        Notify("Save failed: " & varSaveResult.Message, NotificationType.Error),
        Notify("Product saved successfully!", NotificationType.Success);
        Navigate(ProductListScreen, ScreenTransition.UnCover)
    )
)
```

### 11. Patch a SharePoint Person/Lookup field
```plaintext
// Assign a SharePoint Person column and a Lookup column
Patch(
    ProjectTasks,
    Gallery_Tasks.Selected,
    {
        // Person field — must use the { Claims: "i:0#.f|..." } structure or email
        AssignedTo: { DisplayName: User().FullName, Email: User().Email },
        // Lookup column — must pass { Id: number, Value: text }
        ProjectLookup: { Id: varSelectedProject.ID, Value: varSelectedProject.Title }
    }
)
```

### 12. Patch with Dataverse — use logical column names
```plaintext
// Dataverse uses logical names (all lowercase, often prefixed)
Patch(
    cr123_workorders,                        // Dataverse table logical name
    Defaults(cr123_workorders),
    {
        cr123_title:        txtTitle.Text,   // custom text column
        cr123_priority:     2,               // choice column (integer value)
        cr123_assignedto:   varTechnicianId, // lookup column (GUID)
        statecode:          0,               // system status (0 = Active)
        statuscode:         1                // system status reason
    }
)
```

---

## Patch vs SubmitForm

| Feature | `Patch` | `SubmitForm` |
|---------|---------|--------------|
| Requires Form control | ❌ No | ✅ Yes |
| Writes specific fields | ✅ Full control | Auto from data cards |
| Built-in validation | ❌ Must do manually | ✅ Data card rules |
| Returns the saved record | ✅ Yes | ❌ No (use `LastSubmit`) |
| Bulk / multi-record writes | ✅ Yes | ❌ No |
| Background / silent writes | ✅ Yes | ❌ No |
| Best for | Code-first, programmatic saves | Simple user-facing forms |

**Rule of thumb:** Use `SubmitForm` when you have a Form control with data cards and want built-in field validation. Use `Patch` for everything else — background logic, bulk operations, or when you need the returned record.

---

## Error Handling

Starting with **Power Apps version 3.21xxx+**, `Patch` can return an error record that you can inspect with `IsError()`:

```plaintext
Set(varResult, Patch(Tasks, Defaults(Tasks), { Title: txtTitle.Text }));
If(
    IsError(varResult),
    Notify("Error: " & varResult.Message, NotificationType.Error),
    Notify("Saved! ID = " & varResult.ID, NotificationType.Success)
)
```

For older apps without `IsError`, use `IfError`:

```plaintext
IfError(
    Patch(Tasks, Defaults(Tasks), { Title: txtTitle.Text }),
    Notify("Something went wrong.", NotificationType.Error)
)
```

---

## Delegation & Limits

| Data Source | Create | Update | Bulk Patch |
|------------|--------|--------|------------|
| SharePoint | ✅ | ✅ | ✅ (up to 2000 rows) |
| Dataverse | ✅ | ✅ | ✅ |
| SQL Server | ✅ | ✅ | ✅ |
| Excel (OneDrive) | ✅ | ✅ | ⚠️ Limited |
| Collections | ✅ | ✅ | ✅ |

> ⚠️ `Patch` on collections is **in-memory only** — changes do not persist to any server unless you also call `Patch` on the underlying data source.

---

## Best Practices

1. **Always capture the return value** when you need the new record's ID:
   `Set(varNew, Patch(...))`

2. **Use `Defaults(DataSource)` to create** — never pass an empty record `{}` as the base.

3. **Validate before patching** — check `IsBlank`, `IsMatch`, and business rules *before* calling `Patch` to avoid partial saves.

4. **Separate UI concerns from data writes** — put `Patch` in button `OnSelect`, not in a label's `Text` property.

5. **Refresh after Patch** if your gallery reads directly from the data source:
   `Patch(...); Refresh(Products)`

6. **Use `Coalesce` for create-or-update** patterns:
   `Patch(DS, Coalesce(varRecord, Defaults(DS)), { ... })`

7. **Use bulk `Patch` with `AddColumns`** instead of `ForAll + Patch` for better performance on large sets.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| `Patch(DS, {}, { Title: "X" })` | `{}` is not a valid base record | Use `Defaults(DS)` to create, or a real record reference to update |
| Patching a Choice/Lookup field with a plain string | Type mismatch error | Use `{ Value: "ChoiceText" }` for Choice, `{ Id: n }` for Lookup |
| Not refreshing after patch | Gallery shows stale data | Add `Refresh(DataSource)` after `Patch` |
| Calling `Patch` in a `Text` property | Behavior functions can't run in calculated properties | Move to `OnSelect` or another behavior property |
| Missing required columns | Record saves with blank required fields | Validate with `IsBlank()` before calling `Patch` |

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Defaults`](./Defaults.md) | Provides the base record for creating new records with `Patch` |
| [`SubmitForm`](./SubmitForm.md) | Form-based alternative to `Patch` |
| [`Remove`](./Remove.md) | Deletes records from a data source |
| [`Collect`](./Collect.md) | Adds records to an in-memory collection |
| [`ForAll`](./ForAll.md) | Iterate + Patch pattern for row-by-row updates |
| [`LookUp`](./LookUp.md) | Find the base record to pass to `Patch` for updating |
| [`IsError`](./IsError.md) | Check if `Patch` returned an error |
| [`Refresh`](./Refresh.md) | Reload data source after `Patch` |

---

## 🔗 Official Documentation
[Patch function – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-patch)

---

*← [Back to Home](../README.md) | Power Apps Formulas Reference | Last updated: May 2026*
