# ADR-005 — Traçabilité des sinistres par chaîne de hash (blockchain légère)

## Statut
Accepté

## Contexte

Le référentiel mentionne "Utiliser la blockchain pour la sécurité des données". En assurance, un
besoin réel existe : garantir qu'un sinistre déclaré et son historique de traitement n'ont pas été
altérés a posteriori (enjeu de confiance et de conformité réglementaire, ex : lutte anti-fraude).

## Options envisagées

1. **Blockchain publique (Ethereum, smart contract Solidity réel)**
   - ✅ Démonstration "complète" de la technologie blockchain
   - ❌ Coût réel en gas, complexité de déploiement disproportionnée pour un projet pédagogique,
     latence de confirmation incompatible avec un flux métier temps réel
   - ❌ Aucune donnée personnelle (RGPD) ne doit jamais être écrite sur une blockchain publique
     immuable et non conforme au droit à l'effacement — risque juridique réel si mal maîtrisé

2. **Blockchain privée/permissionnée (Hyperledger Fabric)**
   - ✅ Plus réaliste pour un cas d'usage entreprise
   - ❌ Complexité d'infrastructure très élevée pour la valeur pédagogique ajoutée dans ce projet

3. **Chaîne de hash légère (append-only ledger interne)** — chaque événement du cycle de vie d'un
   sinistre (déclaration, instruction, validation, paiement) génère un hash qui inclut le hash de
   l'événement précédent, rendant toute modification rétroactive détectable.
   - ✅ Aucune donnée personnelle stockée sur la chaîne (seulement des empreintes/hash)
   - ✅ Démontre le principe cryptographique fondamental de la blockchain (chaînage, immutabilité,
     détection d'altération) sans la complexité opérationnelle d'un vrai réseau distribué
   - ❌ Ne bénéficie pas de la décentralisation d'une vraie blockchain (un seul nœud de vérité ici)

## Décision

On implémente une **chaîne de hash interne (append-only ledger)** dans le module `blockchain/`,
qui journalise l'empreinte SHA-256 de chaque changement d'état d'un sinistre. Ce choix est assumé
comme une **simplification pédagogique consciente**, explicitement documentée comme telle (et non
présentée comme une "vraie" blockchain décentralisée) — un point qui sera mis en avant en soutenance
pour montrer la capacité à adapter une technologie à un besoin réel plutôt qu'à l'utiliser pour l'effet
d'annonce.

## Conséquences

- **Positif** : le module reste simple à opérer, sans dépendance à un réseau blockchain externe, tout
  en démontrant la compétence "utiliser la blockchain pour la sécurité des données".
- **Positif** : conforme RGPD par construction (seuls des hash sont stockés, jamais de données brutes).
- **Négatif assumé** : en cas de compromission du service qui héberge la chaîne de hash elle-même,
  l'intégrité n'est plus garantie par une décentralisation externe — ce point sera explicitement
  mentionné dans le dossier RNCP comme une limite connue et assumée du choix, avec la vraie blockchain
  décentralisée citée comme évolution possible en environnement de production réel.
