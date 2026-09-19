---
name: audience-applications-registrations
description: The three admin screens surfacing iwan-cms-api's audience/Resend, application, and registration systems. Pair with that repo's resend-email-system skill.
---

# Audience, Applications, Registrations

## Audience

Filters (source/subscription/country/text) live in the URL. ⚠ The
Subscribed/Unsubscribe toggle in `AudienceDetail` is the CMS's **only** write
path to that flag — the dialog says so in its own copy, matching the API's
`Audience` model rule (an unticked box on a public form is never an
unsubscribe). ⚠ Deleting a person here removes them from Resend entirely
(`forgetContact()`) but explicitly **keeps** their `Registration`/
`Application` documents — the confirm dialog's own text says so. CSV export
is an authenticated `fetch` with a manual Bearer header + blob download, not
a plain link — a bare `<a href>` sends no Authorization header.

## Applications

One screen for both `volunteer`/`career`, sharing columns so the table
doesn't reshape on filter. Status moves `new → reviewing → accepted →
declined` via `PATCH /api/admin/applications/:id`. ⚠ `answerCell()`
distinguishes "NA" (this kind's form never asked) from a dash (asked, left
blank) — do not collapse these into one blank state.

## Registrations

Same status-button pattern (`new → confirmed → waitlist → cancelled`). Two
things unique to this screen: a per-event capacity display
(`current.taken`/`current.spots`; `taken` counts only `new`/`confirmed`, the
same rule that makes the public site show an event as full — cancelling
someone frees their place), and a **resend confirmation** button
(`POST /api/admin/registrations/:id/resend`) calling the exact
`sendRegistrationConfirmation()` covered in `iwan-cms-api`'s
`resend-email-system` skill. `confirmationSentAt`/`confirmationSentCount` are
shown per row so an editor can tell whether a resend is actually needed.
