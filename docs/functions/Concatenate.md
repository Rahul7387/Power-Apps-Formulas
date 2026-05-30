# 🔗 Concatenate / &

> **Category:** Text | **Works in:** Canvas Apps

---

## Overview

Joins two or more text strings into one. The & operator is the preferred shorthand form.

---

## Syntax

```plaintext
String1 & String2  OR  Concatenate( String1, String2 [, ...] )
```

---

## Simple Examples

### 1. Full name from two inputs
```plaintext
txtFirst.Text & " " & txtLast.Text
```

### 2. Greeting with user name
```plaintext
"Hello, " & User().FullName & "!"
```

### 3. Order reference label
```plaintext
"Order #" & Text(Gallery1.Selected.ID, "00000")
```

---

## Complex Examples

### 4. Multi-line address (Char(10) = newline)
```plaintext
txtStreet.Text & Char(10) &
txtCity.Text & ", " & txtState.Text & " " & txtZip.Text
// Set label WhiteSpace = WhiteSpace.PreWrap
```

### 5. Dynamic email with encoded subject
```plaintext
Launch(
    "mailto:" & varManager.Email &
    "?subject=" & EncodeUrl("[Action] " & Gallery1.Selected.Title) &
    "&body=" & EncodeUrl("Request ID: " & Gallery1.Selected.ID)
)
```

### 6. Build a unique file reference code
```plaintext
Upper(Left(ddlCategory.Selected.Value, 3)) &
Text(Year(Today()), "0000") &
"-" &
Text(Gallery1.Selected.ID, "0000")
// → "ELE2026-0042"
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
| [`EncodeUrl`](./EncodeUrl.md) | Related function |
| [`Split`](./Split.md) | Related function |
| [`StringFunctions`](./StringFunctions.md) | Related function |

---

## 🔗 Official Documentation
[Concatenate and & – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-concatenate)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*