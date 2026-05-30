# ❓ If

> **Category:** Logic | **Complexity:** ⭐ Beginner | **Works in:** Canvas Apps

---

## Overview

`If` evaluates one or more conditions and returns a different value or runs a different action for each outcome. It supports multi-branch (else-if) logic in a single expression — making it the most widely used logic function in Power Fx.

---

## Syntax

```plaintext
// Two branches
If( Condition, ThenResult, ElseResult )

// Multi-branch (else-if)
If( Cond1, Result1, Cond2, Result2, ..., DefaultResult )
```

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `Condition` | Boolean | ✅ Yes | Expression that evaluates to true or false |
| `ThenResult` | Any | ✅ Yes | Value or action when condition is true |
| `ElseResult` | Any | ❌ Optional | Value or action when all conditions are false |

---

## Simple Examples

### 1. Show/hide text
```plaintext
If(Toggle1.Value, "Enabled", "Disabled")
```

### 2. Colour by status
```plaintext
If(ThisItem.Status = "Active", Color.Green, Color.Gray)
```

### 3. Disable a button
```plaintext
// Button DisplayMode
If(IsBlank(txtTitle.Text), DisplayMode.Disabled, DisplayMode.Edit)
```

### 4. Guard a Patch call
```plaintext
If(
    IsBlank(txtName.Text),
    Notify("Name required", NotificationType.Error),
    Patch(Customers, Defaults(Customers), { Name: txtName.Text })
)
```

### 5. Nested If for overdue badge
```plaintext
If(
    ThisItem.DueDate < Today(), "Overdue",
    ThisItem.DueDate = Today(), "Due Today",
    "On Track"
)
```

---

## Complex Examples

### 6. Multi-field validation chain
```plaintext
If(
    IsBlank(txtName.Text),    Notify("Name is required.",        NotificationType.Error),
    IsBlank(txtEmail.Text),   Notify("Email is required.",       NotificationType.Error),
    !IsMatch(txtEmail.Text, Email), Notify("Invalid email.",     NotificationType.Warning),
    Value(txtAge.Text) < 18,  Notify("Must be 18 or older.",     NotificationType.Error),
    SubmitForm(Form1)
)
```

### 7. Traffic-light KPI colour
```plaintext
// Label colour for a sales KPI
If(
    varSalesTotal >= varTarget * 1.0,  Color.Green,
    varSalesTotal >= varTarget * 0.8,  Color.Orange,
    Color.Red
)
```

### 8. Compute a discount tier
```plaintext
// Label Text showing discount percentage
"Discount: " & If(
    Slider_Qty.Value >= 100, "20%",
    Slider_Qty.Value >= 50,  "10%",
    Slider_Qty.Value >= 20,  "5%",
    "0%"
)
```

### 9. Role-gated navigation
```plaintext
// Button OnSelect
If(
    varUserRole = "Admin",    Navigate(AdminScreen, ScreenTransition.Cover),
    varUserRole = "Manager",  Navigate(ManagerScreen, ScreenTransition.Cover),
    Notify("You do not have access to that area.", NotificationType.Warning)
)
```

### 10. Dynamic form title
```plaintext
// Form screen title label
If(
    EditForm1.Mode = FormMode.New,  "Create New Project",
    EditForm1.Mode = FormMode.Edit, "Edit: " & EditForm1.Item.Title,
    "View: " & EditForm1.Item.Title
)
```

---

## If vs Switch

| Use `If` when | Use `Switch` when |
|---------------|-------------------|
| Conditions involve ranges or expressions | Comparing one value against fixed matches |
| Fewer than 4 branches | 4+ exact-match branches |
| Mixed condition types | All conditions check the same variable |

---

## Best Practices

1. **Use `Switch` for 4+ exact-match branches** — cleaner than deeply nested `If`.
2. **Put the most likely true condition first** — Power Fx short-circuits; first true wins.
3. **Always include a default (else) branch** — avoids returning `Blank()` unexpectedly.
4. **Use `And`/`Or` inside one condition** rather than nesting when logic is simple.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Switch`](./Switch.md) | Cleaner multi-match alternative |
| [`IsBlank`](./IsBlank.md) | Common condition inside `If` |
| [`IsEmpty`](./IsEmpty.md) | Check collections inside `If` |
| [`Coalesce`](./Coalesce.md) | Return first non-blank value |

---

## 🔗 Official Documentation
[If and Switch – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-if)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*