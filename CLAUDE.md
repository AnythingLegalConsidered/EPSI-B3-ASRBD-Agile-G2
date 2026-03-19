# Projet Agile/Scrum - Kubernetes (EPSI Nantes B3 ASRBD 2025-2026)

## Contexte du projet

Ce repo est un exercice scolaire de méthodologie Agile/Scrum appliqué à un projet d'infrastructure Kubernetes.

**Le but principal N'EST PAS technique.** L'objectif est de pratiquer Scrum correctement et de produire les livrables attendus. Kubernetes est le prétexte technique — pas obligatoire que tout fonctionne, mais la méthode Scrum doit être respectée et tracée.

Cours de référence : https://github.com/kevinniel/2526-EPSINANTES-ASRBD-AGILE
TP : https://github.com/kevinniel/2526-EPSINANTES-ASRBD-AGILE/blob/main/tp.md

## Equipe 2 (Groupe 2)

| GitHub | Prénom | Rôle Scrum |
|--------|--------|-----------|
| AnythingLegalConsidered | Ianis | A définir |
| tequilla77 | Ojvind | A définir |
| blaisecarel | Blaise | A définir |
| zaidAbouyaala | Zaid | A définir |

### Rôles Scrum

- **Product Owner (1 personne)** : Rédige et priorise les User Stories, définit les critères d'acceptation, valide le "Done"
- **Scrum Master (1 personne)** : Anime les cérémonies, s'assure que Scrum est respecté, rédige les comptes rendus
- **Dev Team (2 personnes)** : Exécutent les tâches techniques Kubernetes, rédigent la doc technique

## Contexte client (besoins clarifiés 19/03)

- **Stack** : Symfony/PHP (backend) + React (frontend)
- **Ports** : SSL/TLS obligatoire
- **Utilisateurs** : 1 000 simultanés
- **Logs** : obligatoires
- **Monitoring** : requis
- **Accessibilité** : externe
- **Images Docker** : utiliser des images existantes
- **Deadline** : 8 avril 12h

## Organisation Scrum

### Sprints

| Sprint | Milestone GitHub | Objectif (Sprint Goal) |
|--------|-----------------|----------------------|
| Sprint 1 | Sprint 1 - Cluster K8s fonctionnel | Cluster K3s opérationnel + apps Symfony et React déployées |
| Sprint 2 | Sprint 2 - Services accessibles | Services accessibles depuis l'extérieur (NodePort, Ingress) |
| Sprint 3 | Sprint 3 - Sécurité et structure | SSL/TLS, Secrets, ConfigMaps, Namespaces |
| Sprint 4 | Sprint 4 - Scalabilité et robustesse | HPA (1000 users), logs, monitoring, démo finale |

Chaque sprint = 0,5 jour (une demi-journée de cours).

### Cérémonies par sprint

Pour chaque sprint, produire ces livrables dans docs/sprint-X/ :

1. **Sprint Planning** (sprint-planning.md) : Sprint Goal + stories sélectionnées + estimations
2. **Daily Scrums** (daily-scrums.md) : Qu'ai-je fait ? Que vais-je faire ? Blocages ?
3. **Sprint Review** (sprint-review.md) : Ce qui a été réalisé, démonstration, écarts
4. **Sprint Retrospective** (sprint-retro.md) : Keep / Drop / Try

### Product Backlog

Géré via GitHub Issues avec labels :
- Types : epic, user-story, task, spike
- Priorités : priority:high, priority:medium, priority:low
- Points Fibonacci : points:1 à points:13

Format User Story : "En tant que [rôle], je veux [fonctionnalité], afin de [bénéfice]."

### Definition of Ready (DoR)

Une story est prête quand :
- Rédigée au format "En tant que... je veux... afin de..."
- Critères d'acceptation définis et testables
- Estimation en points faite (Planning Poker)
- Compréhensible par toute l'équipe

### Definition of Done (DoD)

Une story est terminée quand :
- Fonctionnelle et testée
- Déployée sur Kubernetes
- Documentée
- Code/config poussé sur le repo

## Infrastructure technique : Proxmox + K3s

### Environnement

Serveur Proxmox à l'école. Le lab MSPR2 utilise 192.168.10.0/24 sur vmbr10, VMIDs 1010-1062.

### Cluster Kubernetes prévu

| VM | VMID | IP | OS | Rôle K8s | Specs |
|----|------|----|----|----------|-------|
| k8s-master | 1100 | 192.168.10.100 | Ubuntu 22.04 | K3s server | 2 vCPU, 2 Go RAM, 20 Go |
| k8s-worker1 | 1101 | 192.168.10.101 | Ubuntu 22.04 | K3s agent | 2 vCPU, 2 Go RAM, 20 Go |
| k8s-worker2 | 1102 | 192.168.10.102 | Ubuntu 22.04 | K3s agent | 2 vCPU, 2 Go RAM, 20 Go |

### Réseau

```
Internet
    |
[Proxmox Host] -- vmbr0 (réseau physique)
    |
    +-- NAT (iptables MASQUERADE)
    |
    +-- vmbr10 (192.168.10.0/24) -- réseau lab isolé
         |
         +-- .1    Gateway (Proxmox)
         +-- .10-.62  VMs MSPR2 (NE PAS TOUCHER)
         +-- .100  k8s-master (K3s server)
         +-- .101  k8s-worker1 (K3s agent)
         +-- .102  k8s-worker2 (K3s agent)
```

### Installation K3s

Sur le master :
```bash
curl -sfL https://get.k3s.io | sh -
cat /var/lib/rancher/k3s/server/node-token
```

Sur chaque worker :
```bash
curl -sfL https://get.k3s.io | K3S_URL=https://192.168.10.100:6443 K3S_TOKEN=<token> sh -
```

## Board GitHub Project

Colonnes : Backlog > Sprint Backlog > In Progress > In Review > Done

## Structure du repo

```
/
+-- CLAUDE.md
+-- README.md
+-- docs/
|   +-- product-backlog.md
|   +-- dor-dod.md
|   +-- sprint-1/ (planning, daily, review, retro)
|   +-- sprint-2/
|   +-- sprint-3/
|   +-- sprint-4/
+-- k8s/
```

## Critères d'évaluation

1. Qualité des livrables Scrum
2. Respect méthodologique
3. Traçabilité Git (repo public)
4. Cohérence technique
5. Collaboration d'équipe

## Instructions pour Claude Code

- Langue : Français (code/config commentés en anglais)
- Quand un membre demande "qu'est-ce que je dois faire", expliquer son rôle Scrum et les livrables concrets
- Vérifier que les livrables Scrum sont produits dans docs/sprint-X/
- Aider avec la config Kubernetes mais rappeler que l'objectif principal est la méthodo Scrum
- Utiliser les GitHub Issues et Milestones pour tout le suivi
- Ne jamais inclure "Co-Authored-By" dans les commits
