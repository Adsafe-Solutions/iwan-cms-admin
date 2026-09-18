---
name: iwan-cms-admin-overview
description: Orientation for iwan-cms-admin — the design system, the one-list-one-form pattern, and where the deep gotchas live. Load when new to the repo or adding a content type/field kind.
---

# iwan-cms-admin overview

`README.md` has the full detail, including nine already-diagnosed gotchas in
`Table.jsx`/`RichText.jsx`/`Repeater.jsx`/`lib/auth.jsx` — re-read that
section before touching any of those four files, rather than rediscovering
the same bug.

## What this is

Vite + React + Tailwind, Vercel-deployed, talking only to `iwan-cms-api`.
Where events, blogs, episodes and promos are written.

## The pattern

Four content types differ only in **fields** — `src/resources.jsx` is the one
file that tells them apart (columns, form sections, labels), matching the
API's `routes/crud.js` on purpose. A fifth type is one entry there, not a new
page. Field kinds render through `src/form/fields.jsx`.

Design tokens live in `tailwind.config.js` only (`rgb(var(--x) / <alpha>)`,
themeable) — deliberately **not** the Iwan brand palette, so the editor tool
and the live site stay visually distinct.

## Countries: empty means EVERYWHERE, not unassigned

`CountryPicker` asks "Everywhere or Specific countries" explicitly rather
than a bare checkbox list, and refuses to let unticking the last country
silently mean "everywhere." A scoped editor cannot select Everywhere at all.

## The blog editor is not a security control

TipTap in `src/ui/RichText.jsx` — the API's sanitise-on-write
(`iwan-cms-api/src/lib/html.js`) is the only real defence; the toolbar is
usability only. Its extension list must stay matched to that allowlist or an
editor's work is silently stripped on save.

## Other skills in this deck

| skill | load it for |
|---|---|
| `environment-and-deploying` | env vars, Vercel deploy, CORS |
| `audience-applications-registrations` | Audience/Applications/Registrations screens — pair with `iwan-cms-api`'s `resend-email-system` skill |
| `apply-forms-and-user-roles` | the volunteer/career form builder, user roles/scoping |
