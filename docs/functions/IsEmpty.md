# 📭 IsEmpty

> **Category:** Validation | **Complexity:** ⭐ Beginner | **Works in:** Canvas Apps

---

## Overview

`IsEmpty` returns `true` if a **table or collection contains zero rows**. Use it to guard against empty galleries, disabled buttons when no items exist, and conditional messages.

---

## Syntax

```plaintext
IsEmpty( Table )
```

---

## Simple Examples

### 1. Show a "no results" label
```plaintext
// Label Visible
IsEmpty(Filter(Products, Status = "Active"))
```

### 2. Disable submit when cart is empty
```plaintext
// Button DisplayMode
If(IsEmpty(varCart), DisplayMode.Disabled, DisplayMode.Edit)
```

### 3. Guard before using First()
```plaintext
If(!IsEmpty(varResults), Set(varTop, First(varResults)))
```

### 4. Conditional message
```plaintext
If(IsEmpty(varNotifications), "You have no new notifications.", "")
```

### 5. Show count badge only when items exist
```plaintext
// Badge Visible
!IsEmpty(varCart)
```

---

## Complex Examples

### 6. Empty-state screen pattern
```plaintext
// Container Visible (shows illustration when no data)
IsEmpty(Filter(Tasks, AssignedTo = User().Email && Status = "Open"))

// Gallery Visible (inverse)
!IsEmpty(Filter(Tasks, AssignedTo = User().Email && Status = "Open"))
```

### 7. Prevent navigation if no items selected
```plaintext
// "Proceed" button OnSelect
If(
    IsEmpty(varSelectedItems),
    Notify("Please select at least one item.", NotificationType.Warning),
    Navigate(CheckoutScreen, ScreenTransition.Cover, { selectedItems: varSelectedItems })
)
```

### 8. Auto-load when collection is empty
```plaintext
// Screen OnVisible — only load if not already cached
If(
    IsEmpty(varProducts),
    ClearCollect(varProducts, Filter(Products, IsActive = true));
    Notify("Products loaded.", NotificationType.Information)
)
```

### 9. Validation before bulk save
```plaintext
If(
    IsEmpty(varOrderLines),
    Notify("Add at least one line item before submitting.", NotificationType.Warning),
    Sum(varOrderLines, LineTotal) <= 0,
    Notify("Order total must be greater than zero.", NotificationType.Error),
    SubmitForm(OrderHeaderForm)
)
```

### 10. Combine IsEmpty with IsBlank for comprehensive guard
```plaintext
// Only allow export when data exists and a date range is set
If(
    IsEmpty(varReportData),
    Notify("No data to export.", NotificationType.Warning),
    IsBlank(dpStart.SelectedDate) || IsBlank(dpEnd.SelectedDate),
    Notify("Select a date range first.", NotificationType.Warning),
    Launch(varExportURL)
)
```

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`IsBlank`](./IsBlank.md) | Check a single value for blank |
| [`CountRows`](./CountRows.md) | Get the count rather than true/false |
| [`Filter`](./Filter.md) | Produce the table tested by `IsEmpty` |
| [`Coalesce`](./Coalesce.md) | Return first non-blank |

---

## 🔗 Official Documentation
[IsBlank and IsEmpty – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-isblank-isempty)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*