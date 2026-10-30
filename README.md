# AssurCore — Projet fil rouge RNCP 39765 (EADL, Bac+5)

Plateforme de gestion des risques, sinistres et paiements pour un assureur multi-branches.
Ce dépôt est construit **phase par phase**, chaque phase correspondant à un ou plusieurs blocs de compétences du référentiel RNCP.

## Où en est-on ?

- [x] Phase 0 — Cadrage (voir `docs/cadrage-projet.md`)
- [x] Phase 1 — Architecture & modélisation (UML + ADR) ← **on y est**
- [ ] Phase 2 — Cœur applicatif (TDD)
- [ ] Phase 3 — CI/CD & DevSecOps
- [ ] Phase 4 — Cloud & IaC
- [ ] Phase 5 — Data, BI & RPA
- [ ] Phase 6 — Sécurité avancée & Blockchain
- [ ] Phase 7 — Dossier RNCP

## Pourquoi cette arborescence ? (à quoi sert chaque dossier)

```
assurcore/
├── docs/
│   ├── uml/            → Diagrammes UML du système (cas d'utilisation, classes, séquence, déploiement)
│   ├── adr/             → Architecture Decision Records : "pourquoi on a choisi X plutôt que Y"
│   └── cadrage-projet.md
│
├── services/             → Un dossier = un microservice = un langage/franework dédié
│   ├── auth-service/           (Java/Spring Boot)   → authentification, JWT, rôles
│   ├── policy-service/         (Java/Spring Boot)   → gestion des contrats d'assurance
│   ├── claims-service/         (Java/Spring Boot)   → gestion des sinistres
│   ├── risk-scoring-service/   (Python/FastAPI)     → scoring de risque via ML
│   └── payment-service/        (C#/.NET)            → traitement des indemnisations
│
├── frontend/             → Interface web (Angular + TypeScript) consommée par le client final
│
├── infra/
│   ├── terraform/        → Infrastructure as Code : provisionnement des ressources cloud
│   └── ansible/          → Configuration/déploiement automatisé des serveurs
│
├── .github/workflows/    → Pipelines CI/CD (build, tests, scan sécurité, déploiement)
│
├── data/
│   ├── etl/               → Scripts d'extraction/transformation/chargement vers l'entrepôt de données
│   └── bi/                 → Dashboards et requêtes décisionnelles
│
├── automation/
│   └── rpa/                 → Bots d'automatisation (contrôle conformité, relances)
│
└── blockchain/           → Module de traçabilité/audit des sinistres (chaîne de hash ou smart contract)
```

**Logique de découpage** : chaque dossier de `services/` est un projet indépendant, déployable et testable
séparément (c'est le principe même d'une architecture microservices — voir `docs/adr/ADR-001*`).
Cette indépendance est ce qui permet ensuite le CI/CD par service (Phase 3) et le passage à l'échelle
sur le cloud (Phase 4).

## Comment lire ce dépôt si tu reprends le projet plus tard

1. Commence toujours par `docs/adr/` : ça explique le "pourquoi" de chaque décision structurante.
2. Puis `docs/uml/` : ça montre le "quoi" (comportement et structure du système).
3. Le code dans `services/` implémente concrètement ces décisions.
