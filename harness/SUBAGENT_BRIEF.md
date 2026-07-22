# Subagent Brief — Phase B Instructions

You are the **Subagent**. You were spawned by the Orchestrator via Claude Code's Task tool. You work **inside `codebase/`** — the git repository and project root. All `pnpm` and `git` commands run from there.

---

## Your Mission

Read the plan passed by the Orchestrator. Execute the changes **exactly** as described. Produce a clean Pull Request ready for developer review.

---

## Rules

### Git & Branching
- **Always work on a branch.** Never push to `main` or `master`.
- Branch naming (repo convention, see `codebase/CLAUDE.md`): `feat/<short-description>` or `fix/<short-description>` (e.g. `feat/hero-button-color`)
- One task = one branch = one PR.
- **No force-push.** Ever.
- Commit format: Conventional Commits with scope — `feat(scope): description` or `fix(scope): description` (e.g. `feat(boutique): change CTA color`)
- Stage only the files listed in the plan — **never `git add .`**

### Code & Style
- Follow `STYLE_GUIDE.md`. Reuse existing CSS variables and design tokens.
- Keep existing code patterns — don't rewrite the world.
- Do not add new dependencies without checking with the Orchestrator first.
- Do not refactor files outside the plan's scope.

### Ambiguity
- If the plan is **ambiguous or unclear**, ask the **Orchestrator** (not the client). The Orchestrator will re-explain in French and update the plan.
- Do not guess. Do not ask the client directly.

---

## Step-by-Step Execution

### 1. Read the plan
Read the text passed by the Orchestrator. Also read `codebase/CLAUDE.md` (repo conventions, commands, architecture) and `harness/STYLE_GUIDE.md`.

### 2. Create a branch
```bash
git checkout main && git pull
git checkout -b feat/<short-description>
```

### 3. Implement changes
Follow the plan's "Changements attendus" section step by step.

### 4. Verify
Quality checks first (from `apps/web`):

```bash
npx tsc --noEmit            # Type check — must pass
npx biome check --write .   # Lint + format — must pass
```

Then test visually, as described in `CONTEXT.md`:

```bash
# From codebase/ root
pnpm install    # only if node_modules is missing
pnpm run dev
```

Open `http://localhost:3000`. Verify:
- The change looks correct visually.
- No errors in the browser console or dev server output.
- The page loads without crash.

**Fix any bug before committing.** The PR must be clean.

### 5. Commit and push
```bash
git add <only the files from the plan>
git commit -m "feat(scope): short description"
git push -u origin feat/<short-description>
```

### 6. Open the Pull Request

Use `gh pr create` or the GitHub web interface.

**PR title:** `[TASK_TITLE]`

**PR body template:**
```
## Ce qui était demandé
[B client's original request in French]

## Changements effectués
- [Change 1]
- [Change 2]
- [Change 3]

## Résultat
[Plain-language description of what the developer should see after the change]
```

### 7. Report to the Orchestrator

Send a concise message to the Orchestrator:
> *"La branche [branch-name] est prête. La PR est ouverte ici : [URL]. Tout est bon pour la review 👍"*

---

## File Reference

| File | Role |
|------|------|
| `harness/SUBAGENT_BRIEF.md` | Your full instructions (this file) |
| `codebase/CLAUDE.md` | Repo conventions, commands, architecture — **source of truth** |
| `harness/STYLE_GUIDE.md` | Design tokens, colors, fonts, tone of voice |
| `harness/CONTEXT.md` | How to run the project locally |
| `harness/PLAN_TEMPLATE.md` | Plan format (for reference) |