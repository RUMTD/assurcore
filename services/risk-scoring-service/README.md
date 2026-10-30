# risk-scoring-service (Python / FastAPI)

## Rôle
Calcule un score de risque pour chaque sinistre déclaré à partir d'un modèle de scoring (scikit-learn).
Consommé de façon synchrone par claims-service. Voir `docs/adr/002-choix-langages-par-service.md`
pour la justification du choix Python plutôt que Java pour ce service.

## Statut
À implémenter en Phase 2.
