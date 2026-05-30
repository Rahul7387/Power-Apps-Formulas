# 👁️ ViewForm

> **Category:** Forms | **Works in:** Canvas Apps

---

## Overview

Sets a Form control to View (read-only) mode, preventing user edits.

---

## Syntax

```plaintext
ViewForm( FormName )
```

---

## Simple Examples

### 1. Switch to view after save
```plaintext
// Form1 OnSuccess:
ViewForm(Form1);
Notify("Changes saved.", NotificationType.Success)
```

### 2. Read-only detail view on screen load
```plaintext
// Screen OnVisible:
ViewForm(DetailForm)
```

### 3. Toggle between view and edit
```plaintext
// Edit button: EditForm(Form1)
// Cancel button: ViewForm(Form1); ResetForm(Form1)
```

---

## Complex Examples

### 4. Full view-edit-save cycle
```plaintext
// Detail screen — initially view mode
// Screen OnVisible: ViewForm(Form1)
// Edit button: EditForm(Form1); UpdateContext({isEditing: true})
// Save button: SubmitForm(Form1)
// Form1 OnSuccess: ViewForm(Form1); UpdateContext({isEditing: false})
// Cancel: ResetForm(Form1); ViewForm(Form1); UpdateContext({isEditing: false})
```

### 5. View mode with selective edit permission
```plaintext
If(
    varCanEdit,
    EditForm(Form1),
    Notify("You do not have edit permissions.", NotificationType.Warning)
)
```

### 6. Auto-switch to view after timeout
```plaintext
// Timer AutoStart:true Duration:300000 OnTimerEnd:
If(EditForm1.Mode = FormMode.Edit, ViewForm(EditForm1))
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
| [`EditForm`](./EditForm.md) | Related function |
| [`ResetForm`](./ResetForm.md) | Related function |
| [`SubmitForm`](./SubmitForm.md) | Related function |
| [`NewForm`](./NewForm.md) | Related function |

---

## 🔗 Official Documentation
[EditForm, ResetForm, SubmitForm – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-form)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*