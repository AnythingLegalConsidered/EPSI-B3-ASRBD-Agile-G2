# Projet Agile/Scrum - Kubernetes (EPSI Nantes B3 ASRBD 2025-2026)

## Contexte du projet

Ce repo est un exercice scolaire de méthodologie Agile/Scrum appliqué à un projet d'infrastructure Kubernetes.

**Le but principal N'EST PAS technique.** L'objectif est de pratiquer Scrum correctement et de produire les livrables attendus. Kubernetes est le prétexte technique — pas obligatoire que tout fonctionne, mais la méthode Scrum doit être respectée et tracée.

Cours de référence : https://github.com/kevinniel/2526-EPSINANTES-ASRBD-AGILE
TP : https://github.com/kevinniel/2526-EPSINANTES-ASRBD-AGILE/blob/main/tp.md

## Equipe 2 (Groupe 2)

| GitHub | Prénom | Rôle Scrum |
|--------|--------|-----------|
| AnythingLegalConsidered | Ianis | Product Owner |
| tequilla77 | Ojvind | Dev Team |
| blaisecarel | Blaise | Scrum Master |
| zaidAbouyaala | Zaid | Dev Team |

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

## Instructions pour Claude Code (OBLIGATOIRE)

### Langue
- Répondre en français
- Code et config commentés en anglais

### Commits
- Ne jamais inclure "Co-Authored-By" dans les messages de commit

### Principe fondamental
**L'objectif de ce projet est la METHODOLOGIE SCRUM, pas la technique Kubernetes.**
Avant toute action technique, vérifier que le processus Scrum est respecté.

### A chaque début de session avec un membre de l'équipe

1. **Identifier qui parle** — demander le prénom si pas clair, vérifier son rôle Scrum dans le tableau équipe ci-dessus
2. **Identifier le sprint en cours** — vérifier les milestones GitHub : `gh api repos/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2/milestones`
3. **Vérifier l'état des livrables Scrum du sprint en cours** :
   - Le `sprint-planning.md` est-il rempli ? Si non, c'est la priorité.
   - Le `daily-scrums.md` est-il à jour ? Rappeler de le compléter.
   - Y a-t-il des issues assignées à cette personne ? Les montrer.
4. **Afficher un résumé de situation** :
   - Sprint en cours et son goal
   - Issues ouvertes du sprint (avec `gh issue list --repo AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2 --milestone "Sprint X"`)
   - Ce qui reste à faire

### Quand un membre demande de l'aide technique (K8s, manifestes, etc.)

1. **Avant de coder** — vérifier que la story associée existe dans les issues GitHub
2. **Pendant le travail** — rappeler de faire un daily scrum si pas encore fait aujourd'hui
3. **Après le travail** — rappeler de :
   - Commiter et pusher sur le repo
   - Mettre à jour l'issue GitHub (commentaire + fermer si terminé)
   - Déplacer l'issue dans le board (In Progress → Done)
   - Vérifier si les critères d'acceptation de l'issue sont tous cochés

### Quand un membre demande "qu'est-ce que je dois faire ?"

1. Rappeler son **rôle Scrum** et ce que ça implique concrètement
2. Montrer les **issues du sprint en cours** qui lui sont assignées (ou non assignées)
3. Vérifier si les **livrables Scrum** du sprint sont remplis (planning, daily, review, retro)
4. Pointer vers le **plan détaillé** dans `docs/plan-sprints.md`

### Quand une story est terminée

1. Vérifier que TOUS les critères d'acceptation sont cochés dans l'issue
2. Vérifier que la DoD est respectée (fonctionnel, testé, déployé, documenté, pushé)
3. Fermer l'issue avec un commentaire résumant ce qui a été fait
4. Rappeler de mettre à jour le board GitHub Project

### Quand un sprint se termine

Rappeler de remplir DANS CET ORDRE :
1. `docs/sprint-X/sprint-review.md` — ce qui a été fait, démo, écarts
2. `docs/sprint-X/sprint-retro.md` — Keep / Drop / Try
3. Fermer les issues terminées, reporter les non-terminées au sprint suivant
4. Commiter et pusher tous les fichiers docs/

### Quand un sprint commence

Rappeler de faire DANS CET ORDRE :
1. Le PO présente les stories prioritaires du backlog
2. Planning Poker pour estimer (suite Fibonacci)
3. L'équipe sélectionne les stories pour le sprint
4. Remplir `docs/sprint-X/sprint-planning.md` avec le goal + stories sélectionnées
5. Assigner les issues aux membres dans GitHub
6. Déplacer les issues de Backlog → Sprint Backlog dans le board

### Rappels automatiques

- Si plus de 30 min de travail technique sans commit → rappeler de commiter
- Si un fichier K8s est créé mais pas dans k8s/ → corriger le chemin
- Si une issue est fermée mais le daily scrum pas rempli → rappeler
- Si on approche de la fin de session → rappeler de faire la review + retro

### Liens utiles à donner aux membres
- Repo : https://github.com/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2
- Issues : https://github.com/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2/issues
- Milestones : https://github.com/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2/milestones
- Board : https://github.com/users/AnythingLegalConsidered/projects/3
- Plan détaillé : docs/plan-sprints.md
- DoR/DoD : docs/dor-dod.md
