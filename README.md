# Atelier Musea — Assistant de modification du site

## 👋 Bienvenue Leanne !

Ce dossier, c'est le "cerveau" de ton assistant. Grâce à lui, tu peux **modifier ton site toi-même, sans coder** : tu discutes en français avec l'assistant, il s'occupe du reste.

### Comment ça marche, en 4 étapes

1. **Tu expliques** ce que tu veux changer, avec tes mots. Par exemple : *"Je voudrais que le bouton de la page d'accueil soit plus visible."*
2. **L'assistant te pose des questions** pour être sûr de bien comprendre, puis il te montre un petit plan de ce qu'il va faire. Rien ne se lance sans ton accord.
3. **Un second assistant fait le travail** dans le code, tranquillement, sur une copie de travail — jamais directement sur ton site en ligne.
4. **Valentin vérifie et met en ligne.** Chaque modification passe par une "Pull Request" : une proposition de changement que Valentin relit avant de la publier. Ton site ne peut donc jamais casser sans qu'un humain ait vérifié.

### Tes deux commandes magiques

Dans la fenêtre de discussion, tu peux taper des commandes qui commencent par `/` :

| Commande | Quand l'utiliser |
|----------|------------------|
| `/nouvelle-tache` | **À chaque nouvelle demande.** C'est ton point de départ : l'assistant te dit bonjour, te pose ses questions et suit tout le processus, à chaque fois de la même façon. |
| `/finaliser-pr` | **Si une demande a été interrompue** (ordinateur fermé, discussion coupée…). L'assistant reprend là où ça s'est arrêté et termine la proposition de changement pour Valentin. |

💡 Le réflexe à avoir : **une envie de changement = `/nouvelle-tache`**. Tu n'as rien d'autre à retenir.

### Ce qu'il faut retenir

- 💬 Tu parles **en français**, normalement — pas besoin de vocabulaire technique.
- ❓ Tu peux **tout demander**, même si ça te semble bête. L'assistant est là pour ça.
- 🔒 **Rien n'est publié automatiquement.** L'assistant prépare, Valentin valide et met en ligne.

---

---

# Partie technique (pour le développeur et les agents)

## Vue d'ensemble

Harness à deux agents pour laisser une cliente non-technique piloter des modifications front-end sur le site Atelier Musea (`codebase/`, repo `ValentinMi/atelier-musea`) :

- **Orchestrateur** (Phase A) — discute en français avec la cliente, reformule, écrit un plan, le fait valider, puis délègue via l'outil Task. Il ne modifie **jamais** le code.
- **Sous-agent** (Phase B) — travaille dans `codebase/`, exécute le plan, vérifie, ouvre une PR. Il ne merge et ne déploie **jamais**.

## Git flow (Phase B)

```
git checkout main && git pull        # toujours partir d'un main à jour
git checkout -b feat/<nom>           # une tâche = une branche = une PR
# ... implémentation ...
npx tsc --noEmit                     # depuis apps/web
npx biome check --write .            # depuis apps/web
pnpm run dev                         # test visuel sur localhost:3000
git add <fichiers du plan>           # jamais `git add .`
git commit -m "feat(scope): ..."     # Conventional Commits
git push -u origin feat/<nom>
gh pr create                         # la PR s'arrête là — merge + déploiement manuels
```

## Installation

Ce repo ne contient **que le harness**. Le code du site vit dans son propre repo (`ValentinMi/atelier-musea`) et se clone dans `codebase/`, qui est gitignoré :

```bash
git clone git@github.com:<org>/harness-musea.git
cd harness-musea
git clone git@github.com:ValentinMi/atelier-musea.git codebase
cd codebase && pnpm install
```

⚠️ Lancer Claude Code **depuis la racine `harness-musea/`** (pas depuis `codebase/`), sinon les commandes `/nouvelle-tache` et `/finaliser-pr` ne sont pas disponibles.

## Fichiers du harness

| Fichier | Rôle |
|---------|------|
| `AGENTS.md` | Persona et règles de l'Orchestrateur |
| `harness/WORKFLOW.md` | Processus complet Phase A → Phase B |
| `harness/CONTEXT.md` | Projet : où est le code, comment le lancer, comment il est maintenu |
| `harness/STYLE_GUIDE.md` | Règles de style et ton de voix — la palette et les fontes sont **référencées**, la source de vérité est `codebase/apps/web/panda.config.ts` |
| `harness/PLAN_TEMPLATE.md` | Format des plans (rédigés en français simple, lisibles par la cliente) |
| `harness/SUBAGENT_BRIEF.md` | Instructions d'exécution du sous-agent |
| `harness/EXAMPLES.md` | 3 exemples complets de bout en bout + checklist "PR prête" |
| `.claude/commands/` | `/nouvelle-tache` (démarrer une demande) · `/finaliser-pr` (terminer une PR en cours) |

## Sources de vérité

- **Conventions du repo** (commits, branches, commandes, architecture) : `codebase/CLAUDE.md`.
- **Design tokens** (couleurs, fontes, spacing) : `codebase/apps/web/panda.config.ts` — jamais dupliqués dans le harness.
- `codebase/` est un clone indépendant, gitignoré ici : les deux historiques git ne se mélangent jamais.
