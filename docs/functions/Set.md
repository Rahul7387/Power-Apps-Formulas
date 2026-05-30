# 📦 Set

> **Category:** Variables | **Complexity:** ⭐ Beginner | **Works in:** Canvas Apps

---

## Overview

`Set` creates or updates a **global variable** — a named value accessible from **every screen** in the app. Unlike context variables, global variables persist as long as the app session is running.

```
Set(varUserRole, "Admin")
          │
          └── Accessible on HomeScreen, DetailScreen, SettingsScreen... everywhere
```

---

## Syntax

```plaintext
Set( VariableName, Value )
```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `VariableName` | Identifier | ✅ Yes | Name of the variable (convention: `var` prefix) |
| `Value` | Any | ✅ Yes | The value to assign — text, number, boolean, record, table |

---

## Simple Examples

### 1. Set a text variable
```plaintext
Set(varUserRole, "Admin")
```

### 2. Set a boolean flag
```plaintext
Set(varIsLoggedIn, true)
```

### 3. Set a number and increment it
```plaintext
Set(varCounter, varCounter + 1)
```

### 4. Toggle a boolean
```plaintext
Set(varShowSidebar, !varShowSidebar)
```

### 5. Store the current user record
```plaintext
// App OnStart
Set(varCurrentUser, User())
```

---

## Complex Examples

### 6. Initialise multiple globals on app start
```plaintext
// App OnStart
Set(varCurrentUser,  User());
Set(varAppVersion,   "2.1.4");
Set(varIsAdmin,      LookUp(UserRoles, Email = User().Email).IsAdmin);
Set(varEnvironment,  If(varIsAdmin, "Production", "Standard"));
ClearCollect(varNotifications, Filter(Notifications, RecipientEmail = User().Email && IsRead = false))
```

### 7. Store a selected record globally for multi-screen use
```plaintext
// Gallery OnSelect — store selection before navigating
Set(varSelectedProject, Gallery_Projects.Selected);
Navigate(ProjectDetailScreen, ScreenTransition.Cover)

// On ProjectDetailScreen — use varSelectedProject everywhere
// Label: varSelectedProject.Title
// Form.Item: varSelectedProject
// Other label: "Owner: " & varSelectedProject.OwnerName
```

### 8. Track wizard step state globally
```plaintext
// "Next" button on each wizard step
Set(varWizardStep, varWizardStep + 1);
Navigate(
    Switch(
        varWizardStep,
        2, WizardStep2,
        3, WizardStep3,
        4, WizardReviewScreen
    ),
    ScreenTransition.Cover
)
```

### 9. Cart pattern — add item and update total
```plaintext
// "Add to Cart" button
Collect(varCart, { 
    ProductID: Gallery_Products.Selected.ID,
    Name:      Gallery_Products.Selected.Name,
    Price:     Gallery_Products.Selected.Price,
    Qty:       1
});
Set(varCartTotal, Sum(varCart, Price * Qty));
Set(varCartCount, CountRows(varCart));
Notify(Gallery_Products.Selected.Name & " added to cart!", NotificationType.Success)
```

### 10. Role-based feature flag
```plaintext
// App OnStart — set feature access flags
Set(varCurrentUser,   LookUp(AppUsers, Email = User().Email));
Set(varCanApprove,    varCurrentUser.Role in ["Manager", "Director", "Admin"]);
Set(varCanDelete,     varCurrentUser.Role = "Admin");
Set(varCanExport,     varCurrentUser.Role in ["Manager", "Director", "Admin", "Analyst"])

// Control visibility with the flags:
// Delete button Visible: varCanDelete
// Approve button Visible: varCanApprove
```

---

## Set vs UpdateContext

| Feature | `Set` | `UpdateContext` |
|---------|-------|-----------------|
| Scope | **Global** — all screens | **Local** — current screen only |
| Syntax | `Set(varName, value)` | `UpdateContext({varName: value})` |
| Multi-var update | One call per variable | Multiple in one call |
| Best for | User info, auth, shared flags | Modal state, loading flags, step counters |

---

## Best Practices

1. **Prefix all variables with `var`** — `varIsAdmin`, `varSelectedOrder` — to distinguish from data source columns.
2. **Initialise in `App.OnStart`** — set all global defaults so screens never encounter undefined variables.
3. **Use `UpdateContext` for screen-local state** — not every variable needs to be global.
4. **Avoid setting the same variable in many places** — centralise in `OnStart` or a dedicated init button.
5. **Never `Set` inside a data card formula** — only in behavior properties.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Using `Set` for every variable | Performance overhead; pollutes global scope | Use `UpdateContext` for screen-specific state |
| Not initialising in `OnStart` | Variable is `false`/`blank` on first use | Always initialise in `App.OnStart` |
| Calling `Set` in a calculated property | Behavior function — ignored | Move to `OnSelect` or `OnVisible` |

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`UpdateContext`](./UpdateContext.md) | Screen-scoped alternative |
| [`ClearCollect`](./ClearCollect.md) | Set a collection (table) variable |
| [`Collect`](./Collect.md) | Append to a collection variable |

---

## 🔗 Official Documentation
[Set function – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-set)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*