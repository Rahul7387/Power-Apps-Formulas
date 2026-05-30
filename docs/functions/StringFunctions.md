# 🔤 String Functions

> **Category:** Text | **Works in:** Canvas Apps

---

## Overview

Reference for all common text manipulation functions: Trim, TrimEnds, Upper, Lower, Proper, Len, Left, Right, Mid, Find, Substitute, Replace, Char, EncodeUrl, IsMatch.

---

## Syntax

```plaintext
Trim(t)  Upper(t)  Lower(t)  Proper(t)  Len(t)
Left(t,n)  Right(t,n)  Mid(t,start,n)
Find(needle,haystack)  Substitute(t,old,new)  Replace(t,start,n,new)
```

---

## Simple Examples

### 1. Case conversions
```plaintext
Upper("hello")    // "HELLO"
Lower("WORLD")    // "world"
Proper("john doe")// "John Doe"
```

### 2. Substring functions
```plaintext
Left("Hello", 3)     // "Hel"
Right("Hello", 3)    // "llo"
Mid("Hello", 2, 3)   // "ell"
Len("Hello")         // 5
```

### 3. Find and Substitute
```plaintext
Find("lo", "Hello")             // 4
Substitute("Hello World", "World", "Power Apps") // "Hello Power Apps"
```

---

## Complex Examples

### 4. Generate a username from name input
```plaintext
Lower(Left(Trim(txtFirstName.Text), 1) & Trim(txtLastName.Text))
// "John Smith" → "jsmith"
```

### 5. Build a reference code
```plaintext
Upper(Left(ddlCategory.Selected.Value, 3)) &
Text(Year(Today()), "0000") & "-" &
Text(Gallery1.Selected.ID, "000")
// → "ELE2026-042"
```

### 6. Sanitise and normalise inputs before Patch
```plaintext
Patch(Contacts, Defaults(Contacts), {
    Name:    Proper(Trim(txtName.Text)),
    Email:   Lower(Trim(txtEmail.Text)),
    Phone:   Substitute(Substitute(Trim(txtPhone.Text), " ", ""), "-", ""),
    PostCode:Upper(Trim(txtPostCode.Text))
})
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
| [`IsMatch`](./IsMatch.md) | Related function |
| [`Split`](./Split.md) | Related function |
| [`Value`](./Value.md) | Related function |

---

## 🔗 Official Documentation
[Text functions – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-text)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*