# Guide de contribution

## Cycle de travail (GitHub Flow)

1. Partir d'un `main` à jour : `git switch main && git pull`
2. Créer une branche : `git switch -c feat/mon-sujet`
3. Faire de petits commits (Conventional Commits : `feat:`, `fix:`, `docs:`, `test:`, `chore:`)
4. Envoyer la branche : `git push -u origin feat/mon-sujet`
5. Ouvrir une Pull Request vers `main` sur GitHub, se relire, puis **Squash and merge**
6. Nettoyer : `git switch main && git pull && git branch -d feat/mon-sujet`

## Règles
- Jamais de commit direct sur `main`.
- Une branche = un sujet. Une PR = un changement relisible.
- En TDD, un commit `test:` (test qui échoue) précède le commit `feat:` qui le fait passer.

Voir `docs/adr/007-strategie-branches-git.md`.
