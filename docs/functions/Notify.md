# 🔔 Notify

> **Category:** UI Feedback | **Complexity:** ⭐ Beginner | **Works in:** Canvas Apps

---

## 📋 Table of Contents
- [Overview](#overview)
- [Syntax](#syntax)
- [Parameters](#parameters)
- [Notification Types](#notification-types)
- [Simple Examples](#simple-examples)
- [Complex Examples](#complex-examples)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)
- [Related Functions](#related-functions)

---

## Overview

`Notify` displays a **toast notification banner** at the top of the Power Apps screen. It is the primary way to give users real-time feedback about the result of an action — saving a record, deleting an item, validation errors, or general information.

```
┌────────────────────────────────────────────────────────┐
│  ✅  Record saved successfully!              [dismiss] │   ← NotificationType.Success
└────────────────────────────────────────────────────────┘
┌────────────────────────────────────────────────────────┐
│  ❌  Could not save. Please try again.       [dismiss] │   ← NotificationType.Error
└────────────────────────────────────────────────────────┘
```

Notifications auto-dismiss after the timeout period (default 2 seconds).

---

## Syntax

```plaintext
Notify( Message [, NotificationType [, Timeout_ms ]] )
```

---

## Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `Message` | Text | ✅ Yes | — | The text to display in the banner |
| `NotificationType` | Enum | ❌ Optional | `NotificationType.Information` | Visual style (colour) of the banner |
| `Timeout_ms` | Number | ❌ Optional | `2000` | Milliseconds before auto-dismiss. Use `0` for persistent |

---

## Notification Types

| Type | Colour | Icon | Best Used For |
|------|--------|------|---------------|
| `NotificationType.Success` | 🟢 Green | ✅ | Record saved, action completed |
| `NotificationType.Error` | 🔴 Red | ❌ | Save failed, permission denied |
| `NotificationType.Warning` | 🟡 Yellow | ⚠️ | Caution required, data will be lost |
| `NotificationType.Information` | 🔵 Blue | ℹ️ | General info (default if omitted) |

---

## Simple Examples

### 1. Success after saving
```plaintext
Notify("Record saved successfully!", NotificationType.Success)
```

### 2. Error notification
```plaintext
Notify("Failed to save. Please try again.", NotificationType.Error)
```

### 3. Warning
```plaintext
Notify("This action cannot be undone.", NotificationType.Warning)
```

### 4. Default info (no type needed)
```plaintext
Notify("Loading your data...")
```

### 5. Dynamic message with user's name
```plaintext
Notify("Welcome back, " & User().FullName & "!", NotificationType.Information)
```

---

## Complex Examples

### 6. Validation chain — show specific error message
```plaintext
// Button OnSelect — validate each field, show the first error found
If(
    IsBlank(txtName.Text),
    Notify("Full name is required.", NotificationType.Error),
    IsBlank(txtEmail.Text),
    Notify("Email address is required.", NotificationType.Error),
    !IsMatch(txtEmail.Text, Email),
    Notify("Please enter a valid email address.", NotificationType.Warning),
    IsBlank(dpStartDate.SelectedDate),
    Notify("Start date must be selected.", NotificationType.Error),
    dpStartDate.SelectedDate > dpEndDate.SelectedDate,
    Notify("Start date cannot be after end date.", NotificationType.Warning),
    // All good — submit
    SubmitForm(Form1)
)
```

### 7. Show count in notification after bulk operation
```plaintext
// After deleting selected items
Remove(Tasks, Filter(Tasks, varIsSelected));
Notify(
    "Deleted " & CountRows(varSelectedItems) & " task(s) successfully.",
    NotificationType.Success,
    4000
)
```

### 8. Persistent notification (user must dismiss manually)
```plaintext
// Timeout = 0 means the banner stays until the user taps it
Notify(
    "⚠️ You have unsaved changes. Navigate away to discard them.",
    NotificationType.Warning,
    0
)
```

### 9. Notify with dynamic status result
```plaintext
// Show different messages based on the save outcome
Set(
    varResult,
    Patch(Projects, Gallery_Projects.Selected, { Status: ddlStatus.Selected.Value })
);
If(
    IsError(varResult),
    Notify("Update failed: " & varResult.Message, NotificationType.Error, 6000),
    Notify(
        "Status changed to '" & ddlStatus.Selected.Value & "' for " & Gallery_Projects.Selected.Title,
        NotificationType.Success
    )
)
```

### 10. Notify inside ForAll — report progress
```plaintext
// Update all selected items and report total updated
Set(varUpdated, 0);
ForAll(
    Filter(varCart, IsSelected = true),
    Patch(Orders, Defaults(Orders), { ProductID: ProductID, Qty: Qty });
    Set(varUpdated, varUpdated + 1)
);
Notify(
    Text(varUpdated, "0") & " order(s) placed successfully!",
    NotificationType.Success,
    3000
)
```

### 11. Combine Notify with Navigate for a smooth UX
```plaintext
// Form OnSuccess — notify then navigate
Notify("Project '" & Form1.LastSubmit.Title & "' created!", NotificationType.Success, 3000);
Navigate(ProjectListScreen, ScreenTransition.UnCover)
```

### 12. App-level error handler using Notify
```plaintext
// App OnError property — catch unhandled errors globally
Notify(
    "An unexpected error occurred: " & Err.Message,
    NotificationType.Error,
    8000
);
Trace("AppError: " & Err.Message & " | Source: " & Err.Source, TraceSeverity.Critical)
```

---

## Best Practices

1. **Always pair with `SubmitForm` / `Patch`** — add to both `OnSuccess` (success type) and `OnFailure` (error type).
2. **Use longer timeouts for errors** — 5000–8000 ms gives users time to read error messages.
3. **Keep messages short and specific** — "Invoice #1042 saved" is better than "Saved successfully".
4. **Use `0` timeout sparingly** — persistent notifications block the UI; use only for critical warnings.
5. **Include dynamic values** in messages (record title, count) to make feedback meaningful.
6. **Use `Warning` before destructive actions**, not just after failures.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Omitting `NotificationType` on errors | Notification appears blue (info), not red | Always specify the type explicitly |
| Short timeout on error messages | User misses the message | Use 5000–8000 ms for errors |
| Calling `Notify` in a calculated property | Behavior function — won't run | Move to `OnSelect`, `OnSuccess`, etc. |
| Long messages | Notification gets cut off on small screens | Keep under ~80 characters |

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`SubmitForm`](./SubmitForm.md) | Trigger `Notify` in `OnSuccess` / `OnFailure` |
| [`Patch`](./Patch.md) | Notify after programmatic saves |
| [`If`](./If.md) | Drive which notification type to show |
| [`IsBlank`](./IsBlank.md) | Validate before notifying |
| [`Trace`](./Trace.md) | Log errors alongside `Notify` |

---

## 🔗 Official Documentation
[Notify function – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-notify)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*
