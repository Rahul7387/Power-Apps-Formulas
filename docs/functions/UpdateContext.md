# 🖼️ UpdateContext

> **Category:** Variables | **Complexity:** ⭐ Beginner | **Works in:** Canvas Apps

---

## Overview

`UpdateContext` creates or updates **context variables** scoped to the **current screen only**. They are ideal for UI state like modal visibility, loading spinners, step counters, and temporary edit values — anything that shouldn't bleed into other screens.

---

## Syntax

```plaintext
UpdateContext( { VariableName: Value [, VariableName2: Value2, ...] } )
```

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| Record | Record | ✅ Yes | One or more name:value pairs to set/update |

---

## Simple Examples

### 1. Show a modal dialog
```plaintext
// "Delete" button
UpdateContext({ showDeleteModal: true })
// Modal Visible property: showDeleteModal
```

### 2. Hide modal on cancel
```plaintext
// "Cancel" button inside modal
UpdateContext({ showDeleteModal: false })
```

### 3. Set a loading flag
```plaintext
UpdateContext({ isLoading: true })
```

### 4. Update multiple variables at once
```plaintext
UpdateContext({ isLoading: true, errorMsg: "", currentTab: "Details" })
```

### 5. Increment a step counter
```plaintext
UpdateContext({ wizardStep: wizardStep + 1 })
```

---

## Complex Examples

### 6. Full modal confirm-delete pattern
```plaintext
// "Delete" icon button
UpdateContext({ showConfirm: true, recordToDelete: ThisItem })

// Confirm modal "Yes" button
Remove(Tasks, recordToDelete);
UpdateContext({ showConfirm: false, recordToDelete: Blank() });
Notify("Task deleted.", NotificationType.Success)

// Confirm modal "No" button
UpdateContext({ showConfirm: false, recordToDelete: Blank() })

// Modal container Visible property: showConfirm
// Modal message label: "Delete "" & recordToDelete.Title & ""?"
```

### 7. Multi-step wizard state
```plaintext
// Screen OnVisible — initialise
UpdateContext({ wizStep: 1, wizData: { Name: "", Email: "", Dept: "" } })

// "Next" button on step 1
If(
    IsBlank(txtName.Text) || IsBlank(txtEmail.Text),
    Notify("Complete all fields.", NotificationType.Warning),
    UpdateContext({
        wizStep: 2,
        wizData: { Name: txtName.Text, Email: txtEmail.Text, Dept: wizData.Dept }
    })
)

// Progress bar Width: (wizStep / 3) * Parent.Width
// Step indicator Visible: wizStep = 2
```

### 8. Inline edit pattern (edit row without leaving the screen)
```plaintext
// Edit icon in gallery row
UpdateContext({ editingRowID: ThisItem.ID, editTitle: ThisItem.Title, editStatus: ThisItem.Status })

// Save button (visible only when editingRowID matches)
Patch(Tasks, LookUp(Tasks, ID = editingRowID), { Title: editTitle, Status: editStatus });
UpdateContext({ editingRowID: -1 });
Notify("Saved!", NotificationType.Success)

// Row edit controls Visible: ThisItem.ID = editingRowID
// Row view labels Visible: ThisItem.ID <> editingRowID
```

### 9. Tab navigation state
```plaintext
// Tab button 1 OnSelect
UpdateContext({ activeTab: "Overview" })
// Tab button 2 OnSelect
UpdateContext({ activeTab: "Timeline" })
// Tab button 3 OnSelect
UpdateContext({ activeTab: "Files" })

// Tab underline indicator Width: If(activeTab = "Overview", tabOverview.Width, 0)
// Content panel Visible: activeTab = "Overview"
```

### 10. Optimistic UI update while Patch runs
```plaintext
// "Approve" button
UpdateContext({ isApproving: true });
Patch(Requests, Gallery_Requests.Selected, { Status: "Approved", ApprovedBy: User().Email });
UpdateContext({ isApproving: false });
Notify("Request approved!", NotificationType.Success)

// Button DisplayMode: If(isApproving, DisplayMode.Disabled, DisplayMode.Edit)
// Spinner Visible: isApproving
```

---

## Set vs UpdateContext

| | `Set` | `UpdateContext` |
|-|-------|-----------------|
| Scope | Global (all screens) | Local (current screen) |
| Multi-var | One per call | Multiple in one call |
| Best for | Shared state, user info | UI state, modals, steps |

---

## Best Practices

1. **Use for all UI state** — modals, loading flags, tab selection, inline edit IDs.
2. **Initialise in `Screen.OnVisible`** — so variables reset when the user navigates back.
3. **Update multiple vars in one call** — `UpdateContext({ a: 1, b: 2 })` is cleaner than two separate calls.
4. **Never use context vars across screens** — pass data via `Navigate` context record or `Set` instead.

---

## Related Functions

| Function | Relationship |
|----------|-------------|
| [`Set`](./Set.md) | Global alternative |
| [`Navigate`](./Navigate.md) | Pass context vars to another screen |

---

## 🔗 Official Documentation
[UpdateContext – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-updatecontext)

---

*← [Back to Home](../../README.md) | Power Apps Formulas Reference | Last updated: May 2026*