# ADR-002 — Choix d'un langage différent par service

## Statut
Accepté

## Contexte

Le référentiel RNCP exige explicitement la maîtrise de plusieurs langages (Java, Python, C#).
Au-delà de la contrainte pédagogique, chaque domaine métier a des besoins techniques différents qui
justifient objectivement un choix de langage distinct (pas un choix arbitraire "pour cocher la case").

## Options envisagées et décision par service

| Service | Langage retenu | Justification technique (pas seulement pédagogique) |
|---|---|---|
| `auth-service` | Java / Spring Boot | Écosystème de sécurité mature (Spring Security, OAuth2/JWT), standard en entreprise pour l'IAM |
| `policy-service` | Java / Spring Boot | Logique métier transactionnelle classique, cohérence avec l'écosystème JVM déjà choisi pour l'auth |
| `claims-service` | Java / Spring Boot | Idem, et orchestrateur central du scénario "sinistre" (voir diagramme de séquence) |
| `risk-scoring-service` | Python / FastAPI | Écosystème data-science/ML incomparable (scikit-learn, pandas) ; aucune raison de faire du ML en Java |
| `payment-service` | C# / .NET | Démontre l'interopérabilité entre écosystèmes différents (JVM ↔ .NET) via API REST, illustre qu'un système d'information réel est rarement mono-techno |

## Alternative envisagée : tout en Java

- ✅ Simplicité de maintenance, une seule stack à maîtriser
- ❌ Ne permettrait pas de démontrer la compétence "programmation en divers langages" du référentiel
- ❌ Pour le scoring de risque en particulier, refaire en Java ce que Python fait nativement
  (bibliothèques ML) serait un choix technique objectivement moins bon, pas seulement pédagogique

## Décision

On retient un langage différent par service selon la table ci-dessus, avec pour règle que
**la communication inter-services passe exclusivement par des API REST/HTTP + JSON**, jamais par
appel de code direct — c'est la seule façon de faire cohabiter des écosystèmes hétérogènes.

## Conséquences

- **Positif** : chaque service devient un exercice complet et cohérent dans son langage (pas du code
  Java "traduit" artificiellement en Python).
- **Négatif assumé** : la sérialisation JSON entre services impose de maintenir des contrats d'API
  stricts (spécifiés en OpenAPI/Swagger dès la Phase 2) pour éviter les incompatibilités de schéma.
- **Négatif assumé** : pas de partage de code (bibliothèque commune) entre services de langages
  différents — chaque service redéfinit ses propres modèles de données, avec le risque de divergence
  qu'il faudra surveiller par les tests d'intégration.
