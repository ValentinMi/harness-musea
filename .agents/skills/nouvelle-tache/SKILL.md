---
name: nouvelle-tache
description: Démarrer une nouvelle demande de modification du site avec la cliente (Phase A puis Phase B)
---

Démarre une nouvelle tâche avec le client.

1. Dis **"Bonjour !"** en français, de manière chaleureuse.
2. Demande au client ce qu'il ou elle veut changer. Pose des questions ouvertes :
    - *"Tu veux changer quoi exactement ?"*
    - *"Sur quelle page du site ?"*
    - *"Pourquoi — qu'est-ce qui ne va pas actuellement ?"*
    - *"À quoi ça devrait ressembler après ?"*
3. Si la demande est vague, pose des questions de suivi. **Ne devine jamais.**
4. Une fois le besoin clair, **reformule-le en français simple** et montre-le au client.
5. Demande confirmation : *"C'est bien ça que tu veux ?"*
6. Écris le plan complet en utilisant le modèle dans `harness/PLAN_TEMPLATE.md`.
7. Montre le plan au client et demande validation.
8. Une fois validé, **passe en Phase B (exécution)** : lis `harness/EXECUTION_BRIEF.md` et `codebase/CLAUDE.md`, puis exécute le plan **dans le dépôt git `codebase/`** (toutes les commandes `pnpm` et `git` se lancent depuis là). Dis d'abord au client en français :
   > *"Parfait, c'est parti ! Je m'occupe de la modification dans le projet. Je te redis quand c'est prêt pour la review 👍"*
9. Quand la PR est ouverte, dis au client en français :
   > *"C'est prêt ! Tu peux venir voir la Pull Request ici : [URL]. Elle est prête pour la review 👍"*

**Règles :** En Phase A tu ne touches jamais au code — tu comprends, tu planifies, tu fais valider. En Phase B tu suis le plan à la lettre, sur une branche, et tu t'arrêtes à la PR : jamais de merge, jamais de déploiement.
