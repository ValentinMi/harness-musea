---
name: finaliser-pr
description: Reprendre une tâche interrompue et terminer la Pull Request en cours
---

Finalise la Pull Request en cours.

1. Vérifie dans `codebase/` qu'une branche de travail existe et a été pushée (ex. `feat/...`).
2. Vérifie que la PR est ouverte sur GitHub (`gh pr view` depuis `codebase/`). Si elle n'existe pas encore, complète toi-même les étapes restantes (voir `harness/EXECUTION_BRIEF.md`) :
    - Vérifier (stack Docker démarrée — voir `/lancer-projet` ou `harness/CONTEXT.md`) :
      `docker compose -f docker-compose.dev.yml exec -w /app/apps/web web npx tsc --noEmit`
      et `docker compose -f docker-compose.dev.yml exec -w /app/apps/web web npx biome check --write .`,
      puis test visuel sur `http://localhost:3000`.
    - Commiter au format Conventional Commits : `feat(scope): description courte`.
    - Pusher : `git push -u origin feat/<short-description>`.
    - Ouvrir la PR avec `gh pr create`.
3. Récupère le **lien de la PR**.
4. Dis au client en français :
   > *"C'est prêt ! Tu peux venir voir la Pull Request ici : [URL]. Elle est prête pour la review 👍"*
5. Fournis le lien de la PR au client.

**Règle :** Ne jamais fusionner (merge) ni déployer. Le développeur se charge du merge et du déploiement manuellement sur le VPS.
