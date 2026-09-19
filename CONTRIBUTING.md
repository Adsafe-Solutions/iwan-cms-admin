# Contributing to iwan-cms-admin

Start here if you are new to this repo. `README.md` is the technical
reference for how the admin actually works (the design system, the
one-list/one-form pattern, the blog editor, the gotchas already fixed once);
this one is about getting a working local setup and knowing which document to
open next.

## The three repos this system is made of

| repo | what it is | where |
|---|---|---|
| `iwan-cms-api` | the Express + Mongoose API this app talks to, and only this app | sibling directory |
| **`iwan-cms-admin`** | **this one** — the React admin UI | you are here |
| `new-iwan` | the public marketing site, its Cloudflare Worker, and the email templates | separate checkout |

This app talks to exactly one thing: `iwan-cms-api`'s `/api/admin` routes.
Adding a field here that the API does not accept yet — or the reverse — is
the most common way a "the CMS is broken" report turns out to be a
two-repo change, not a one-repo bug.

## Before you start

- **Node 22+** (check `iwan-cms-api`'s own `engines: 24.x` too if running
  both) — **Vite will not even start on Node 18**, with an error
  (`crypto.hash is not a function`) that does not mention the Node version at
  all. Check `node --version` first if `npm run dev` fails immediately.
- The API running somewhere this app can reach — see below.

## First-time setup

```bash
git clone <this repo>
cd iwan-cms-admin
npm install
cp .env.example .env.local     # points VITE_API_URL at the API
```

Get the API running too — the quickest path needs no database of your own:

```bash
cd ../iwan-cms-api && npm run dev:memory   # :4000, throwaway data, prints a login
```

Then, back here:

```bash
npm run dev     # :5174
```

Sign in with whatever `dev:memory` printed in its own banner.

## Where the documentation actually lives

| doc | for |
|---|---|
| `README.md` | the technical reference — design system, the resource pattern, the blog editor's real constraints, every gotcha already diagnosed once |
| `.env.example` | the three real environment variables this app reads, verified against source |
| `.agents/skills/*/SKILL.md` (mirrored at `.claude/skills/`) | focused deep dives, one per area — load the one matching what you are actually doing rather than reading everything |

**The skill deck:**

| skill | load it when |
|---|---|
| `iwan-cms-admin-overview` | new here, or the resource/field-kind pattern for content types |
| `environment-and-deploying` | env vars, Vercel deploy, or a CORS error against the API |
| `audience-applications-registrations` | the Audience, Applications, or Registrations screens — these are the admin half of `iwan-cms-api`'s Resend/email system, so its `resend-email-system` skill is the other half of this picture |
| `apply-forms-and-user-roles` | the volunteer/career form builder, or user roles and country scoping |

## Before opening a pull request

- `npm run build` succeeds — there is no test suite here, the build itself is
  the check (per `README.md`'s own framing of it).
- `npm run format:check` clean.
- If a field or screen changed here, does the matching API route in
  `iwan-cms-api` still agree with it? Check the relevant skill in that repo,
  not just this one.
- If you touched `Table.jsx`, `RichText.jsx`, `Repeater.jsx`, or
  `lib/auth.jsx` — re-read `README.md`'s "Gotchas that have already bitten"
  section first. Each entry there is a real bug that has already happened
  once in exactly that file.
