# ⬅️ Back

> **Category:** Navigation | **Works in:** Canvas Apps

---

## Overview

Returns to the previously displayed screen using the navigation history stack — the complement to Navigate with Cover transition.

---

## Syntax

```plaintext
Back( [Transition] )
```

---

## Simple Examples

### 1. Simple back
```plaintext
Back()
```

### 2. With UnCover transition
```plaintext
Back(ScreenTransition.UnCover)
```

### 3. Cancel button on a form screen
```plaintext
ResetForm(EditForm1);
Back(ScreenTransition.UnCover)
```

---

## Complex Examples

### 4. Conditional back — fallback if no history
```plaintext
If(
    IsBlank(varOriginScreen),
    Navigate(HomeScreen, ScreenTransition.Fade),
    Back(ScreenTransition.UnCover)
)
```

### 5. Confirm discard before back
```plaintext
If(
    EditForm1.Unsaved,
    UpdateContext({ showDiscardModal: true }),
    Back(ScreenTransition.UnCover)
)
// Modal confirm button:
ResetForm(EditForm1);
UpdateContext({ showDiscardModal: false });
Back(ScreenTransition.UnCover)
```

### 6. Back after OnSuccess with audit log
```plaintext
// Form1 OnSuccess:
Patch(AuditLog, Defaults(AuditLog), {
    Action: "Update", RecordID: Form1.LastSubmit.ID,
    By: User().Email, On: Now()
});
Notify("Saved!", NotificationType.Success);
Back(ScreenTransition.UnCover)
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
| [`Navigate`](./Navigate.md) | Related function |
| [`ResetForm`](./ResetForm.md) | Related function |
| [`UpdateContext`](./UpdateContext.md) | Related function |

---

## 🔗 Official Documentation
[Navigate and Back – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-navigate)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*