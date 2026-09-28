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

## Deploy — temporary path on praxisweb.xyz (how it actually works)

**praxisweb.xyz is NOT Vercel/Railway/hosted.** It is served by the
`praxis-webapp` backend (`pansa`, Cloudflare tunnel `praxis-edge`) which runs
`scripts/auto_update.sh` every 5 minutes: fast-forward `main` → `npm ci` →
`npm run build` (vite copies `client/public/*` → `client/dist/`) → restart
`praxis-backend.service`. Express serves `client/dist/` via `express.static`
before the SPA `/index.html` fallback.

So `/agliardi/` is live simply by having the files on disk. Two places matter:

- **Durable (survives rebuilds):** `client/public/agliardi/` committed in
  `ilPez00/praxis_webapp` — vite copies it to `client/dist/agliardi/` on every
  `npm run build`.
- **Immediate (served right now, in the live `client/dist`):** the same files
  were copied into `client/dist/agliardi/` on the host, so the site answered
  before the next 5-minute build tick.

There is **no external Vercel Pages** and **no `vercel.json` rewrite**. The
`agliardi-web` repo (this one) is the source of record / future home; the
canonical deploy repo is `ilPez00/praxis_webapp` (`main`).

### To redeploy / update content

1. Edit files here (root `index.html`, `img/`).
2. Commit + push to `ilPez00/agliardi-web` (source of truth), **and** mirror into
   the deploy repo:
   ```bash
   cp index.html img /home/gio/praxis_webapp/client/public/agliardi/
   cd /home/gio/praxis_webapp
   git add client/public/agliardi && git commit -m "chore: update Agliardi site" && git push origin main
   ```
3. Either wait for the next `praxis-autoupdate.timer` tick (≤5 min), or trigger
   `npm run build` in `client/` + restart `praxis-backend.service` on `pansa`.

> ⚠️ If `praxis_webapp` has uncommitted changes (e.g. `docs/DEV_PLAN.md` is
> dirty), `auto_update.sh` **refuses** to deploy — commit/stash them first.

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
