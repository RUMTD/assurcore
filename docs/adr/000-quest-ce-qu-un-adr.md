# ADR-000 — Qu'est-ce qu'un ADR et pourquoi on en écrit ?

Un **Architecture Decision Record (ADR)** est un court document qui capture **une** décision
d'architecture importante, avec son contexte, les options envisagées, et les conséquences assumées.

## Pourquoi c'est une compétence RNCP à part entière

Un jury de certification (ou un vrai employeur) ne juge pas seulement le résultat technique, mais
la **capacité à justifier des choix**. Un ADR répond exactement à la question qu'on te posera en
soutenance : *"Pourquoi as-tu choisi X plutôt que Y ?"*. C'est aussi une pratique réelle d'entreprise
(popularisée par Michael Nygard), utilisée dans la plupart des équipes tech matures.

## Format standard utilisé pour chaque ADR de ce projet

```
# ADR-XXX — Titre de la décision

## Statut
Accepté / Rejeté / Remplacé par ADR-YYY

## Contexte
Quel problème on essaie de résoudre ? Quelles contraintes ?

## Options envisagées
Liste des alternatives sérieusement considérées, avec avantages/inconvénients.

## Décision
Ce qu'on choisit, en une phrase claire.

## Conséquences
Ce que ça implique (positif ET négatif) — un ADR honnête assume aussi les inconvénients du choix fait.
```

## Index des ADR du projet

| ADR | Titre | Bloc RNCP concerné |
|---|---|---|
| [001](./001-architecture-microservices.md) | Architecture microservices vs monolithe | Bloc 1 |
| [002](./002-choix-langages-par-service.md) | Choix d'un langage différent par service | Bloc 1 |
| [003](./003-base-de-donnees.md) | PostgreSQL comme base transactionnelle | Bloc 1 / Bloc 4 |
| [004](./004-infra-as-code.md) | Terraform + Ansible pour l'infrastructure | Bloc 3 |
| [005](./005-blockchain-audit-sinistres.md) | Traçabilité des sinistres par chaîne de hash | Bloc 3 |
| [006](./006-choix-angular-frontend.md) | Angular comme framework frontend | Bloc 1 / Bloc 2 |
