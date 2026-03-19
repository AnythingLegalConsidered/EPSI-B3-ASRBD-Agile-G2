# Product Backlog

> Source de vérité : [GitHub Issues](https://github.com/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2/issues)

## Epics

### Epic 1 - Cluster Kubernetes + Applications
Installation K3s sur Proxmox et déploiement Symfony + React.

### Epic 2 - Exposition des services
Rendre frontend et backend accessibles depuis l'extérieur.

### Epic 3 - Sécurité et structuration
SSL/TLS, Secrets, ConfigMaps, Namespaces.

### Epic 4 - Scalabilité et observabilité
HPA pour 1000 users, logs centralisés, monitoring.

## User Stories par Sprint

### Sprint 1 - Cluster K8s fonctionnel (16 points)

| # | User Story | Points | Priorité |
|---|-----------|--------|----------|
| #1 | Créer les VMs sur Proxmox pour le cluster K3s | 3 | High |
| #2 | Installer K3s (master + 2 workers) | 3 | High |
| #3 | Déployer le backend Symfony/PHP | 5 | High |
| #4 | Déployer le frontend React | 3 | High |
| #5 | Documenter la procédure d'installation | 2 | Medium |

### Sprint 2 - Services accessibles (11 points)

| # | User Story | Points | Priorité |
|---|-----------|--------|----------|
| #6 | Exposer le backend via NodePort | 2 | High |
| #7 | Exposer le frontend via NodePort | 2 | High |
| #8 | Configurer Ingress (Traefik) pour router le trafic | 5 | High |
| #9 | Tester et documenter l'accès externe | 2 | Medium |

### Sprint 3 - Sécurité et structure (13 points)

| # | User Story | Points | Priorité |
|---|-----------|--------|----------|
| #10 | Créer des Namespaces (dev, prod) | 2 | High |
| #11 | Configurer SSL/TLS | 5 | High |
| #12 | Utiliser ConfigMaps pour la configuration | 3 | High |
| #13 | Stocker les credentials dans des Secrets | 3 | High |

### Sprint 4 - Scalabilité et robustesse (20 points)

| # | User Story | Points | Priorité |
|---|-----------|--------|----------|
| #14 | Configurer HPA pour 1000 utilisateurs | 5 | High |
| #15 | Tester la résilience (self-healing) | 3 | High |
| #16 | Mettre en place les logs centralisés | 5 | High |
| #17 | Mettre en place le monitoring | 5 | High |
| #18 | Préparer la démo finale | 2 | High |

Total : 60 points sur 4 sprints
