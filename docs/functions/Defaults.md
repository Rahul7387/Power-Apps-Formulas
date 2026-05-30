# ⚙️ Defaults

> **Category:** Variables / Forms | **Works in:** Canvas Apps

---

## Overview

Returns a record containing default column values for a data source. Essential as the base record when using Patch to create new records.

---

## Syntax

```plaintext
Defaults( DataSource )
```

---

## Simple Examples

### 1. Create new record with Patch
```plaintext
Patch(Tasks, Defaults(Tasks), { Title: txtTitle.Text, AssignedTo: User().Email })
```

### 2. Set form to new record
```plaintext
EditForm1.Item = Defaults(SharePointList)
```

### 3. Pass defaults to a new screen
```plaintext
Navigate(NewItemScreen, ScreenTransition.Cover, { FormItem: Defaults(Projects) })
```

---

## Complex Examples

### 4. Upsert — create or update
```plaintext
Set(varExisting, LookUp(UserProfiles, Email = User().Email));
Patch(
    UserProfiles,
    Coalesce(varExisting, Defaults(UserProfiles)),
    { Email: User().Email, Name: User().FullName, LastLogin: Now() }
)
```

### 5. New record with all computed defaults
```plaintext
Patch(
    Employees,
    Defaults(Employees),
    {
        Name:       txtName.Text,
        Email:      txtEmail.Text,
        StartDate:  Today(),
        EmployeeID: "EMP-" & Text(CountRows(Employees)+1, "000"),
        CreatedBy:  User().Email,
        IsActive:   true
    }
)
```

### 6. Multi-form new mode
```plaintext
// New button:
Set(varRecord, Defaults(Projects));
NewForm(EditForm1);
Navigate(FormScreen, ScreenTransition.Cover)
// EditForm1.Item: varRecord
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
| [`NewForm`](./NewForm.md) | Related function |
| [`Coalesce`](./Coalesce.md) | Related function |
| [`LookUp`](./LookUp.md) | Related function |

---

## 🔗 Official Documentation
[Defaults – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-defaults)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*