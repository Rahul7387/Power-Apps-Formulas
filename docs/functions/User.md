# 👤 User

> **Category:** Context | **Works in:** Canvas Apps

---

## Overview

Returns properties of the currently authenticated Microsoft 365 user: FullName, Email, and Image.

---

## Syntax

```plaintext
User()  →  { FullName: text, Email: text, Image: image }
```

---

## Simple Examples

### 1. Show user's name
```plaintext
"Welcome, " & User().FullName
```

### 2. Filter data by current user
```plaintext
Filter(Tasks, AssignedTo = User().Email)
```

### 3. Stamp created-by on new record
```plaintext
Patch(Tickets, Defaults(Tickets), { Title: txtTitle.Text, CreatedBy: User().Email, CreatedOn: Now() })
```

---

## Complex Examples

### 4. Role-based app init on OnStart
```plaintext
Set(varCurrentUser,  User());
Set(varUserProfile,  LookUp(AppUsers, Email = User().Email));
Set(varIsAdmin,      varUserProfile.Role = "Admin");
Set(varCanApprove,   varUserProfile.Role in ["Admin","Manager","Director"]);
If(
    IsBlank(varUserProfile),
    Navigate(AccessDeniedScreen),
    Navigate(HomeScreen)
)
```

### 5. Personalised task filter
```plaintext
Filter(
    Tasks,
    AssignedTo = User().Email &&
    Status <> "Closed" &&
    DueDate >= Today()
)
```

### 6. User profile image in a header
```plaintext
// Image control Image property:
If(IsBlank(varUserProfile.PhotoURL), User().Image, varUserProfile.PhotoURL)
// Label: User().FullName
// Sub-label: User().Email
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
| [`LookUp`](./LookUp.md) | Related function |
| [`Filter`](./Filter.md) | Related function |
| [`Patch`](./Patch.md) | Related function |

---

## 🔗 Official Documentation
[User – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-user)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*