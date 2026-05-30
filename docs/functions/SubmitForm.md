# 📨 SubmitForm

> **Category:** Forms | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## 📋 Table of Contents
- [Overview](#overview)
- [Syntax](#syntax)
- [Parameters](#parameters)
- [Form Modes](#form-modes)
- [Simple Examples](#simple-examples)
- [Complex Examples](#complex-examples)
- [Key Form Properties](#key-form-properties)
- [SubmitForm vs Patch](#submitform-vs-patch)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)
- [Related Functions](#related-functions)

---

## Overview

`SubmitForm` saves the data entered in a **Form control** to its connected data source. It automatically handles both **creating new records** (when the form is in New mode) and **updating existing records** (when in Edit mode). It also triggers built-in field-level validation from data cards.

```
User fills form  →  Clicks Save  →  SubmitForm(Form1)
                                          │
                          ┌───────────────┴───────────────┐
                          ▼                               ▼
                    Form.Valid = true             Form.Valid = false
                          │                               │
                    Writes to data source         OnFailure fires
                          │
                    OnSuccess fires
```

---

## Syntax

```plaintext
SubmitForm( FormName )
```

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `FormName` | Form Control | ✅ Yes | The name of the Edit Form control to submit |

---

## Form Modes

| Mode | Set With | Action on Submit |
|------|----------|-----------------|
| `FormMode.New` | `NewForm(Form1)` | Creates a new record in the data source |
| `FormMode.Edit` | `EditForm(Form1)` | Updates the bound existing record |
| `FormMode.View` | `ViewForm(Form1)` | Read-only — `SubmitForm` has no effect |

---

## Simple Examples

### 1. Basic submit on button click
```plaintext
// Button OnSelect
SubmitForm(Form1)
```

### 2. Submit with a validity pre-check
```plaintext
// Only submit if all required fields are valid
If(
    Form1.Valid,
    SubmitForm(Form1),
    Notify("Please fill in all required fields.", NotificationType.Warning)
)
```

### 3. Navigate after success
```plaintext
// Form1 OnSuccess property
Navigate(HomeScreen, ScreenTransition.UnCover);
Notify("Record saved successfully!", NotificationType.Success)
```

### 4. Show error on failure
```plaintext
// Form1 OnFailure property
Notify("Save failed: " & Form1.Error, NotificationType.Error)
```

### 5. Reset after submit for back-to-back entry
```plaintext
// Form1 OnSuccess property
NewForm(Form1);
Notify("Entry saved. Add another!", NotificationType.Success)
```

---

## Complex Examples

### 6. Full save workflow with validation, success, and failure handling
```plaintext
// Save button OnSelect
If(
    IsBlank(DataCardValue_Title.Text),
    Notify("Title is required.", NotificationType.Error),
    IsBlank(DataCardValue_DueDate.SelectedDate),
    Notify("Due date is required.", NotificationType.Error),
    Not(Form1.Valid),
    Notify("Please correct form errors before saving.", NotificationType.Warning),
    SubmitForm(Form1)
)

// Form1 OnSuccess
Set(varLastSavedID, Form1.LastSubmit.ID);
Notify("Task #" & varLastSavedID & " saved!", NotificationType.Success);
Navigate(TaskListScreen, ScreenTransition.UnCover)

// Form1 OnFailure
Notify("Could not save: " & Form1.Error, NotificationType.Error);
Trace("SubmitForm failure: " & Form1.Error, TraceSeverity.Error)
```

### 7. Capture the newly created record and use its ID immediately
```plaintext
// Form1 OnSuccess — get the auto-generated ID and create a child record
Set(varNewParent, Form1.LastSubmit);
Patch(
    AuditLog,
    Defaults(AuditLog),
    {
        Action:     "Created",
        RecordID:   varNewParent.ID,
        RecordTitle:varNewParent.Title,
        ActionBy:   User().Email,
        ActionOn:   Now()
    }
);
Navigate(DetailScreen, ScreenTransition.Cover, { CurrentRecord: varNewParent })
```

### 8. Conditional form mode — add or edit from one screen
```plaintext
// "Add New" button
NewForm(EditForm1);
Navigate(FormScreen, ScreenTransition.Cover)

// "Edit" button in gallery
EditForm(EditForm1);
Navigate(FormScreen, ScreenTransition.Cover)
// Form1.Item = Gallery1.Selected  ← set in the Item property of the form

// Save button on FormScreen
If(
    Form1.Valid,
    SubmitForm(Form1),
    Notify("Fix errors before saving.", NotificationType.Warning)
)

// Form1 OnSuccess
Notify(
    If(Form1.Mode = FormMode.New, "Record created!", "Record updated!"),
    NotificationType.Success
);
Navigate(ListScreen, ScreenTransition.UnCover)
```

### 9. Multi-form screen — submit two forms together
```plaintext
// Save button that submits a header form and a details form
If(
    FormHeader.Valid And FormDetails.Valid,
    SubmitForm(FormHeader),
    Notify("Please complete all required fields.", NotificationType.Warning)
)

// FormHeader OnSuccess — now submit details form linking to the header
Set(varHeaderID, FormHeader.LastSubmit.ID);
SubmitForm(FormDetails)

// FormDetails OnSuccess
Patch(
    FormDetails.DataSource,
    FormDetails.LastSubmit,
    { ParentID: varHeaderID }
);
Notify("Both forms saved!", NotificationType.Success);
Navigate(HomeScreen, ScreenTransition.Fade)
```

### 10. Auto-save on a timer
```plaintext
// Timer Control — Duration: 30000, AutoStart: true, Repeat: true
// Timer OnTimerEnd:
If(
    Form1.Unsaved And Form1.Valid,
    SubmitForm(Form1)
)

// Form1 OnSuccess (auto-save path — no navigation)
Notify("Auto-saved at " & Text(Now(), "hh:mm AM/PM"), NotificationType.Information)
```

---

## Key Form Properties

| Property | Type | Description |
|----------|------|-------------|
| `Form.Valid` | Boolean | `true` if all data cards pass their validation rules |
| `Form.Error` | Text | Error message from the last failed submit |
| `Form.LastSubmit` | Record | The record that was last successfully submitted |
| `Form.Mode` | Enum | `FormMode.New`, `FormMode.Edit`, or `FormMode.View` |
| `Form.Unsaved` | Boolean | `true` if user has changed any field since last save |
| `Form.Updates` | Record | Record of current field values (before submit) |
| `Form.Item` | Record | The record bound to the form (set to `Gallery.Selected` or a variable) |

---

## SubmitForm vs Patch

| Feature | `SubmitForm` | `Patch` |
|---------|--------------|---------|
| Requires Form control on screen | ✅ Yes | ❌ No |
| Built-in field validation | ✅ Yes (data card rules) | ❌ Manual |
| Returns saved record | Via `LastSubmit` | Directly as return value |
| Write specific fields only | ❌ Writes all cards | ✅ Full control |
| Bulk / background writes | ❌ No | ✅ Yes |
| Simplest for user-facing forms | ✅ Yes | ❌ More code |

---

## Best Practices

1. **Always set `OnSuccess` and `OnFailure`** — never leave them blank.
2. **Check `Form.Valid` first** to give early feedback before calling `SubmitForm`.
3. **Use `Form.LastSubmit`** in `OnSuccess` to get the saved record (including server-assigned ID).
4. **Don't navigate inside `OnSelect`** after `SubmitForm` — navigate inside `OnSuccess` instead, so the write completes first.
5. **Pair with `NewForm` / `EditForm`** to control mode before the user reaches the screen.
6. **Log errors** in `OnFailure` using `Trace()` for better debugging in Monitor.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Navigating away in the same `OnSelect` as `SubmitForm` | Screen changes before save completes | Put `Navigate` in `Form.OnSuccess` |
| Using `SubmitForm` without setting `Form.Item` | Form submits blank data | Set `Form.Item = Gallery1.Selected` or a variable |
| Ignoring `Form.Error` in `OnFailure` | User sees nothing when save fails | Add `Notify(Form.Error, ...)` to `OnFailure` |
| Calling `SubmitForm` in `FormMode.View` | Nothing happens silently | Use `EditForm(Form1)` before `SubmitForm` |

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`NewForm`](./NewForm.md) | Set form to create mode before `SubmitForm` |
| [`EditForm`](./EditForm.md) | Set form to edit mode before `SubmitForm` |
| [`ResetForm`](./ResetForm.md) | Discard changes instead of submitting |
| [`ViewForm`](./ViewForm.md) | Switch to read-only after submit |
| [`Patch`](./Patch.md) | Code-first alternative to `SubmitForm` |
| [`Notify`](./Notify.md) | Show feedback in `OnSuccess` / `OnFailure` |
| [`Navigate`](./Navigate.md) | Navigate after successful submit |

---

## 🔗 Official Documentation
[SubmitForm, EditForm, ResetForm, ViewForm – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-form)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*
