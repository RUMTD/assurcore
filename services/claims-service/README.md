# claims-service (Java / Spring Boot)

## Rôle
Orchestrateur du cycle de vie d'un sinistre : déclaration, appel au risk-scoring-service, ancrage
blockchain, déclenchement conditionnel du paiement. Voir le scénario complet dans
`docs/uml/03-diagramme-sequence-declaration-sinistre.md`.

## Statut
À implémenter en Phase 2 (TDD) — service prioritaire car il traverse le plus de composants.
