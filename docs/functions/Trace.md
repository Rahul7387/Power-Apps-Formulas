# 🔍 Trace

> **Category:** Debugging | **Works in:** Canvas Apps

---

## Overview

Logs a custom message (and optional data record) to the Power Apps Monitor tool during testing and debugging.

---

## Syntax

```plaintext
Trace( Message [, TraceSeverity [, CustomRecord]] )
```

---

## Simple Examples

### 1. Simple log message
```plaintext
Trace("Button clicked")
```

### 2. Log with severity
```plaintext
Trace("User navigated to home", TraceSeverity.Information)
```

### 3. Log an error
```plaintext
Trace("Form submit failed: " & Form1.Error, TraceSeverity.Error)
```

---

## Complex Examples

### 4. OnSuccess logging with context
```plaintext
// Form1 OnSuccess
Trace(
    "Record submitted",
    TraceSeverity.Information,
    { UserEmail: User().Email, RecordID: Form1.LastSubmit.ID, Mode: Form1.Mode }
)
```

### 5. Global error handler
```plaintext
// App OnError property
Trace(
    "Unhandled error",
    TraceSeverity.Critical,
    { Message: Err.Message, Source: Err.Source, User: User().Email }
);
Notify("An error occurred. Support has been notified.", NotificationType.Error)
```

### 6. Performance checkpoint logging
```plaintext
Trace("Screen loaded", TraceSeverity.Information, { Screen: "ProjectList", RecordCount: CountRows(varProjects), LoadTime: DateDiff(varStartTime, Now(), Milliseconds) })
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
| [`IsError`](./IsError.md) | Related function |
| [`Notify`](./Notify.md) | Related function |
| [`Patch`](./Patch.md) | Related function |

---

## 🔗 Official Documentation
[Trace – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-trace)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*