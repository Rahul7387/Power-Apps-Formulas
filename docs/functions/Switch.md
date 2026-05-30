# 🔀 Switch

> **Category:** Logic | **Complexity:** ⭐ Beginner | **Works in:** Canvas Apps

---

## Overview

`Switch` compares one value against multiple exact matches and returns the result of the first match. It is cleaner and more readable than deeply nested `If` when you are checking the same variable against several fixed values.

---

## Syntax

```plaintext
Switch( Formula, Match1, Result1 [, Match2, Result2, ...] [, DefaultResult] )
```

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `Formula` | Any | ✅ Yes | The value to compare |
| `MatchN` | Any | ✅ Yes | A candidate value to compare against |
| `ResultN` | Any | ✅ Yes | Returned when Formula = MatchN |
| `DefaultResult` | Any | ❌ Optional | Returned when no match found |

---

## Simple Examples

### 1. Status colour badge
```plaintext
Switch(
    ThisItem.Status,
    "Active",   Color.Green,
    "Pending",  Color.Orange,
    "Inactive", Color.Gray,
    "Rejected", Color.Red,
    Color.Black
)
```

### 2. Priority label
```plaintext
Switch(
    ThisItem.Priority,
    1, "🔴 Critical",
    2, "🟠 High",
    3, "🟡 Medium",
    4, "🟢 Low",
    "Unknown"
)
```

### 3. Navigate by role
```plaintext
Switch(
    varUserRole,
    "Admin",      Navigate(AdminDashboard,    ScreenTransition.Fade),
    "Manager",    Navigate(ManagerDashboard,  ScreenTransition.Fade),
    "Technician", Navigate(TechnicianScreen,  ScreenTransition.Fade),
                  Navigate(HomeScreen,        ScreenTransition.Fade)
)
```

### 4. Day-of-week name
```plaintext
Switch(
    Weekday(Today()),
    1, "Sunday",   2, "Monday",  3, "Tuesday",
    4, "Wednesday",5, "Thursday",6, "Friday",
    "Saturday"
)
```

### 5. Icon name by document type
```plaintext
Switch(
    ThisItem.FileType,
    "pdf",  Icon.Document,
    "xlsx", Icon.DetailList,
    "docx", Icon.Note,
    Icon.Attachment
)
```

---

## Complex Examples

### 6. Wizard step content visibility
```plaintext
// Step 1 panel Visible:
Switch(varWizardStep, 1, true, false)

// Step 2 panel Visible:
Switch(varWizardStep, 2, true, false)

// Step 3 panel Visible:
Switch(varWizardStep, 3, true, false)
```

### 7. Multi-action switch on a context menu selection
```plaintext
Switch(
    varContextMenuAction,
    "Edit",     EditForm(Form1); Navigate(EditScreen, ScreenTransition.Cover),
    "Delete",   UpdateContext({ showDeleteConfirm: true }),
    "Duplicate",Patch(Projects, Defaults(Projects), {
                    Title: Gallery1.Selected.Title & " (Copy)",
                    OwnerEmail: User().Email
                }),
    "Share",    Navigate(ShareScreen, ScreenTransition.Cover, { ShareRecord: Gallery1.Selected })
)
```

### 8. Compute shipping cost by zone
```plaintext
// Label Text
"Shipping: " & Text(
    Switch(
        ddlShippingZone.Selected.Value,
        "Zone A", 5.00,
        "Zone B", 10.00,
        "Zone C", 18.50,
        "International", 35.00,
        0.00
    ),
    "$#,##0.00"
)
```

### 9. Build a dynamic screen title
```plaintext
Switch(
    varActiveSection,
    "Dashboard",  "📊 Dashboard",
    "Projects",   "📁 My Projects",
    "Tasks",      "✅ My Tasks",
    "Settings",   "⚙️ Settings",
    "Power Apps"
)
```

### 10. Map approval status codes to labels and colours
```plaintext
// Status label text
Switch(ThisItem.ApprovalCode, 0,"Draft", 1,"Submitted", 2,"In Review", 3,"Approved", 4,"Rejected","Unknown")

// Status label colour
Switch(ThisItem.ApprovalCode, 0,Color.Gray, 1,Color.Blue, 2,Color.Orange, 3,Color.Green, 4,Color.Red, Color.Black)
```

---

## Switch vs If

| Use `Switch` when | Use `If` when |
|-------------------|---------------|
| Comparing one value against 4+ fixed options | Conditions are range checks (`> 100`) |
| All branches test the same formula | Different fields/expressions per branch |
| You want cleaner, readable code | Complex boolean logic (`&&`, `||`) |

---

## Best Practices

1. **Always include a default** (last argument without a match) to handle unexpected values.
2. **Use `Switch` over nested `If`** when you have 3+ exact-value checks on one variable.
3. **Combine with icon/colour properties** for consistent status indicators across the app.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`If`](./If.md) | General conditional logic |
| [`Coalesce`](./Coalesce.md) | Return first non-blank value |

---

## 🔗 Official Documentation
[If and Switch – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-if)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*