# Sprint 2 - Review

**Date** : 20 mars 2026

## Sprint Goal
Cluster K3s opérationnel avec Symfony (backend) + React (frontend) déployés et exposés via NodePort.

## Stories complétées

| # Issue | User Story | Status |
|---------|-----------|--------|
| #1 | Créer les VMs sur Proxmox pour le cluster K3s | Done |
| #2 | Installer K3s (master + 2 workers) | Partiel — difficultés sur la configuration réseau |
| #3 | Déployer le backend Symfony/PHP | Non fait |
| #4 | Déployer le frontend React | Non fait |
| #5 | Documenter la procédure d'installation | Non fait |
| #6 | Exposer le backend via NodePort | Non fait |
| #7 | Exposer le frontend via NodePort | Non fait |

## Démonstration
- Les VMs Proxmox ont été créées et sont opérationnelles
- Installation K3s démarrée mais bloquée par des problèmes de configuration des cartes réseau
- Ianis a préparé les machines en avance — assignation en cours

## Feedback
- L'équipe a rencontré des difficultés réseau sur la configuration Kubernetes
- Ianis (PO) : les machines sont prêtes, l'assignation permettra de débloquer l'équipe
- Le Sprint Goal n'a pas été atteint entièrement, les issues #2 à #7 sont reportées au Sprint 3

## Stories reportées
- #2 Installer K3s (master + 2 workers) → Sprint 3
- #3 Déployer le backend Symfony/PHP → Sprint 3
- #4 Déployer le frontend React → Sprint 3
- #5 Documenter la procédure d'installation → Sprint 3
- #6 Exposer le backend via NodePort → Sprint 3
- #7 Exposer le frontend via NodePort → Sprint 3
