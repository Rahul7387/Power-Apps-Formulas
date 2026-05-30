# ❗ IsError / IfError

> **Category:** Error Handling | **Works in:** Canvas Apps

---

## Overview

IsError tests if a value is an error record. IfError evaluates an expression and returns an alternative value if it results in an error.

---

## Syntax

```plaintext
IsError( Value )
IfError( Value, Fallback [, Value2, Fallback2, ...] )
```

---

## Simple Examples

### 1. Check Patch result
```plaintext
Set(varResult, Patch(Tasks, Defaults(Tasks), { Title: txtTitle.Text }));
If(IsError(varResult), Notify("Save failed.", NotificationType.Error))
```

### 2. Safe numeric conversion
```plaintext
IfError(Value(txtQty.Text), 0)
```

### 3. Fallback for failed LookUp
```plaintext
IfError(LookUp(Products, ID = varID).Price, 0)
```

---

## Complex Examples

### 4. Full error-aware save with navigate
```plaintext
Set(varResult, Patch(Orders, Defaults(Orders), { Title: txtTitle.Text, Total: varTotal }));
If(
    IsError(varResult),
    Notify("Save failed: " & varResult.Message, NotificationType.Error, 6000);
    Trace("Patch failed", TraceSeverity.Error, { Msg: varResult.Message }),
    Notify("Order #" & varResult.ID & " created!", NotificationType.Success);
    Navigate(OrdersScreen)
)
```

### 5. IfError chain — live source with cache fallback
```plaintext
Set(varData, IfError(
    Filter(LiveData, AssignedTo = User().Email),
    varCachedData
))
```

### 6. Safe formula in a label (never show errors)
```plaintext
// Label Text — show calculated value or dash if it errors
IfError(
    Text(Gallery1.Selected.UnitPrice * Value(txtQty.Text), "$#,##0.00"),
    "—"
)
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
| [`Patch`](./Patch.md) | Related function |
| [`Notify`](./Notify.md) | Related function |
| [`Trace`](./Trace.md) | Related function |
| [`IfError`](./IfError.md) | Related function |

---

## 🔗 Official Documentation
[IsError, IfError – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-iferror)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*