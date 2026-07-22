# Worked Examples

This file shows 3 complete end-to-end examples of the two-agent workflow, adapted to the real Atelier Musea stack (TanStack Start + Panda CSS + Strapi). Each example goes from a French client request → Orchestrator questions → filled plan → subagent execution → PR result.

---

## Exemple 1 — Couleur du bouton CTA

### La demande (client, en français)

> *"Je voudrais que le bouton 'Découvrir la boutique' sur la page d'accueil ressorte plus, là il se fond dans le fond"*

### Étape 1 — Questions du Orchestrateur

> *"Salut Leanne ! Oui on peut arranger ça. Juste pour être sûr : tu veux qu'il soit plus foncé (le marron foncé de ta palette), ou carrément une autre couleur ? Et c'est bien le gros bouton en haut de la page d'accueil, pas ceux de la boutique ?"*

### Réponse client

> *"Oui celui en haut de l'accueil. Le marron foncé de ma palette ce serait parfait !"*

### Étape 2 — Plan rempli

```
Titre :  Renforcer le bouton CTA de la page d'accueil
Besoin initial :  Je voudrais que le bouton 'Découvrir la boutique' sur la page d'accueil ressorte plus, là il se fond dans le fond
Objectif reformulé :  Le bouton principal en haut de la page d'accueil passe à la couleur accent foncée de la palette pour mieux contraster avec le fond beige clair. Aucun autre bouton n'est touché.
Pages / Fichiers concernés :  apps/web/src/components/home/ (section hero de la page d'accueil)
Changements attendus :
  Étape 1 : Trouver le composant hero de la page d'accueil dans apps/web/src/components/home/
  Étape 2 : Changer le fond du bouton du token accent vers accent.dark (token Panda existant — ne pas hard-coder #6e5c44)
  Étape 3 : Vérifier que le texte reste lisible (contraste ≥ 4.5:1) et que le hover reste visible
  Étape 4 : Tester visuellement sur http://localhost:3000
Résultat attendu :  Le bouton "Découvrir la boutique" en haut de la page d'accueil est nettement plus foncé et ressort du fond beige. Les autres boutons du site restent inchangés.
Notes de style :  Utiliser les tokens Panda (accent.dark), jamais de valeur hexadécimale en dur. Garder le hover-state actuel.
```

### Étape 3 — Sous-agent (Phase B)

```bash
cd codebase
git checkout main && git pull
git checkout -b feat/homepage-cta-contrast
# → Trouve le composant hero, change le token accent → accent.dark dans le css()
# → npx tsc --noEmit + npx biome check --write . (depuis apps/web)
# → pnpm run dev, vérifie http://localhost:3000
# → git add apps/web/src/components/home/Hero.tsx
# → Commit: "feat(home): darken hero CTA for contrast"
# → Push + PR ouverte via gh pr create
```

### Résultat (PR)

```
Titre :  Page d'accueil — Bouton "Découvrir la boutique" plus contrasté

## Ce qui était demandé
Je voudrais que le bouton 'Découvrir la boutique' sur la page d'accueil ressorte plus.

## Changements effectués
- Composant hero de la page d'accueil : fond du bouton passé du token `accent` au token `accent.dark`
- Aucune autre page ou bouton modifié

## Résultat
Le bouton principal de la page d'accueil est maintenant marron foncé (#6e5c44) et ressort nettement du fond beige clair. Texte et hover inchangés.
```

---

## Exemple 2 — Texte sur la fiche produit

### La demande (client, en français)

> *"Bonjour, sur mes fiches produit je veux changer le texte 'Ajouter au panier' en 'Commander' c'est plus direct je trouve"*

### Étape 1 — Questions du Orchestrateur

> *"Salut ! Bonne idée, 'Commander' c'est plus direct. Tu veux ça sur toutes les fiches produit de la boutique ? Et est-ce qu'on garde le même bouton dans le panier, ou lui aussi change ?"*

### Réponse client

> *"Toutes les fiches produit oui. Le panier on ne touche pas."*

### Étape 2 — Plan rempli

