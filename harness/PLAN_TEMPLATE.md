# Plan Template

Use this template to write every action plan. Fill every section in **simple French** — the client reads this too.

---

## Titre de la tâche

> *[Short label, e.g. : "Changer la couleur du bouton CTA sur la page d'accueil"]*

---

## Besoin initial

> *[What the client said, in their own words. Keep it verbatim.]*

---

## Objectif reformulé

> *[Your plain-French rephrasing of the need. Clear, simple, no jargon.]*

---

## Pages / Fichiers concernés

| Page / Fichier | Rôle |
|----------------|------|
| `path/to/file.css` | Styles du header |
| `src/components/Navbar.jsx` | Composant de navigation |

> *List the files you expect to touch. Best guess is fine — the subagent may adjust if needed.*

---

## Changements attendus

> *[Precise, actionable steps. Be specific enough that the subagent can execute without asking more questions.]*

**Étape 1 :** Trouver le composant du bouton CTA dans `apps/web/src/components/`
**Étape 2 :** Modifier le token de couleur Panda utilisé pour le fond (ex. `accent` → `accent.dark`)
**Étape 3 :** Vérifier que le contraste reste lisible (rapport ≥ 4.5:1)
**Étape 4 :** Tester visuellement sur la page d'accueil

---

## Résultat attendu

> *[What the client should see after the change. In simple French.]*

> *Le bouton "Commander" sur la page d'accueil affiche maintenant un fond bleu vif (#3B82F6) au lieu de noir. Le texte reste blanc. Le hover garde l'effet de soulignement.*

---

## Notes de style

> *[Reference applicable rules from `STYLE_GUIDE.md`. Color, typography, spacing, etc.]*

- Couleurs autorisées : tokens Panda existants (`accent`, `sage`, `beige`, `brown`…) — jamais de valeur hexadécimale en dur
- Fontes : tokens existants (`heading` = Playfair Display, `body` = Manrope)
- Ne pas ajouter d'ombre ou d'animation non demandée (garder le style minimal)

---

## Notes du sous-agent

> *[Any additional context for the subagent — tricky areas, dependencies, gotchas. Leave blank if nothing special.]*

> *Le fichier `styles/buttons.css` surcharge certaines propriétés. Vérifier les deux fichiers après modification.*

---

*Plan généré par Orchestrator · [Date]*
