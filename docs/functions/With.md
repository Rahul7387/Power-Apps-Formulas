# 🧩 With

> **Category:** Logic / Variables | **Works in:** Canvas Apps

---

## Overview

Evaluates a formula within a named record scope — eliminates repeated sub-expressions and makes complex formulas more readable.

---

## Syntax

```plaintext
With( { Name: Value [, Name2: Value2, ...] }, Formula )
```

---

## Simple Examples

### 1. Avoid repeating a calculation
```plaintext
With({ total: Sum(varCart, Price * Qty) }, "Cart: " & Text(total, "$#,##0") & " (" & CountRows(varCart) & " items)")
```

### 2. Clean label from a lookup chain
```plaintext
With(
    { emp: LookUp(Employees, Email = User().Email) },
    emp.Name & " — " & emp.Department
)
```

### 3. Pre-compute before Patch
```plaintext
With(
    { sub: Sum(varCart, Price * Qty), tax: 0.18 },
    Patch(Orders, Defaults(Orders), { Subtotal: sub, Tax: Round(sub*tax,2), Total: Round(sub*(1+tax),2) })
)
```

---

## Complex Examples

### 4. Nested With for step-by-step finance calc
```plaintext
With(
    { basePrice: Gallery1.Selected.Price },
    With(
        { memberPrice: If(varIsMember, basePrice * 0.9, basePrice) },
        With(
            { finalPrice: Round(memberPrice * 1.18, 2) },
            "Final: " & Text(finalPrice, "$#,##0.00")
        )
    )
)
```

### 5. With to simplify a complex filter
```plaintext
With(
    {
        today:    Today(),
        userEmail:User().Email
    },
    Filter(Tasks,
        AssignedTo = userEmail &&
        DueDate >= today &&
        DueDate <= DateAdd(today, 7, Days)
    )
)
```

### 6. Build a summary record cleanly
```plaintext
With(
    {
        sales:    Filter(Orders, Month(OrderDate) = Month(Today())),
        prev:     Filter(Orders, Month(OrderDate) = Month(DateAdd(Today(),-1,Months)))
    },
    Set(varMonthlySummary, {
        ThisMonth: Sum(sales, Total),
        LastMonth: Sum(prev, Total),
        Growth:    (Sum(sales,Total) - Sum(prev,Total)) / Sum(prev,Total)
    })
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
| [`Set`](./Set.md) | Related function |
| [`UpdateContext`](./UpdateContext.md) | Related function |
| [`Coalesce`](./Coalesce.md) | Related function |
| [`If`](./If.md) | Related function |

---

## 🔗 Official Documentation
[With – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-with)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*