# 🔵 Round / RoundUp / RoundDown

> **Category:** Math | **Works in:** Canvas Apps

---

## Overview

Rounds a number to a specified number of decimal places. RoundUp always rounds away from zero; RoundDown always toward zero.

---

## Syntax

```plaintext
Round( Number, DecimalPlaces )
RoundUp( Number, DecimalPlaces )
RoundDown( Number, DecimalPlaces )
```

---

## Simple Examples

### 1. Round to 2 decimals
```plaintext
Round(3.567, 2)    // → 3.57
```

### 2. Always round up
```plaintext
RoundUp(3.001, 2)  // → 3.01
```

### 3. Always round down
```plaintext
RoundDown(3.999, 2) // → 3.99
```

---

## Complex Examples

### 4. Cart total with tax
```plaintext
Set(varSubtotal, Sum(varCart, Price * Qty));
Set(varTax,      Round(varSubtotal * 0.18, 2));
Set(varTotal,    varSubtotal + varTax)
```

### 5. Packs to order (ceiling division)
```plaintext
// Units needed → packs (sold in 12s)
"Packs needed: " & RoundUp(Value(txtUnits.Text) / 12, 0)
```

### 6. Display clean averages on dashboard
```plaintext
// KPI label
Text(Round(Average(Sales, Amount), 0), "$#,##0")
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
| [`Sum`](./Sum.md) | Related function |
| [`Text`](./Text.md) | Related function |
| [`Value`](./Value.md) | Related function |

---

## 🔗 Official Documentation
[Round, RoundUp, RoundDown – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-round)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*