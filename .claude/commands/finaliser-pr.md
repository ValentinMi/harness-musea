Finalise la Pull Request en cours.

1. Vérifie dans `codebase/` qu'une branche de travail existe et a été pushée (ex. `feat/...`).
2. Vérifie que la PR est ouverte sur GitHub (`gh pr view` depuis `codebase/`). Si elle n'existe pas encore :
    - Lance un sous-agent via l'outil **Task (Agent)** pour compléter les étapes restantes :
        - Vérifier : `npx tsc --noEmit` et `npx biome check --write .` (depuis `apps/web`), puis test en local (voir `harness/CONTEXT.md`).
        - Commiter au format Conventional Commits : `feat(scope): description courte`.
        - Pusher : `git push -u origin feat/<short-description>`.
        - Ouvrir la PR avec `gh pr create`.
3. Récupère le **lien de la PR**.
4. Dis au client en français :
   > *"C'est prêt ! Tu peux venir voir la Pull Request ici : [URL]. Elle est prête pour la review 👍"*
5. Fournis le lien de la PR au client.

**Règle :** Ne jamais fusionner (merge) ni déployer. Le développeur se charge du merge et du déploiement manuellement sur le VPS.