# Two-Agent Workflow

This document describes the end-to-end process for making a change to the project.

---

## Phase A — Orchestrator (with the client)

Phase A is fully in **French**. The client speaks French only and knows HTML/CSS only.

### Step 1 — Comprendre le besoin

Welcome the client in French. Ask friendly, open questions to understand:

- **Quoi ?** — Que veux-tu changer exactement ?
- **Pourquoi ?** — Qu'est-ce qui ne va pas actuellement, ou quel est le but ?
- **Où ?** — Sur quelle page ou quel élément ?
- **À quoi ça devrait ressembler ?** — Description ou exemple concret.

**Example (in French):**
> *"Salut ! Pour bien démarrer, dis-moi : tu veux changer quoi exactement, et sur quelle page du site ?"*

If the request is vague, ask follow-up questions. **Never guess.** It's better to ask 3 questions now than to deliver the wrong thing.

### Step 2 — Reformuler et confirmer

Rephrase the client's request in simple French. Show it back to them and ask:
> *"C'est bien ça que tu veux ? Je vais reformuler pour être sûr : …"*

Wait for a clear **yes** before continuing.

### Step 3 — Écrire le plan

Using `PLAN_TEMPLATE.md`, write a detailed action plan. Include:

- **Titre de la tâche** — short label
- **Besoin initial** — client's words, verbatim
- **Objectif reformulé** — your plain-French rephrasing
- **Pages/fichiers concernés** — best guess at the files to touch
- **Changements attendus** — precise, actionable steps
- **Résultat attendu** — what the client should see after
- **Notes de style** — reference `STYLE_GUIDE.md` for colors, fonts, tone

### Step 4 — Montrer le plan et obtenir confirmation

Show the full plan to the client in French:
> *"Voilà mon plan. Je vais demander à mon assistant de le mettre en place. Ça te va ?"*

Get explicit confirmation. Do not proceed without it.

### Step 5 — Déléguer au sous-agent

The Orchestrator calls the **Task tool** (Agent) directly — a normal tool call in the conversation, no UI panel or shortcut.

**Tool call:**
- `description`: short label of the task
- `prompt`:

```
Work inside the git repository at codebase/ (project root — all pnpm and git
commands run from there). First read harness/SUBAGENT_BRIEF.md and
codebase/CLAUDE.md, then execute the plan below.

=== PLAN START ===
[Full plan text from Step 3]
=== PLAN END ===
```

**After spawning**, tell the client in French:
> *"Parfait, c'est parti ! Je lance un assistant qui va travailler dans le projet. Je te redis quand c'est prêt pour la review 👍"*

---

## Phase B — Sous-agent (inside `codebase/`)

The subagent works **inside `codebase/`** (the git repository — all commands below run from there), follows the plan, and never touches `main`.

### Step 6 — Lire le plan

Read `harness/SUBAGENT_BRIEF.md`, `codebase/CLAUDE.md`, and the plan passed by the orchestrator.

### Step 7 — Créer une branche

```bash
git checkout main && git pull
git checkout -b feat/<short-description>
```
Example: `git checkout -b feat/hero-button-color`

**Rule:** One task = one branch = one PR. Never work on `main` or `master`. Branch naming follows the repo convention (`codebase/CLAUDE.md`): `feat/<name>`, or `fix/<name>` for a bug fix.

### Step 8 — Implémenter les changements

Follow the plan exactly. Use `STYLE_GUIDE.md` for design consistency. If the plan is ambiguous, ask the orchestrator (not the client) — the orchestrator will re-explain.

Key rules:
- Reuse existing CSS variables and design tokens.
- Keep the existing code style and patterns.
- Do not refactor unrelated files.
- Do not add new dependencies without checking with the orchestrator first.

### Step 9 — Vérifier

Run the quality checks first (from `codebase/apps/web`):

```bash
npx tsc --noEmit            # Type check — must pass
npx biome check --write .   # Lint + format — must pass
```

Then test visually. Follow the commands in `CONTEXT.md`:

```bash
# From codebase/ root
pnpm install    # only if node_modules is missing
pnpm run dev
```

Open `http://localhost:3000`. Verify:
- The change looks correct visually.
- No errors in the browser console or the dev server output.
- The page loads without crash.

If something is broken, fix it before committing.

### Step 10 — Commit, push, ouvrir la Pull Request

```bash
git add <only the files from the plan>   # never `git add .`
git commit -m "feat(scope): short description"
git push -u origin feat/<short-description>
```

Commit format: Conventional Commits with scope, e.g. `feat(boutique): change CTA color` (see `codebase/CLAUDE.md`).

**PR title format:** `[TASK_TITLE] — [Client's original need]`

**PR body format:**
```
## Ce qui était demandé
[Client's original request in French]

## Changements effectués
- [Change 1]
- [Change 2]

## Résultat
[Plain-language description of what the developer should see]
```

### Step 11 — Reporter au orchestrateur

When done, send a message to the orchestrator:
> *"La branche [branch-name] est prête. La PR est ouverte ici : [URL]. Tout est bon pour la review 👍"*

---

## Phase A — Orchestrator (report back to client)

Tell the client in French:
> *"C'est prêt ! Le code est en ligne, tu peux venir review la Pull Request. Voici le lien : [URL] 👍"*

---

## Summary

| Phase | Who | Where | Language |
|-------|-----|-------|----------|
| A (understand, plan, delegate) | Orchestrator | Repo root (never edits code) | French |
| B (implement, test, PR) | Subagent | `codebase/` | English (code) |
| A (report) | Orchestrator | Outside project | French |