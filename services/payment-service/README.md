# payment-service (C# / .NET)

## Rôle
Exécute les paiements/indemnisations une fois un sinistre validé (auto ou manuellement).
Jamais exposé directement au frontend — appelé uniquement par claims-service
(voir `docs/uml/04-diagramme-deploiement.md`, principe de moindre privilège).

## Statut
À implémenter en Phase 2.
