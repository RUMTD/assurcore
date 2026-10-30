# ADR-006 — Angular comme framework frontend

## Statut
Accepté (remplace le choix initial de React du cadrage de la Phase 0)

## Contexte

Le cadrage initial prévoyait React. L'objectif réel du projet est de **monter en compétence sur
Angular**, framework vu et attendu dans de nombreux environnements d'entreprise (finance, assurance).
Le frontend d'AssurCore est une application de gestion (formulaires, tableaux, rôles, workflows),
cas d'usage pour lequel Angular est très adapté.

## Options envisagées

1. **React + TypeScript**
   - ✅ Écosystème immense, courbe d'entrée douce
   - ❌ C'est une bibliothèque : routing, formulaires, état et structure sont à assembler soi-même
   - ❌ Ne répond pas à l'objectif de montée en compétence visé

2. **Angular + TypeScript**
   - ✅ Framework complet et opinioné (routing, formulaires réactifs, HttpClient, injection de
     dépendances, tests intégrés) : une seule façon standard de structurer l'application
   - ✅ TypeScript natif, cohérent avec le typage fort côté back (Java, C#)
   - ✅ L'injection de dépendances ressemble à Spring (côté Java) : concepts transférables
   - ✅ Très présent dans les grands comptes (banque, assurance) : valeur pour le profil visé
   - ❌ Courbe d'apprentissage plus raide (RxJS, modules/composants standalone, décorateurs)

3. **Vue.js**
   - ✅ Simple et progressif
   - ❌ Moins présent dans le secteur cible, et ne sert pas l'objectif Angular

## Décision

Le frontend est développé en **Angular + TypeScript** (composants standalone, formulaires réactifs,
`HttpClient` avec intercepteur JWT, guards de routes par rôle, tests avec Jasmine/Karma ou Jest).

## Conséquences

- **Positif** : la structure imposée par Angular facilite la lecture, les tests et le CI/CD (Phase 3 :
  `ng build`, `ng test` dans le pipeline).
- **Positif** : les intercepteurs HTTP et guards d'Angular répondent naturellement à l'authentification
  par JWT de `auth-service` (voir diagramme de séquence).
- **Négatif assumé** : démarrage plus lent qu'avec React ; il faudra apprendre RxJS (flux
  asynchrones) dès les premiers appels API.
- **Impact sur les autres documents** : le pod `frontend` du diagramme de déploiement devient une
  image Angular compilée (fichiers statiques) servie par Nginx.
