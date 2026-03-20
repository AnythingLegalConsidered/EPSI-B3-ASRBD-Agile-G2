# Sprint 2 - Planning

**Date** : 20 mars 2026

## Sprint Goal
Cluster K3s opérationnel avec Symfony (backend) + React (frontend) déployés et exposés via NodePort.

## User Stories sélectionnées

| # Issue | User Story | Points | Assigné à |
|---------|-----------|--------|-----------|
| #1 | Créer les VMs sur Proxmox pour le cluster K3s | 3 | Ojvind + Zaid |
| #2 | Installer K3s (master + 2 workers) | 3 | Ojvind + Zaid |
| #3 | Déployer le backend Symfony/PHP | 5 | Ojvind |
| #4 | Déployer le frontend React | 3 | Zaid |
| #5 | Documenter la procédure d'installation | 2 | Zaid |
| #6 | Exposer le backend via NodePort | 2 | Ojvind |
| #7 | Exposer le frontend via NodePort | 2 | Zaid |

## Estimation totale : 20 points

## Notes
- Sprint d'une demi-journée (~3h)
- Infrastructure sur Proxmox : 3 VMs Ubuntu 22.04 (k8s-master: 192.168.10.100, k8s-worker1: 192.168.10.101, k8s-worker2: 192.168.10.102)
- Réseau lab isolé : 192.168.10.0/24
- Blaise (Scrum Master) anime les cérémonies et remplit les docs
- Ianis (PO) valide chaque issue terminée
