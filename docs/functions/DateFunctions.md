# 📅 Date & Time Functions

> **Category:** Date & Time | **Works in:** Canvas Apps

---

## Overview

A group of functions for creating, parsing, and calculating with dates and times: Today, Now, Date, DateAdd, DateDiff, DateValue, Year, Month, Day, Hour, Minute, Weekday.

---

## Syntax

```plaintext
Today()  Now()  DateAdd(date, n, Units)  DateDiff(d1, d2, Units)
Date(year, month, day)  DateValue("text")  Year(date)  Month(date)  Day(date)
```

---

## Simple Examples

### 1. Current date and time
```plaintext
Today()   // 5/30/2026
Now()     // 5/30/2026 14:30 PM
```

### 2. Add days to today
```plaintext
DateAdd(Today(), 30, Days)   // 30 days from today
DateAdd(Today(), -7, Days)   // 7 days ago
```

### 3. Days between two dates
```plaintext
DateDiff(StartDate, Today(), Days)
```

---

## Complex Examples

### 4. Overdue indicator label
```plaintext
If(
    ThisItem.DueDate < Today(),
    "🔴 Overdue by " & DateDiff(ThisItem.DueDate, Today(), Days) & " day(s)",
    ThisItem.DueDate = Today(),
    "🟡 Due Today",
    "🟢 In " & DateDiff(Today(), ThisItem.DueDate, Days) & " day(s)"
)
```

### 5. Fiscal year calculation
```plaintext
"FY" & If(
    Month(Today()) >= 4,
    Year(Today()),
    Year(Today()) - 1
)  // FY2026 for Apr–Mar fiscal year
```

### 6. Patch with all date fields
```plaintext
Patch(Projects, Defaults(Projects), {
    Title:       txtTitle.Text,
    StartDate:   Today(),
    Deadline:    DateAdd(Today(), Value(txtDays.Text), Days),
    CreatedOn:   Now(),
    CreatedBy:   User().Email,
    FiscalMonth: Text(Today(), "[$-en-US]mmm yyyy")
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
| [`Text`](./Text.md) | Related function |
| [`Filter`](./Filter.md) | Related function |
| [`Patch`](./Patch.md) | Related function |

---

## 🔗 Official Documentation
[Date, Time, and DateTime – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-dateadd-datediff)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*