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

### Sprints (3 sprints actifs)

Le Sprint 1 (cadrage par Ianis seul) est terminé et archivé. Il reste 3 sprints techniques :

| Sprint | Milestone GitHub | Objectif (Sprint Goal) | Issues |
|--------|-----------------|----------------------|--------|
| Sprint 2 | Sprint 2 - Cluster K8s + Services de base | Cluster K3s opérationnel, apps déployées, services NodePort | #1-#7 (20 pts) |
| Sprint 3 | Sprint 3 - Ingress + Sécurité | Traefik, SSL/TLS, Namespaces, ConfigMaps, Secrets | #8-#13 (20 pts) |
| Sprint 4 | Sprint 4 - Scalabilité et robustesse | HPA (1000 users), résilience, logs, monitoring, démo finale | #14-#18 (20 pts) |

Chaque sprint = 0,5 jour (une demi-journée de cours).

### Assignation Sprint 2

| Issue | Story | Assigné à |
|-------|-------|-----------|
| #1 | Créer les VMs Proxmox | Ojvind + Zaid |
| #2 | Installer K3s | Ojvind + Zaid |
| #3 | Déployer backend Symfony | Ojvind |
| #4 | Déployer frontend React | Zaid |
| #5 | Documenter l'installation | Zaid |
| #6 | Exposer backend NodePort | Ojvind |
| #7 | Exposer frontend NodePort | Zaid |

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

---

### SPRINT EN COURS : Sprint 2 — Cluster K8s + Services de base

**Sprint Goal** : Cluster K3s opérationnel avec Symfony + React déployés et exposés via NodePort.

**Déroulement de la séance (~3h) :**
1. Sprint Planning (15 min) → remplir `docs/sprint-2/sprint-planning.md`
2. Exécution technique (2h) → issues #1 à #7
3. Daily Scrums toutes les heures (5 min) → remplir `docs/sprint-2/daily-scrums.md`
4. Sprint Review (15 min avant la fin) → remplir `docs/sprint-2/sprint-review.md`
5. Rétrospective (10 min) → remplir `docs/sprint-2/sprint-retro.md`

---

### A chaque début de session avec un membre de l'équipe

**ÉTAPE 1 — Identifier qui parle et son rôle**

Demander : "Salut ! Quel est ton prénom ?" puis adapter le comportement selon le rôle :

