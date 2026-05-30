# ↩️ ResetForm

> **Category:** Forms | **Works in:** Canvas Apps

---

## Overview

Resets all data cards in a Form control back to their original values — discarding any unsaved changes.

---

## Syntax

```plaintext
ResetForm( FormName )
```

---

## Simple Examples

### 1. Cancel button
```plaintext
ResetForm(Form1);
Navigate(ListScreen, ScreenTransition.UnCover)
```

### 2. Discard and go back
```plaintext
ResetForm(EditForm1);
Back(ScreenTransition.UnCover)
```

### 3. Reset after submit for next entry
```plaintext
// Form1 OnSuccess:
ResetForm(Form1); NewForm(Form1);
Notify("Saved! Add another.", NotificationType.Success)
```

---

## Complex Examples

### 4. Confirm discard with modal
```plaintext
If(
    EditForm1.Unsaved,
    UpdateContext({ showDiscardConfirm: true }),
    Back(ScreenTransition.UnCover)
)
// Confirm "Discard" button:
ResetForm(EditForm1);
UpdateContext({ showDiscardConfirm: false });
Back(ScreenTransition.UnCover)
```

### 5. Reset all forms on a multi-form screen
```plaintext
ResetForm(FormHeader);
ResetForm(FormDetails);
Navigate(HomeScreen, ScreenTransition.Fade)
```

### 6. Auto-reset on screen close
```plaintext
// Screen OnHidden property:
ResetForm(EditForm1)
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
| [`EditForm`](./EditForm.md) | Related function |
| [`SubmitForm`](./SubmitForm.md) | Related function |
| [`Back`](./Back.md) | Related function |

---

## 🔗 Official Documentation
[EditForm, ResetForm, SubmitForm – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-form)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*