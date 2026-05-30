# ✅ IsMatch

> **Category:** Validation | **Works in:** Canvas Apps

---

## Overview

Tests whether a text string matches a pattern — either a built-in Power Fx pattern enum or a custom regular expression.

---

## Syntax

```plaintext
IsMatch( Text, Pattern [, MatchOptions.Complete | MatchOptions.Contains | ...] )
```

---

## Simple Examples

### 1. Validate email format
```plaintext
IsMatch(txtEmail.Text, Email)
```

### 2. 10-digit phone number
```plaintext
IsMatch(txtPhone.Text, "^[0-9]{10}$")
```

### 3. Only digits allowed
```plaintext
IsMatch(txtCode.Text, MultipleDigits)
```

---

## Complex Examples

### 4. Full form validation with IsMatch
```plaintext
If(
    IsBlank(txtEmail.Text),       Notify("Email required",       NotificationType.Error),
    !IsMatch(txtEmail.Text, Email),Notify("Invalid email",        NotificationType.Error),
    !IsMatch(txtPhone.Text, "^[0-9]{10}$"), Notify("Invalid phone", NotificationType.Error),
    SubmitForm(Form1)
)
```

### 5. Password strength check
```plaintext
And(
    Len(txtPassword.Text) >= 8,
    IsMatch(txtPassword.Text, ".*[A-Z].*"),      // uppercase
    IsMatch(txtPassword.Text, ".*[0-9].*"),      // digit
    IsMatch(txtPassword.Text, ".*[^a-zA-Z0-9].*")// special char
)
```

### 6. Validate custom reference code format
```plaintext
// Format: AAA-2026-0042
If(
    !IsMatch(txtRef.Text, "[A-Z]{3}-[0-9]{4}-[0-9]{4}"),
    Notify("Format must be AAA-YYYY-0000", NotificationType.Warning)
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
| [`IsBlank`](./IsBlank.md) | Related function |
| [`If`](./If.md) | Related function |
| [`Notify`](./Notify.md) | Related function |
| [`Text`](./Text.md) | Related function |

---

## 🔗 Official Documentation
[IsMatch, Match, MatchAll – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-ismatch)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*