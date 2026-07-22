# Style Guide

## Purpose

This guide ensures every change stays **consistent** with the existing design. Read it before touching any CSS or visual element.

---

## Color Palette

**Single source of truth: `codebase/apps/web/panda.config.ts`** (summarized in `codebase/CLAUDE.md`). The palette is **not duplicated here** — always read it from the codebase so it can never go stale.

Token names to know: `accent` (+ `accent.dark`, `accent.light`) for brand/CTA, `sage`, `beige`, `lightBeige` (page background), `brown` (main text), `muted`, plus `status.*` tokens for workshop badges.

**Rule:** Always use existing Panda CSS tokens (via `styled-system/css`). Never hard-code hex values. Do not add a new color unless the plan explicitly requests it — and then add it as a token in `panda.config.ts`, not inline.

---

## Typography

Font tokens are defined in `codebase/apps/web/panda.config.ts` (source of truth): `heading` (Playfair Display), `body` (Manrope), `logo` (Cormorant). Always use the tokens, never a `font-family` in hard.

> ⚠️ **`html { font-size: 62.5% }`** est appliqué globalement : **`1rem` = 10px** dans ce projet. Un texte courant fait `1.6rem` (16px), pas `1rem`. Toujours raisonner en base 10.

Font sizes are written inline in `css()` calls using rem (base 10px). Match the sizes already used by neighboring components rather than inventing new ones.

---

## Spacing

Vertical rhythm between page sections uses the Panda spacing tokens `section.sm` (4rem), `section.md` (6rem), `section.lg` (8rem). For everything else, reuse the spacing values already present in the component you're editing (rem, base 10px).

---

## Component Rules

- **Reuse before creating.** Look at existing components in `apps/web/src/components/` and copy their patterns (padding, radius, hover states) instead of inventing new ones.
- Buttons: bold, states hover / focus / disabled always present.
- Images: always an `alt` text describing the image; rounded corners on cards.

---

## Tone of Voice

In all client-facing text (labels, messages, notifications):

- **Friendly and warm.** Use "tu" not "vous".
- **Clear and direct.** No jargon. One idea per sentence.
- **Encouraging.** The client is not technical — celebrate their choices.
- **Examples:**
  - ✅ *"Le bouton est maintenant bleu, comme tu le voulais !"*
  - ❌ *"Background updated per the client's request."*
  - ❌ *"La modification du sélecteur CSS a été appliquée."*

---

## What Not to Do

- Do not introduce new fonts unless the plan says so.
- Do not change the color palette without explicit client request + confirmation.
- Do not add animations without checking the style.
- Do not strip existing classes or refactor unrelated code.
- Do not add external dependencies without the developer's (Valentin) approval.