---
name: lancer-projet
description: Démarrer le site et le CMS en local (Docker) pour la cliente — tout est automatique, elle n'a qu'à ouvrir son navigateur
---

Lance le projet en local pour le client, **entièrement via Docker** : le client n'a rien à installer ni à taper (à part avoir Docker Desktop ouvert). Tu gères tout en boîte noire — ne montre jamais de commandes au client, parle-lui uniquement en français simple.

## Étapes

1. Dis au client en français que tu démarres le site, et que la **première fois ça peut prendre plusieurs minutes** (le temps de tout préparer).

2. **Vérifie que Docker tourne** : `docker info` (n'importe où). Si ça échoue, dis au client :
   > *"Il faut d'abord ouvrir l'application **Docker Desktop** (la baleine 🐳). Dis-moi quand c'est fait et je relance !"*

   Puis réessaie quand le client confirme.

3. **Vérifie que `codebase/` existe.** S'il manque, clone-le d'abord (voir le `README.md` racine), puis continue.

4. **Démarre la stack** depuis `codebase/` :
   ```bash
   docker compose -f docker-compose.dev.yml --profile full up -d
   ```

5. **Attends que tout soit prêt** : surveille `docker compose -f docker-compose.dev.yml --profile full ps` jusqu'à ce que les services `cms` et `web` soient `healthy`. Pendant l'attente, tu peux suivre `docker compose -f docker-compose.dev.yml --profile full logs -f` pour détecter un problème.

6. **En cas d'erreur** (service qui ne démarre pas, port déjà pris, etc.) : lis les logs, diagnostique et corrige toi-même. Ne montre jamais les logs bruts au client — explique simplement : *"Petit souci technique, je m'en occupe…"*. Si tu ne peux vraiment pas corriger seul (ex. problème d'installation Docker), explique au client qu'il faut prévenir Valentin.

7. Quand tout est `healthy`, dis au client :
   > *"C'est prêt ! 🎉*
   > *— Ton **site** : http://localhost:3000*
   > *— Ton **espace d'administration** (contenus, produits, ateliers) : http://localhost:1337/admin*
   >
   > *Tu n'as rien d'autre à faire : ouvre simplement ces liens dans ton navigateur. Quand je modifie le code, la page se met à jour toute seule."*

## À savoir

- **Tout tourne dans Docker** : PostgreSQL, le CMS Strapi et le frontend. Aucun besoin de Node, pnpm ou d'une base de données installés sur la machine.
- Le code source est monté dans les conteneurs : les modifications faites dans `codebase/` sont visibles en direct sur `localhost:3000` (rechargement automatique). Si un changement ne s'affiche pas, redémarre le service concerné : `docker compose -f docker-compose.dev.yml restart web` (ou `cms`).
- Pour **arrêter** le projet (si le client le demande) : `docker compose -f docker-compose.dev.yml --profile full down`. Les données et les dépendances sont conservées (volumes) — le prochain démarrage sera rapide.
- Ne supprime **jamais** les volumes (`down -v`) sans l'accord explicite de Valentin : ça effacerait la base de données locale.
