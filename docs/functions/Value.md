# 🔢 Value

> **Category:** Type Conversion | **Works in:** Canvas Apps

---

## Overview

Converts a text string to a number. Required when reading from text input controls for arithmetic operations.

---

## Syntax

```plaintext
Value( Text )
```

---

## Simple Examples

### 1. Multiply text input by price
```plaintext
Value(txtQty.Text) * Gallery1.Selected.UnitPrice
```

### 2. Safe conversion with fallback
```plaintext
If(IsBlank(txtAmount.Text), 0, Value(txtAmount.Text))
```

### 3. Patch with numeric value
```plaintext
Patch(Products, Gallery1.Selected, { Stock: Value(txtStock.Text) })
```

---

## Complex Examples

### 4. Full order line calculation
```plaintext
With(
    { qty: Value(txtQty.Text), price: Value(txtPrice.Text) },
    Collect(varOrderLines, {
        ProductID: ddlProduct.Selected.ID,
        Qty:       qty,
        UnitPrice: price,
        LineTotal: Round(qty * price, 2)
    })
)
```

### 5. Progress bar from text input
```plaintext
// Rectangle Width:
(If(IsBlank(txtPercent.Text), 0, Value(txtPercent.Text)) / 100) * Parent.Width
```

### 6. Validate numeric range before save
```plaintext
Set(varAge, IfError(Value(txtAge.Text), -1));
If(
    varAge < 0,      Notify("Age must be a number.",   NotificationType.Error),
    varAge < 18,     Notify("Must be 18 or older.",    NotificationType.Error),
    varAge > 120,    Notify("Please enter a valid age.",NotificationType.Error),
    SubmitForm(Form1)
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
| [`Text`](./Text.md) | Related function |
| [`IfError`](./IfError.md) | Related function |
| [`IsBlank`](./IsBlank.md) | Related function |
| [`Round`](./Round.md) | Related function |

---

## 🔗 Official Documentation
[Value – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-value)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*