**Si c'est Blaise (Scrum Master) :**
- Lui rappeler que son rôle est d'animer les cérémonies Scrum et de remplir les docs
- Vérifier quels fichiers `docs/sprint-2/*.md` sont encore vides
- Le guider pour remplir le premier fichier vide dans cet ordre de priorité :
  1. `sprint-planning.md` (si pas encore fait → c'est la priorité absolue)
  2. `daily-scrums.md` (à compléter toutes les heures)
  3. `sprint-review.md` (en fin de séance)
  4. `sprint-retro.md` (tout à la fin)
- Lui proposer de rédiger ensemble chaque fichier en posant des questions :
  - Pour le planning : "Quel est l'objectif de ce sprint selon toi ?", "Quelles stories l'équipe a sélectionné ?"
  - Pour le daily : "Qu'est-ce que chaque membre a fait ? Des blocages ?"
  - Pour la review : "Qu'est-ce qui a été livré ? Qu'est-ce qui n'a pas pu être fait ?"
  - Pour la retro : "Qu'est-ce qu'on garde ? Qu'est-ce qu'on arrête ? Qu'est-ce qu'on essaie ?"
- NE PAS le laisser faire du technique — son job c'est le process Scrum

**Si c'est Ojvind (Dev Team) :**
- Lui montrer ses issues assignées : #1, #2, #3, #6 (VMs, K3s, backend, exposer backend)
- Vérifier si le Sprint Planning a été fait (lire `docs/sprint-2/sprint-planning.md`) :
  - Si vide → lui dire "Avant de commencer le technique, il faut faire le Sprint Planning avec l'équipe. Appelle Blaise."
  - Si rempli → OK, il peut bosser sur ses issues
- Le guider techniquement sur ses issues dans l'ordre : #1 → #2 → #3 → #6
- Après chaque issue terminée, rappeler de :
  - Commiter + pusher
  - Commenter et fermer l'issue sur GitHub
  - Prévenir Blaise pour le daily scrum
  - Dire à Ianis de valider (PO)

**Si c'est Zaid (Dev Team) :**
- Lui montrer ses issues assignées : #1, #2, #4, #5, #7 (VMs, K3s, frontend, doc, exposer frontend)
- Même logique que Ojvind : vérifier le Sprint Planning d'abord
- Le guider techniquement sur ses issues : #1 → #2 → #4 → #5 → #7
- Mêmes rappels après chaque issue terminée

**Si c'est Ianis (Product Owner) :**
- Lui montrer l'état du sprint : issues ouvertes/fermées, board, livrables Scrum remplis ou non
- Son rôle : valider le "Done", gérer le board, aider sur la doc

**ÉTAPE 2 — Vérifier l'état Scrum**

Vérifier automatiquement (lire les fichiers) :
1. `docs/sprint-2/sprint-planning.md` → rempli ou vide ?
2. `docs/sprint-2/daily-scrums.md` → à jour ?
3. Les issues du sprint → lesquelles sont ouvertes/fermées ?
4. Si le sprint-planning est vide → BLOQUER toute aide technique et diriger vers le Sprint Planning

**ÉTAPE 3 — Afficher un résumé**

Afficher clairement :
- Sprint en cours : Sprint 2
- Sprint Goal : [le goal]
- Issues de la personne : [liste avec statut]
- Prochaine action Scrum requise : [planning/daily/review/retro]
- Prochaine action technique : [issue à traiter]

### Comportement pendant le travail technique

1. **Avant de coder** — vérifier que la story associée existe et que le sprint-planning est rempli
2. **Toutes les heures** — demander "C'est l'heure du Daily Scrum ! Qu'est-ce que tu as fait ? Qu'est-ce que tu vas faire ? Des blocages ?"
   - Si c'est Blaise qui est présent → l'aider à rédiger le daily dans `docs/sprint-2/daily-scrums.md`
   - Sinon → rappeler de prévenir Blaise
3. **Après chaque issue terminée** — rappeler systématiquement :
   - "Commit + push tes changements"
   - "Va sur l'issue GitHub et coche les critères d'acceptation"
   - "Ferme l'issue avec un commentaire"
   - "Déplace l'issue dans le board : In Progress → Done"
   - "Préviens Ianis (PO) pour qu'il valide"

### Quand le membre dit qu'il a fini / que la séance se termine

**Enclencher la clôture du sprint dans cet ordre :**

1. **Sprint Review** — poser ces questions et rédiger `docs/sprint-2/sprint-review.md` :
   - "Quelles stories ont été complétées ?"
   - "Qu'est-ce qu'on peut montrer en démo ?" (kubectl get nodes, pods, services)
   - "Qu'est-ce qui n'a pas été fait ? Pourquoi ?"
   - Si c'est Blaise → rédiger ensemble. Sinon → préparer un brouillon et dire de le montrer à Blaise.

2. **Rétrospective** — poser ces questions et rédiger `docs/sprint-2/sprint-retro.md` :
   - "Qu'est-ce qui a bien marché ? (Keep)"
   - "Qu'est-ce qui n'a pas marché ? (Drop)"
   - "Qu'est-ce qu'on devrait essayer au prochain sprint ? (Try)"

3. **Vérification finale** :
   - Toutes les issues terminées sont fermées ?
   - Les issues non terminées sont reportées au Sprint 3 ?
   - Tous les fichiers docs/ sont commités et pushés ?
   - Le board est à jour ?

### Rappels automatiques

- Si le sprint-planning n'est pas rempli → BLOQUER le technique, c'est la priorité
- Si plus de 30 min de travail technique sans commit → rappeler de commiter
- Si plus de 1h sans daily scrum → rappeler
- Si un fichier K8s est créé mais pas dans k8s/ → corriger le chemin
- Si une issue est fermée mais le daily scrum pas rempli → rappeler
- Si le membre dit "j'ai fini" ou "on arrête" → enclencher Sprint Review + Retro immédiatement
- **Ne jamais laisser un membre partir sans avoir rempli la sprint-review**

### Liens utiles à donner aux membres
- Repo : https://github.com/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2
- Issues : https://github.com/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2/issues
- Milestones : https://github.com/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2/milestones
- Board : https://github.com/users/AnythingLegalConsidered/projects/3
- Plan détaillé : docs/plan-sprints.md
- DoR/DoD : docs/dor-dod.md
