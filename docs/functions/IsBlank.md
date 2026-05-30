# ⬜ IsBlank

> **Category:** Validation | **Complexity:** ⭐ Beginner | **Works in:** Canvas Apps

---

## Overview

`IsBlank` returns `true` if a value is **blank, null, or an empty string**. It is the primary guard function for form validation, null-safety checks, and conditional display logic.

---

## Syntax

```plaintext
IsBlank( Value )
```

---

## Simple Examples

### 1. Required field guard
```plaintext
If(IsBlank(txtName.Text), Notify("Name is required.", NotificationType.Error))
```

### 2. Disable a button until input is provided
```plaintext
// Button DisplayMode
If(IsBlank(txtEmail.Text), DisplayMode.Disabled, DisplayMode.Edit)
```

### 3. Fallback display value
```plaintext
If(IsBlank(Gallery1.Selected.Description), "No description provided.", Gallery1.Selected.Description)
```

### 4. Guard a navigation action
```plaintext
If(IsBlank(Gallery1.Selected), Notify("Select an item first.", NotificationType.Warning), Navigate(DetailScreen))
```

### 5. Prevent blank Patch
```plaintext
If(
    IsBlank(txtTitle.Text) || IsBlank(ddlCategory.Selected.Value),
    Notify("All fields required.", NotificationType.Error),
    Patch(Tasks, Defaults(Tasks), { Title: txtTitle.Text, Category: ddlCategory.Selected.Value })
)
```

---

## Complex Examples

### 6. Multi-field validation before submit
```plaintext
If(
    IsBlank(txtName.Text),     Notify("Name required",     NotificationType.Error),
    IsBlank(txtEmail.Text),    Notify("Email required",    NotificationType.Error),
    IsBlank(txtPhone.Text),    Notify("Phone required",    NotificationType.Error),
    IsBlank(dpDOB.SelectedDate), Notify("DOB required",   NotificationType.Error),
    !IsMatch(txtEmail.Text, Email), Notify("Invalid email", NotificationType.Warning),
    SubmitForm(Form1)
)
```

### 7. Use IsBlank to build a dynamic filter
```plaintext
// Gallery Items — when search box is blank, show all; otherwise filter
Filter(
    Products,
    IsBlank(txtSearch.Text) || StartsWith(Name, txtSearch.Text),
    IsBlank(ddlCategory.Selected.Value) || Category = ddlCategory.Selected.Value
)
```

### 8. Coalesce pattern using IsBlank
```plaintext
// Show value if present, otherwise show default
If(IsBlank(varSelectedRecord.Notes), "N/A", varSelectedRecord.Notes)

// Cleaner with Coalesce:
Coalesce(varSelectedRecord.Notes, "N/A")
```

### 9. Cascade dropdowns — disable second until first is chosen
```plaintext
// Second dropdown DisplayMode
If(IsBlank(ddlCountry.Selected.Value), DisplayMode.Disabled, DisplayMode.Edit)

// Second dropdown Items
If(
    IsBlank(ddlCountry.Selected.Value),
    [],
    Filter(Cities, Country = ddlCountry.Selected.Value)
)
```

### 10. Form completeness progress bar
```plaintext
// Width of progress bar (4 fields, 25% each)
((If(!IsBlank(txtName.Text),    1, 0) +
  If(!IsBlank(txtEmail.Text),   1, 0) +
  If(!IsBlank(dpDOB.SelectedDate), 1, 0) +
  If(!IsBlank(ddlRole.Selected.Value), 1, 0)) / 4) * Parent.Width
```

---

## IsBlank vs IsEmpty

| | `IsBlank` | `IsEmpty` |
|-|-----------|-----------|
| Tests | Single value | Table / collection |
| Returns true for | `""`, `null`, `blank` | Table with 0 rows |
| Use for | Text, numbers, dates | Galleries, collections |

---

## Best Practices

1. **Use `Coalesce` as shorthand** — `Coalesce(val, "default")` replaces `If(IsBlank(val), "default", val)`.
2. **Check before using LookUp results** — `LookUp` returns blank when nothing is found.
3. **Use `||` to combine `IsBlank` checks** in a single condition for multi-field validation.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`IsEmpty`](./IsEmpty.md) | Check if a table has rows |
| [`Coalesce`](./Coalesce.md) | Return first non-blank value |
| [`If`](./If.md) | Branch on `IsBlank` result |
| [`IsMatch`](./IsMatch.md) | Validate format (email, phone) |

---

## 🔗 Official Documentation
[IsBlank and IsEmpty – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-isblank-isempty)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*