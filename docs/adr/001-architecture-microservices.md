# ADR-001 — Architecture microservices vs monolithe

## Statut
Accepté

## Contexte

AssurCore doit gérer des domaines métier assez distincts (authentification, contrats, sinistres,
scoring de risque, paiements) avec des contraintes très différentes : le scoring de risque nécessite
un écosystème data/ML (Python), le paiement nécessite une isolation forte pour des raisons de
conformité, et chaque brique doit pouvoir évoluer et être testée indépendamment (exigence pédagogique
du référentiel : "concevoir une architecture évolutive").

## Options envisagées

1. **Monolithe modulaire** (un seul déploiement, modules internes bien séparés)
   - ✅ Plus simple à développer et déployer au départ
   - ✅ Pas de latence réseau entre modules
   - ❌ Un seul langage possible pour tout le monolithe → incompatible avec l'exigence de démontrer
     plusieurs langages (Java/Python/C#)
   - ❌ Impossible de scaler un module indépendamment (ex : scoring ML gourmand en CPU vs auth léger)

2. **Microservices** (un service = un domaine métier = un déploiement indépendant)
   - ✅ Permet un langage adapté par domaine (Python pour le ML, C# pour illustrer la polyvalence)
   - ✅ Scalabilité indépendante par service
   - ✅ Correspond directement aux compétences attendues (CI/CD par service, cloud, conteneurisation)
   - ❌ Complexité opérationnelle plus élevée (orchestration, observabilité, gestion des pannes réseau)
   - ❌ Cohérence des données plus difficile (pas de transaction SQL unique entre services)

3. **Architecture serverless (FaaS)**
   - ✅ Scalabilité automatique, pas de gestion de serveur
   - ❌ Moins pertinent pour démontrer Terraform/Ansible/Kubernetes (compétences explicitement visées)
   - ❌ Vendor lock-in fort

## Décision

On adopte une **architecture microservices**, avec un service par domaine métier (`auth`, `policy`,
`claims`, `risk-scoring`, `payment`), communication synchrone via API REST, orchestrés par Kubernetes.

## Conséquences

- **Positif** : chaque service devient un terrain d'application concret d'un bloc de compétences
  (TDD par service, pipeline CI/CD par service, langage adapté au besoin).
- **Négatif assumé** : on accepte une complexité de démarrage plus élevée qu'un monolithe, et on devra
  gérer explicitement la cohérence des données entre services (ex : que se passe-t-il si le paiement
  échoue après que le sinistre a été marqué "validé" ?). Ce point sera traité en Phase 2 avec un pattern
  de compensation (Saga simplifiée), documenté dans un futur ADR si nécessaire.
- **Négatif assumé** : la latence entre `claims-service` et `risk-scoring-service` (appel synchrone,
  voir diagramme de séquence) devra être surveillée — un ADR futur pourra revenir sur ce choix si les
  temps de réponse deviennent un problème (passage à un appel asynchrone via file de messages).
