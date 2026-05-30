# ➕ NewForm

> **Category:** Forms | **Works in:** Canvas Apps

---

## Overview

Sets a Form control to New (create) mode, clearing all fields so the user can enter a fresh record.

---

## Syntax

```plaintext
NewForm( FormName )
```

---

## Simple Examples

### 1. Add new button
```plaintext
NewForm(EditForm1);
Navigate(FormScreen, ScreenTransition.Cover)
```

### 2. Reset to new mode after submit
```plaintext
// Form1 OnSuccess:
NewForm(Form1);
Notify("Saved! Add another.", NotificationType.Success)
```

### 3. New form with pre-filled context
```plaintext
NewForm(EditForm1);
Navigate(FormScreen, ScreenTransition.Cover, { defaultCategory: ddlFilter.Selected.Value })
```

---

## Complex Examples

### 4. Conditional new vs edit navigation
```plaintext
// New button:
Set(varFormRecord, Blank());
NewForm(EditForm1);
Navigate(FormScreen)
// Edit button:
Set(varFormRecord, Gallery1.Selected);
EditForm(EditForm1);
Navigate(FormScreen)
// Form1.Item: varFormRecord
```

### 5. Post-submit cycling with counter
```plaintext
// Form1 OnSuccess:
Set(varSavedCount, varSavedCount + 1);
NewForm(Form1);
Notify(Text(varSavedCount,"0") & " record(s) saved this session.", NotificationType.Success)
```

### 6. New form for child record
```plaintext
// After selecting a parent project, open form for a new task
Set(varParentProject, Gallery_Projects.Selected);
NewForm(TaskForm);
Navigate(NewTaskScreen, ScreenTransition.Cover)
// On NewTaskScreen — DataCardValue_ProjectID.Default: varParentProject.ID
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
| [`Defaults`](./Defaults.md) | Related function |

---

## 🔗 Official Documentation
[EditForm, ResetForm, SubmitForm – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-form)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*