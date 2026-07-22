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

```bash
# From codebase/
# 1. Install dependencies (only once, or after pulling updates)
pnpm install

# 2. Start the development server
pnpm run dev

# 3. Open in browser
http://localhost:3000
```

**Quality checks (run before every commit, from `codebase/apps/web`):**

```bash
npx tsc --noEmit            # Type check
npx biome check --write .   # Lint + format
```

> Le CMS Strapi (contenu, produits, ateliers) tourne séparément via `pnpm run dev:cms` → `http://localhost:1337/admin`, avec la base de données lancée par `pnpm run db:dev`.

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