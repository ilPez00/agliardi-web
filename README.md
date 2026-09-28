# agliardi-web

Temporary static website for **Agliardi S.n.c. di Agliardi Franco & C.** —
Autotrasporti e Scavi (Carisolo, TN — Val Rendena, Trentino-Alto Adente).

Hosted provisionally on the path `praxisweb.xyz/agliardi`; intended to become
its own site later (e.g. `agliardiweb.xyz`).

## Source data (all public)

| Field        | Value                                                                 |
|--------------|-----------------------------------------------------------------------|
| Ragione soc. | Agliardi S.n.c. di Agliardi Franco & C.                                |
| Sede         | Via Presanella, 1 — 38080 Carisolo (TN)                                |
| Tel.         | 0465 501085 *(verified PagineGialle "numero verificato")*             |
| P.IVA        | 02143840227 *(from PagineGialle registry payload)*                     |
| Email        | agliardisnc.scavi@gmail.com *(Instagram bio)*                          |
| IG           | [@agliardi_autotrasporti_e_scavi](https://instagram.com/agliardi_autotrasporti_e_scavi) |
| Servizi      | Scavi e movimento terra, demolizioni, autotrasporti conto terzi, manutenzione strade, sgombero neve, frantumazione pietre |

Photos: 10 screenshots from the company Instagram feed (Carisolo, Madonna di
Campiglio, San Lorenzo Dorsino). Kept as-is (with IG UI) per request; crop or
replace with clean photos when available.

Map: OpenStreetMap embed, marker at Via Presanella 1, Carisolo
(lat 46.16716, lon 10.76068 — Nominatim verified 2026-09-28).

## Reference design system

Visual system mirrors `praxis_webapp` (client/AGENTS.md, docs/IMPLEMENTATION_SUMMARY.md):
- tokens: `--spacing-{xs,sm,md,lg,xl}` 4/8/16/24/32px, `--radius-{sm,md,lg,xl}` 8/12/16/24px
- font: iOS-inspired stack (`SF Pro Display`, Segoe UI, system sans)
- cards: `--bg-primary` panel, `--border-light`, `--shadow-sm`→`--shadow-md` on hover

`--praxis-primary` is mapped to an **earth brown** (`#8B5A2B`) instead of the
iOS blue `#007AFF` — blue is inappropriate for an earthworks brand. All other
tokens reused verbatim.

## Local preview

```bash
npx serve .      # or any static file server
open http://localhost:3000/agliardi/
```

> Note the trailing slash: an earthworks site is fine on `/agliardi/`; bare
> `/agliardi` (no slash) is handled by the Vercel rewrite rule (see deploy).

## Deploy — temporary path on praxisweb.xyz

This repo becomes a standalone Vercel project, then praxis_webapp mounts it
under `/agliardi/` via a rewrite. Two manual steps:

1. **Create the agliardi project** (do once):
   `vercel` (or Vercel Dashboard → New Project → import this repo) on the
   `main` branch. Project name: `agliardi-web`. Default output `.` (static).
   → You get an origin like `https://agliardi-web.vercel.app`.

2. **Add rewrite in praxis_webapp** (`client/vercel.json`):

   ```jsonc
   "rewrites": [
     { "source": "/agliardi/:path*", "destination": "https://agliardi-web.vercel.app/:path*" },
     { "source": "/api/:path*",        "destination": "https://web-production-646a4.up.railway.app/api/:path*" },
     { "source": "/(.*)",              "destination": "/index.html" }
   ]
   ```
   Order matters: the `/agliardi` rewrite must precede the SPA catch-all.

Push to `main` here ⇒ auto-deploy (new content).
Push to praxis `main` ⇒ new rewrite rule active.

## TODO

- [ ] Replace IG screenshots with clean photos when provided.
- [ ] Swap the temporary address/P.IVA block with owner-verified exact data.
- [ ] Add `robots.txt` + favicon.
- [ ] When promoted to a real site: custom domain `agliardiweb.xyz`,
      Italian + (optional) a short English version, a simple contact form
      (email + phone only — no third-party tracking), dark mode respect.

## License

Content is a public-profile snapshot of a third-party business, compiled only
for temporary hosting. Not for commercial reuse.
