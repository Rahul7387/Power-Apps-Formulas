# 📝 Text

> **Category:** Text | **Works in:** Canvas Apps

---

## Overview

Formats a number, date, or value as a text string using a format pattern — the primary formatting function in Power Fx.

---

## Syntax

```plaintext
Text( Value [, FormatString] )
```

---

## Simple Examples

### 1. Format as currency
```plaintext
Text(1234.5, "$#,##0.00")  // → "$1,234.50"
```

### 2. Format a date
```plaintext
Text(Today(), "dd mmm yyyy")  // → "30 May 2026"
```

### 3. Format as percentage
```plaintext
Text(0.125, "0.0%")  // → "12.5%"
```

---

## Complex Examples

### 4. Invoice summary label
```plaintext
"Subtotal: " & Text(varSubtotal, "$#,##0.00") & Char(10) &
"Tax (18%): " & Text(varTax, "$#,##0.00") & Char(10) &
"Total: " & Text(varTotal, "$#,##0.00")
```

### 5. Locale-aware date format
```plaintext
Text(ThisItem.DueDate, "[$-en-IN]dd/mm/yyyy")   // India
Text(ThisItem.DueDate, "[$-en-US]mmmm d, yyyy") // US
Text(Now(), "yyyy-mm-dd_hh-mm")                 // file-safe
```

### 6. KPI with colour and percentage
```plaintext
// Label Text:
Text(varRevenue, "$#,##0") & " (" & Text(varRevenue/varTarget, "0.0%") & ")"
// Label Color:
If(varRevenue >= varTarget, Color.Green, Color.Red)
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
| [`Concatenate`](./Concatenate.md) | Related function |
| [`Value`](./Value.md) | Related function |
| [`Round`](./Round.md) | Related function |
| [`DateFunctions`](./DateFunctions.md) | Related function |

---

## 🔗 Official Documentation
[Text – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-text)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*