# 🤝 Coalesce

> **Category:** Logic | **Works in:** Canvas Apps

---

## Overview

Returns the first non-blank value from a list of arguments. A cleaner alternative to If(IsBlank(...), default, value) chains.

---

## Syntax

```plaintext
Coalesce( Value1 [, Value2, Value3, ...] )
```

---

## Simple Examples

### 1. Fallback to default text
```plaintext
Coalesce(txtName.Text, "Unknown")
```

### 2. Multiple fallbacks
```plaintext
Coalesce(varRecord.Notes, Gallery1.Selected.Notes, "No notes available.")
```

### 3. App setting with fallback
```plaintext
Coalesce(LookUp(Settings, Key="AppTitle").Value, "My Power App")
```

---

## Complex Examples

### 4. Safe defaults in Patch
```plaintext
Patch(Contacts, Defaults(Contacts), {
    Name:    txtName.Text,
    Email:   txtEmail.Text,
    Phone:   Coalesce(txtPhone.Text, "Not provided"),
    Company: Coalesce(txtCompany.Text, "Individual"),
    Region:  Coalesce(ddlRegion.Selected.Value, varDefaultRegion)
})
```

### 5. Create-or-update base record
```plaintext
Set(varExisting, LookUp(UserPrefs, Email = User().Email));
Patch(
    UserPrefs,
    Coalesce(varExisting, Defaults(UserPrefs)),
    { Email: User().Email, Theme: ddlTheme.Selected.Value }
)
```

### 6. Cascade fallback for a display label
```plaintext
// Show item note → category default note → global default
Coalesce(
    ThisItem.CustomNote,
    LookUp(CategoryDefaults, Category = ThisItem.Category).DefaultNote,
    "No notes for this item."
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
| [`Defaults`](./Defaults.md) | Related function |
| [`LookUp`](./LookUp.md) | Related function |

---

## 🔗 Official Documentation
[Coalesce – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-isblank-isempty)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*