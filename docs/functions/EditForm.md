# ✏️ EditForm

> **Category:** Forms | **Works in:** Canvas Apps

---

## Overview

Sets a Form control to Edit mode so the user can modify the currently bound record.

---

## Syntax

```plaintext
EditForm( FormName )
```

---

## Simple Examples

### 1. Edit button on a detail screen
```plaintext
EditForm(DetailForm)
```

### 2. Navigate to edit screen
```plaintext
EditForm(EditForm1);
Navigate(EditScreen, ScreenTransition.Cover)
```

### 3. Toggle inline edit
```plaintext
If(varEditMode, EditForm(Form1), ViewForm(Form1))
```

---

## Complex Examples

### 4. Bind item and navigate in one step
```plaintext
Set(varEditRecord, Gallery1.Selected);
EditForm(EditForm1);
Navigate(EditFormScreen, ScreenTransition.Cover)
// EditForm1.Item: varEditRecord
```

### 5. Edit with change tracking
```plaintext
Set(varOriginalRecord, Gallery1.Selected);
EditForm(EditForm1);
Navigate(EditFormScreen, ScreenTransition.Cover)
// Cancel: Patch back varOriginalRecord if needed
```

### 6. Dynamic form mode — new or edit from one screen
```plaintext
// EditForm1.Item:
If(IsBlank(varFormRecord), Defaults(Projects), varFormRecord)
// Title:
If(IsBlank(varFormRecord), "New Project", "Edit: " & varFormRecord.Title)
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
| [`NewForm`](./NewForm.md) | Related function |
| [`ViewForm`](./ViewForm.md) | Related function |
| [`ResetForm`](./ResetForm.md) | Related function |
| [`SubmitForm`](./SubmitForm.md) | Related function |

---

## 🔗 Official Documentation
[EditForm, ResetForm, SubmitForm – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-form)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*