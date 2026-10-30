# ADR-004 — Terraform (provisionnement) + Ansible (configuration)

## Statut
Accepté

## Contexte

Le référentiel exige explicitement "Automatiser la gestion des ressources avec Terraform et Ansible".
Il faut comprendre que ces deux outils ne sont pas interchangeables mais complémentaires, et le
choisir consciemment plutôt que de les utiliser comme deux boîtes à cocher.

## Options envisagées

1. **Terraform seul**
   - ✅ Excellent pour le provisionnement déclaratif de ressources cloud (créer un cluster, une base,
     un réseau)
   - ❌ Pas conçu pour la configuration fine post-provisionnement (installer un paquet, modifier un
     fichier de config sur une VM)

2. **Ansible seul**
   - ✅ Excellent pour la configuration de machines existantes (agentless, idempotent)
   - ❌ Moins naturel pour créer/détruire des ressources cloud de façon déclarative avec état
     (Terraform maintient un "state" qui sait ce qui existe déjà, pas Ansible)

3. **Terraform pour provisionner + Ansible pour configurer (combinaison)**
   - ✅ Chaque outil fait ce pour quoi il est le meilleur
   - ✅ Correspond exactement à un usage réel en entreprise
   - ❌ Deux outils à maintenir, deux syntaxes à apprendre

## Décision

**Terraform** provisionne l'infrastructure cloud (réseau, cluster Kubernetes managé, base de données
managée, load balancer — voir diagramme de déploiement). **Ansible** intervient ensuite pour toute
configuration qui n'est pas nativement gérée par le cluster Kubernetes lui-même (ex : bootstrap de
runners CI auto-hébergés, durcissement sécurité de VM annexes si besoin).

Dans ce projet, la majorité de la configuration applicative passe par Kubernetes (manifests/Helm) et
non par Ansible, car les workloads sont conteneurisés — Ansible garde un rôle volontairement ciblé pour
rester cohérent avec l'architecture cloud-native choisie, plutôt que d'être utilisé artificiellement
pour justifier la compétence.

## Conséquences

- **Positif** : séparation claire des responsabilités, traçable dans `infra/terraform/` (état
  déclaratif de l'infra) et `infra/ansible/` (playbooks de configuration ciblés).
- **Négatif assumé** : deux outils différents à maintenir en cohérence (ex: si Terraform détruit une VM,
  il faut s'assurer qu'Ansible n'y fait plus référence) — géré par une documentation claire de ce que
  chaque outil possède.
