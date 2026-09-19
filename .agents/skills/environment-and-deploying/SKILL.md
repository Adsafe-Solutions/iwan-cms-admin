---
name: environment-and-deploying
description: Every VITE_* variable this app actually reads, verified against source, plus the Vercel deploy and the CORS coordination with iwan-cms-api. Load for env setup, deploying, or a CORS error.
---

# Environment and deploying

## ⚠ `VITE_ENV_LABEL` is documented, not implemented

`README.md`'s variable table describes a dev/staging badge; nothing in `src/`
reads this variable (verified by grepping every `import.meta.env.[A-Z_]+`
reference). Setting it today does nothing — do not assume the safety
property it describes is real.

## Variables actually read

| var | read in | default if unset |
|---|---|---|
| `VITE_API_URL` | `lib/api.js` | `http://localhost:4000` — **silent** failure mode in a real deployment, not an error |
| `VITE_SITE_URL` | `layout/Shell.jsx` | `https://iwan.community` |
| `VITE_MAX_UPLOAD_MB` | `form/ImageField.jsx` | must match `iwan-cms-api`'s own limit for whichever host (10MB Render, 4MB Vercel) |

All `VITE_*` inline at **build** time — changing one always means a
rebuild/redeploy, and none may ever hold a secret (the built bundle is
public).

```bash
cp .env.example .env.local
```

## Deploying

Vercel, preset **Vite**, build `npm run build`, output `dist`.
`vercel.json` carries the SPA rewrite (without it, deep links 404 on a hard
refresh) and `X-Robots-Tag: noindex`.

## ⚠ CORS is a two-repo change

Whatever origin Vercel assigns this deployment has to be added to
`iwan-cms-api`'s `CORS_ORIGINS` — see that repo's `environment-and-secrets`
skill — or every request is refused by the browser before it reaches the
API's own logic. No automatic link between the two; a new preview
deployment's origin has to be added there by hand.
