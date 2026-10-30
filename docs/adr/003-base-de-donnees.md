# ADR-003 — PostgreSQL comme base de données transactionnelle

## Statut
Accepté

## Contexte

Les données gérées (contrats, sinistres, paiements) sont sensibles et soumises à des contraintes
fortes de cohérence : un contrat ne doit jamais exister à moitié, un paiement doit être atomique.
On a aussi besoin, en Phase 5, d'alimenter un entrepôt de données pour la BI.

## Options envisagées

1. **PostgreSQL** (relationnel, open source)
   - ✅ Garanties ACID fortes, indispensable pour données financières/contractuelles
   - ✅ Excellent support JSON (colonnes `jsonb`) si besoin de flexibilité ponctuelle
   - ✅ Gratuit, largement supporté par Terraform (module RDS) et par tous les frameworks retenus
   - ❌ Moins adapté que du NoSQL pour des données massivement non structurées

2. **MongoDB** (NoSQL orienté documents)
   - ✅ Flexibilité de schéma
   - ❌ Garanties transactionnelles historiquement plus faibles (même si améliorées depuis MongoDB 4+)
   - ❌ Moins naturel pour modéliser des relations fortes (Contrat-Garantie-Sinistre) comme vu dans le
     diagramme de classes

3. **Une base différente par service (polyglot persistence totale)**
   - ✅ Optimal en théorie (chaque service choisit sa base idéale)
   - ❌ Complexité opérationnelle disproportionnée pour un projet pédagogique en solo
   - ❌ Backup/monitoring/sauvegarde à multiplier inutilement

## Décision

**PostgreSQL** pour tous les services transactionnels (`auth`, `policy`, `claims`, `payment`), avec
**une base logique séparée par service** (pas de base partagée unique) pour respecter l'indépendance
microservices décidée en ADR-001, même si le moteur de base de données est le même.

## Conséquences

- **Positif** : cohérence technique, un seul type d'infrastructure de base de données à provisionner
  via Terraform (module RDS PostgreSQL réutilisable pour chaque service).
- **Positif** : le passage vers l'entrepôt de données (Phase 5) est facilité car PostgreSQL supporte
  nativement des requêtes analytiques complexes, utile même en attendant l'ETL complet.
- **Négatif assumé** : aucune jointure SQL native possible entre les bases de deux services différents
  (ex: `policy-service` et `claims-service` ne partagent pas de base) — toute donnée nécessaire d'un
  autre service doit être obtenue par appel API, jamais par requête SQL directe inter-service. C'est
  une contrainte du choix microservices (ADR-001), pas de PostgreSQL en tant que tel.
