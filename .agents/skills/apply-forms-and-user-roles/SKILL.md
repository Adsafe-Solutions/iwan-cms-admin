---
name: apply-forms-and-user-roles
description: The volunteer/career form builder and the Users/roles screen — the live/default deletion restriction and how "Everywhere" is decided. Load for either screen.
---

# Apply forms and user roles

## Apply forms

One editable form per kind (`volunteer`, `career`), built from
`src/form/FormBuilder.jsx` — the same field-kind system as content types.
⚠ Neither the live form nor the default form can be deleted, and the delete
button is not even rendered for either
(`canWrite && !row.active && !row.isDefault`) — this mirrors a real API-side
refusal (`iwan-cms-api`'s `applyForms.js`/`applyDefaults.js`), not just a UI
nicety. "Make live" switches which form is currently active per kind.

## User roles

⚠ "Everywhere" in the country column is **not** just an empty `countries`
array — it is also true for every `admin` regardless of what `countries`
holds:

```js
row.role === "admin" || row.countries.length === 0
```

Do not simplify this to just the length check; an admin's `countries` field
is not meaningful and role already overrides it. Deactivating an account
(`PATCH /api/admin/users/:id`, `{ active: false }`) is a live effect on the
API side too — auth middleware re-reads `active` on every request, so it
signs someone out immediately, not on next token expiry.