```
Titre :  Renommer le bouton "Ajouter au panier" en "Commander"
Besoin initial :  Sur mes fiches produit je veux changer le texte 'Ajouter au panier' en 'Commander'
Objectif reformulé :  Le bouton principal de chaque fiche produit (/boutique/<produit>) affiche "Commander" au lieu de "Ajouter au panier". La page panier n'est pas modifiée.
Pages / Fichiers concernés :  apps/web/src/routes/boutique/$slug.tsx (fiche produit) · apps/web/src/components/ (composant bouton panier si partagé)
Changements attendus :
  Étape 1 : Trouver le bouton "Ajouter au panier" (fiche produit ou composant partagé dans components/)
  Étape 2 : Remplacer le texte par "Commander" uniquement là où il est utilisé sur les fiches produit
  Étape 3 : Vérifier que la page /panier n'est pas affectée
  Étape 4 : Tester sur une fiche produit et sur la page panier
Résultat attendu :  Toutes les fiches produit affichent "Commander". La page panier est inchangée.
Notes de style :  Respecter la casse actuelle du bouton (majuscules ou non). Ton de voix : direct et chaleureux.
```

### Résultat (PR)

```
Titre :  Fiche produit — "Ajouter au panier" → "Commander"

## Ce qui était demandé
Je veux changer le texte 'Ajouter au panier' en 'Commander' sur les fiches produit.

## Changements effectués
- Texte du bouton des fiches produit → "Commander"
- Page panier vérifiée : inchangée

## Résultat
Toutes les fiches produit affichent "Commander". Cohérent sur toute la boutique, panier intact.
```

---

## Exemple 3 — Bannière en page d'accueil

### La demande (client, en français)

> *"Je veux mettre à jour ma bannière d'accueil avec une nouvelle photo de mes créations et un nouveau texte. La photo est sur mon bureau."*

### Étape 1 — Questions du Orchestrateur

> *"Salut ! Pas de souci pour la bannière. J'ai besoin de quelques précisions :
> 1. Tu peux me transférer la photo ici ? Je la placerai au bon endroit dans le projet.
> 2. Quel est le nouveau texte que tu veux afficher sur la bannière ?
> 3. On garde la même mise en page (texte + image) ou tu veux changer quelque chose ?"*

### Réponse client

> *"Voici la photo en pièce jointe. Le texte : 'Créations en jesmonite, façonnées à la main dans mon atelier'. On garde la même mise en page."*

### Étape 2 — Plan rempli

```
Titre :  Nouvelle bannière sur la page d'accueil
Besoin initial :  Je veux mettre à jour ma bannière d'accueil avec une nouvelle photo et un nouveau texte
Objectif reformulé :  La bannière en haut de la page d'accueil affiche la nouvelle photo fournie par Leanne et le texte "Créations en jesmonite, façonnées à la main dans mon atelier". La mise en page reste inchangée.
Pages / Fichiers concernés :  apps/web/src/components/home/ (section hero) · apps/web/public/ (image à remplacer — vérifier le chemin exact utilisé par le composant)
Changements attendus :
  Étape 1 : Repérer l'image actuelle de la bannière (chemin importé dans le composant hero) et la remplacer par la nouvelle photo, même nom de fichier si possible
  Étape 2 : Mettre à jour le texte de la bannière avec le nouveau texte
  Étape 3 : Mettre à jour l'attribut alt de l'image pour décrire la nouvelle photo
  Étape 4 : Tester en local sur http://localhost:3000
Résultat attendu :  La bannière affiche la nouvelle photo et le nouveau texte. Le reste de la page est inchangé.
Notes de style :  Même fonte (Playfair Display via le token heading) et même taille que l'ancien texte. Alt text descriptif obligatoire.
```

### Résultat (PR)

```
Titre :  Page d'accueil — Nouvelle bannière (photo + texte)

## Ce qui était demandé
Je veux mettre à jour ma bannière d'accueil avec une nouvelle photo et un nouveau texte.

## Changements effectués
- Image de bannière remplacée par la nouvelle photo fournie
- Texte de bannière → "Créations en jesmonite, façonnées à la main dans mon atelier"
- Alt text de l'image mis à jour

## Résultat
La bannière de la page d'accueil affiche la nouvelle photo et le nouveau texte. Mise en page inchangée.
```

---

## Checklist avant de déclarer "PR prête"

- [ ] `npx tsc --noEmit` passe sans erreur (depuis `apps/web`)
- [ ] `npx biome check --write .` passe sans erreur (depuis `apps/web`)
- [ ] Le changement a été testé en local (navigateur + console, pas d'erreur)
- [ ] Le commit suit le format Conventional Commits `feat(scope):` ou `fix(scope):`
- [ ] La branche suit le format `feat/<nom>` ou `fix/<nom>`
- [ ] Seuls les fichiers du plan ont été stagés (pas de `git add .`)
- [ ] La PR est ouverte et son corps décrit : demandé / fait / résultat
- [ ] Le Orchestrateur a reçu le lien de la PR
