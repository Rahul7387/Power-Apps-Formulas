# 🧭 Navigate

> **Category:** Navigation | **Complexity:** ⭐⭐ Intermediate | **Works in:** Canvas Apps

---

## 📋 Table of Contents
- [Overview](#overview)
- [Syntax](#syntax)
- [Parameters](#parameters)
- [Transition Types](#transition-types)
- [Simple Examples](#simple-examples)
- [Complex Examples](#complex-examples)
- [Passing Context Variables](#passing-context-variables)
- [Navigate vs Back](#navigate-vs-back)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)
- [Related Functions](#related-functions)

---

## Overview

`Navigate` changes the currently displayed screen in a Canvas App. It optionally applies a visual transition animation and can pass data to the destination screen via context variables — without needing global variables.

```
Screen A                             Screen B
────────────────                     ────────────────
[Button OnSelect]                    Receives context:
Navigate(                  ──────►  {SelectedItem: ...}
  ScreenB,                           {IsEditMode: true}
  ScreenTransition.Cover,
  {SelectedItem: Gallery1.Selected,
   IsEditMode: true}
)
```

---

## Syntax

```plaintext
Navigate( Screen [, Transition [, UpdateContextRecord ]] )
```

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `Screen` | Screen | ✅ Yes | The name of the target screen |
| `Transition` | ScreenTransition Enum | ❌ Optional | Visual animation (default: `None`) |
| `UpdateContextRecord` | Record | ❌ Optional | Context variables to create/update on the target screen |

---

## Transition Types

| Transition | Direction | Best Paired With |
|------------|-----------|-----------------|
| `ScreenTransition.None` | Instant | Tab navigation |
| `ScreenTransition.Cover` | Slides in from right | Forward navigation |
| `ScreenTransition.CoverRight` | Slides in from left | Forward (RTL feel) |
| `ScreenTransition.Fade` | Cross-fade | Neutral transitions |
| `ScreenTransition.UnCover` | Current slides away right | Back / Cancel |
| `ScreenTransition.UnCoverRight` | Current slides away left | Back (RTL feel) |

---

## Simple Examples

### 1. Navigate to a screen
```plaintext
Navigate(HomeScreen)
```

### 2. Navigate with a fade transition
```plaintext
Navigate(DashboardScreen, ScreenTransition.Fade)
```

### 3. Navigate with a Cover (forward) feel
```plaintext
Navigate(DetailScreen, ScreenTransition.Cover)
```

### 4. Navigate on app start (OnStart)
```plaintext
// App OnStart — go straight to login if not authenticated
If(
    IsBlank(varCurrentUser),
    Navigate(LoginScreen, ScreenTransition.None),
    Navigate(HomeScreen, ScreenTransition.None)
)
```

### 5. Navigate from a gallery row tap
```plaintext
// Gallery1 OnSelect
Navigate(DetailScreen, ScreenTransition.Cover)
// Then on DetailScreen: Form1.Item = Gallery1.Selected
```

---

## Complex Examples

### 6. Pass the selected record as a context variable
```plaintext
// Gallery1 OnSelect
Navigate(
    ProductDetailScreen,
    ScreenTransition.Cover,
    { SelectedProduct: Gallery1.Selected }
)

// On ProductDetailScreen:
// Label Text: SelectedProduct.ProductName
// Label Text: Text(SelectedProduct.Price, "$#,##0.00")
// Form1.Item: SelectedProduct
```

### 7. Pass multiple context variables for an edit workflow
```plaintext
// Edit button OnSelect
Navigate(
    EditFormScreen,
    ScreenTransition.Cover,
    {
        RecordToEdit:  Gallery1.Selected,
        FormIsNewMode: false,
        OriginScreen:  "ProjectList"
    }
)

// On EditFormScreen — use context variables:
// EditForm1.Item = RecordToEdit
// Back button label: "← Back to " & OriginScreen
```

### 8. Role-based navigation
```plaintext
// Login button OnSelect — route user to the right screen
Switch(
    LookUp(UserRoles, Email = User().Email).Role,
    "Admin",     Navigate(AdminDashboard,   ScreenTransition.Fade),
    "Manager",   Navigate(ManagerScreen,    ScreenTransition.Fade),
    "Technician",Navigate(TechnicianScreen, ScreenTransition.Fade),
               Navigate(HomeScreen,       ScreenTransition.Fade)   // default
)
```

### 9. Wizard / multi-step form navigation
```plaintext
// Step 1 "Next" button
If(
    IsBlank(txtName.Text) || IsBlank(txtEmail.Text),
    Notify("Complete all fields before continuing.", NotificationType.Warning),
    Navigate(
        WizardStep2Screen,
        ScreenTransition.Cover,
        {
            wizStep1Data: {
                Name:  txtName.Text,
                Email: txtEmail.Text,
                Phone: txtPhone.Text
            }
        }
    )
)

// Step 2 "Next" button — merge data from step 1 + step 2
Navigate(
    WizardStep3Screen,
    ScreenTransition.Cover,
    {
        wizStep1Data: wizStep1Data,
        wizStep2Data: {
            Company:  txtCompany.Text,
            JobTitle: txtJobTitle.Text
        }
    }
)

// Final step "Submit"
Patch(
    Leads,
    Defaults(Leads),
    {
        Name:    wizStep1Data.Name,
        Email:   wizStep1Data.Email,
        Company: wizStep2Data.Company
    }
);
Navigate(ConfirmationScreen, ScreenTransition.Fade)
```

### 10. Conditional back-navigation after form submit
```plaintext
// Form OnSuccess — go back to whichever screen the user came from
Navigate(
    If(OriginScreen = "Dashboard", DashboardScreen, ProjectListScreen),
    ScreenTransition.UnCover
)
```

### 11. Navigate with a loading state
```plaintext
// Show a loading overlay, load data, then navigate
UpdateContext({ isLoading: true });
ClearCollect(varProjectData, Filter(Projects, OwnerEmail = User().Email));
UpdateContext({ isLoading: false });
Navigate(ProjectScreen, ScreenTransition.Fade, { projects: varProjectData })
```

---

## Passing Context Variables

Context variables passed via `Navigate` are **screen-scoped** — they only exist on the destination screen and are reset when you navigate away.

```
// Passing from Screen A:
Navigate(ScreenB, ScreenTransition.Cover, { customerID: 42, isVIP: true })

// Using on Screen B (no Set/UpdateContext needed — they just exist):
LookUp(Customers, ID = customerID)
If(isVIP, "⭐ VIP Customer", "Standard")
```

> To persist data across multiple screens, use `Set()` (global variable) instead.

---

## Navigate vs Back

| Scenario | Use |
|----------|-----|
| Go to a specific screen | `Navigate()` |
| Return to previous screen | `Back()` |
| Cancel / discard form | `Back()` |
| Passing data forward | `Navigate()` with `UpdateContextRecord` |
| After form submit → confirmation | `Navigate()` |

---

## Best Practices

1. **Use `None` for tab navigation** — transitions on tabs feel sluggish.
2. **Pair `Cover` (forward) with `UnCover` (back)** for a natural mobile feel.
3. **Use context variables instead of global vars** for screen-to-screen data when the data is only needed on the next screen.
4. **Never call `Navigate` inside calculated properties** — only in behavior properties (`OnSelect`, `OnSuccess`, etc.).
5. **Avoid deep stacks** — more than 4–5 back-navigations confuses users; consider resetting with a direct `Navigate`.
6. **Put navigation in `OnSuccess`, not `OnSelect`** — so the write finishes before the screen changes.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| `Navigate` in a `Text` property | Won't run — not a behavior property | Move to `OnSelect` |
| Navigating before `SubmitForm` completes | Screen changes, save may fail silently | Put `Navigate` in `Form.OnSuccess` |
| Using global vars when context vars suffice | Unnecessary state pollution | Pass data in `UpdateContextRecord` |
| Forgetting `ScreenTransition.UnCover` on back | Feels like going forward | Match cover/uncover for natural feel |

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Back`](./Back.md) | Return to the previous screen |
| [`Launch`](./Launch.md) | Open a URL or external app |
| [`UpdateContext`](./UpdateContext.md) | Update local variables on a screen |
| [`Set`](./Set.md) | Global alternative to context variables |

---

## 🔗 Official Documentation
[Navigate and Back functions – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-navigate)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*
