# 🚀 Launch

> **Category:** Navigation | **Works in:** Canvas Apps

---

## Overview

Opens a URL, email, phone link, or another Power App in the system browser or default handler.

---

## Syntax

```plaintext
Launch( URL [, Parameter, Value, ...] )
```

---

## Simple Examples

### 1. Open a website
```plaintext
Launch("https://www.microsoft.com")
```

### 2. Open email client
```plaintext
Launch("mailto:" & Gallery1.Selected.Email & "?subject=Follow Up")
```

### 3. Open phone dialer
```plaintext
Launch("tel:" & Gallery1.Selected.Phone)
```

---

## Complex Examples

### 4. Open another Canvas App with parameters
```plaintext
Launch(
    "https://apps.powerapps.com/play/" & varAppID,
    "CustomerID", Gallery1.Selected.ID,
    "Mode",       "Edit"
)
```

### 5. Open a SharePoint file directly
```plaintext
Launch(ThisItem.FileServerRelativeUrl)
```

### 6. Pre-filled email with encoded body
```plaintext
Launch(
    "mailto:" & varManagerEmail &
    "?subject=" & EncodeUrl("Approval: " & Gallery1.Selected.Title) &
    "&body=" & EncodeUrl("Please review request #" & Gallery1.Selected.ID &
    ". Due: " & Text(Gallery1.Selected.DueDate, "dd MMM yyyy"))
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
| [`Navigate`](./Navigate.md) | Related function |
| [`EncodeUrl`](./EncodeUrl.md) | Related function |
| [`Back`](./Back.md) | Related function |

---

## 🔗 Official Documentation
[Launch – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-param)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*