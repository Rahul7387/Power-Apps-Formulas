# 🔗 EncodeUrl / JSON

> **Category:** Utilities | **Works in:** Canvas Apps

---

## Overview

EncodeUrl percent-encodes a string for safe use in a URL. JSON converts a value to a JSON-formatted text string.

---

## Syntax

```plaintext
EncodeUrl( Text )
JSON( Value [, JSONFormat.IndentFour | JSONFormat.FlattenValueTables | ...] )
```

---

## Simple Examples

### 1. Encode a title for a URL
```plaintext
EncodeUrl(Gallery1.Selected.Title)
// "Hello World" → "Hello%20World"
```

### 2. Open pre-filled mailto link
```plaintext
Launch("mailto:support@co.com?subject=" & EncodeUrl("Issue: " & Gallery1.Selected.Title))
```

### 3. Convert record to JSON string
```plaintext
JSON({ Name: "John", Age: 30 })
// → "{\"Name\":\"John\",\"Age\":30}"
```

---

## Complex Examples

### 4. Full encoded email with body
```plaintext
Launch(
    "mailto:" & varManager.Email &
    "?subject=" & EncodeUrl("[Approval] " & Gallery1.Selected.Title) &
    "&body=" & EncodeUrl(
        "Record ID: " & Gallery1.Selected.ID & Char(10) &
        "Requested by: " & User().FullName & Char(10) &
        "Due: " & Text(Gallery1.Selected.DueDate, "dd MMM yyyy")
    )
)
```

### 5. JSON snapshot in audit log
```plaintext
Patch(AuditLog, Defaults(AuditLog), {
    Action:     "Update",
    RecordID:   Gallery1.Selected.ID,
    DataBefore: JSON(Gallery1.Selected, JSONFormat.IndentFour),
    ChangedBy:  User().Email,
    ChangedOn:  Now()
})
```

### 6. Build a Power Automate trigger URL with parameters
```plaintext
Launch(
    varFlowTriggerURL &
    "?customerID=" & EncodeUrl(Text(varCustomer.ID)) &
    "&action=" & EncodeUrl("SendWelcomeEmail") &
    "&language=" & EncodeUrl(varUserLanguage)
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
| [`Launch`](./Launch.md) | Related function |
| [`Concatenate`](./Concatenate.md) | Related function |
| [`JSON`](./JSON.md) | Related function |
| [`Patch`](./Patch.md) | Related function |

---

## 🔗 Official Documentation
[EncodeUrl – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-encode-decode)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*