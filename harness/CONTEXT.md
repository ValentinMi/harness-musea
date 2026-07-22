# Project Context

## Client

**Leanne Humez** — cliente non-technique (HTML/CSS uniquement), propriétaire de l'atelier.

## What is this project

> **Atelier Musea — Site vitrine et e-commerce pour un atelier de créations en jesmonite (vente de produits, commandes sur mesure, ateliers physiques)**

This is an e-commerce website. Built with **TanStack Start (React 19, Vite 7, SSR via Nitro) + Panda CSS, avec un CMS Strapi v5 (PostgreSQL) et Stripe pour les paiements**.

---

## Where the code lives

The project is a **separate git repository cloned into `codebase/`** (remote: `github.com/leannehmz/atelier-musea`). All `pnpm` and `git` commands run from there. The harness repo (this one) gitignores `codebase/` entirely: the two git histories never mix, and harness files are never committed to the project repo. If `codebase/` is missing, clone it first (see the root `README.md`).

The repo's own `codebase/CLAUDE.md` is the **source of truth** for conventions (commits, branches, architecture) — read it before any Phase B work.

---

## How to run it locally

**Everything runs in Docker** — the client's machine (Windows) only needs **Docker Desktop**, no Node/pnpm/PostgreSQL installed. The agent manages the stack as a black box; the client only opens URLs in their browser. Use the `/lancer-projet` skill (or its commands directly):

```bash
# From codebase/ — starts PostgreSQL + Strapi (CMS) + frontend, with live reload
docker compose -f docker-compose.dev.yml --profile full up -d

# Wait until `cms` and `web` are healthy:
docker compose -f docker-compose.dev.yml --profile full ps
```

| URL | What |
|-----|------|
| `http://localhost:3000` | Le site (frontend) |
| `http://localhost:1337/admin` | Le CMS Strapi (contenus, produits, ateliers) |

The source in `codebase/` is bind-mounted into the containers: edits on the host show up live on `localhost:3000` (HMR). First start is slow (install + Strapi build); later starts are fast (dependencies cached in Docker volumes). Never run `down -v` — it would wipe the local database.

**Quality checks (run before every commit, inside the running `web` container, from `codebase/`):**

```bash
docker compose -f docker-compose.dev.yml exec -w /app/apps/web web npx tsc --noEmit
docker compose -f docker-compose.dev.yml exec -w /app/apps/web web npx biome check --write .
```

> Sur une machine de dev avec Node/pnpm installés (celle de Valentin), le mode "hôte" reste possible : `pnpm install`, `pnpm run db:dev`, `pnpm run dev`, `pnpm run dev:cms` — voir `codebase/CLAUDE.md`.

---

## Technology

| Layer | Technology |
|-------|------------|
| Frontend | TanStack Start (React 19, Vite 7, SSR via Nitro) |
| Styling | Panda CSS (zero-runtime, PostCSS) |
| Backend / CMS | Strapi v5 (TypeScript, PostgreSQL) |
| Paiements | Stripe Checkout |
| Hosting | VPS — le développeur déploie manuellement |

---

## Simplified project map

This is a **best-effort guidance map** — not exhaustive. Explore the directory to confirm the exact layout.

| Folder / Pattern | What's inside |
|-----------------|---------------|
| `apps/web/src/` | Code source du frontend |
| `apps/web/panda.config.ts` | Design tokens (couleurs, polices) — voir `STYLE_GUIDE.md` |
| `apps/web/src/routes/` | Pages (routing file-based, ne jamais éditer `routeTree.gen.ts`) |
| `apps/web/src/components/` | Composants UI partagés + `components/checkout`, `components/form` |
| `apps/web/src/lib/` | Client Strapi + fetchers (cart, checkout, products…) |
| `cms/src/api/` | Contenu géré par Strapi (produits, ateliers, commandes…) |

---

## How the project is maintained

- **Developer** reviews every Pull Request before merging.
- **Developer** deploys manually to a VPS after merge.
- **Agents stop at the PR.** They never merge and never deploy.

> *Note: if a request involves backend changes, secrets, database, or infrastructure — those areas are out of scope for a typical front-end change. Tell the client the developer must handle it.*