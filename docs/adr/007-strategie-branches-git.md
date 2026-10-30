# ADR-007 — Stratégie de branches : GitHub Flow

## Statut
Accepté

## Contexte
Projet développé seul, avec une CI/CD à venir (Phase 3). Il faut une stratégie simple, qui garde
`main` toujours stable et laisse une trace claire de chaque changement.

## Options envisagées
1. **Commits directement sur `main`**
   - ✅ Zéro friction
   - ❌ Aucun filet : un commit cassé arrive directement en « production », pas de relecture, pas de CI
2. **GitHub Flow** (`main` stable + une branche courte par changement + Pull Request)
   - ✅ Simple, adapté au déploiement continu, une PR = une unité de revue et de test
   - ✅ Habitude identique à celle des équipes en entreprise
   - ❌ Un peu de cérémonie même en solo
3. **Git Flow** (`main`, `develop`, `release/*`, `hotfix/*`)
   - ✅ Adapté aux versions livrées par lots planifiés
   - ❌ Trop lourd pour un seul développeur et pour du déploiement continu

## Décision
**GitHub Flow.** `main` est toujours stable et déployable. Tout changement passe par une branche
dédiée, une Pull Request, puis un merge. `main` est protégée : pas de push direct.

Nommage des branches : `<type>/<sujet-court>` avec les types de Conventional Commits
(`feat/`, `fix/`, `docs/`, `test/`, `chore/`), par exemple `feat/claims-declarer-sinistre`.

## Conséquences
- **Positif** : la CI de la Phase 3 se branchera naturellement sur les Pull Requests.
- **Positif** : l'historique de `main` raconte le projet, une PR après l'autre.
- **Négatif assumé** : un temps de cérémonie en plus pour de petites modifications, accepté comme
  entraînement au fonctionnement réel d'une équipe.
