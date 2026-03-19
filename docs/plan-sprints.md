# Plan détaillé des Sprints

> Contexte : 4 demi-journées, 1 sprint = 1 demi-journée (~3h)
> Deadline finale : 8 avril 12h

---

## Sprint 1 (demi-journée 1 — passée)

**Sprint Goal** : Formation de l'équipe et découverte du sujet

### Ce qui a été fait
- Prise de connaissance du TP et des exigences Scrum
- Formation de l'équipe (Ianis, Ojvind, Blaise, Zaid)
- Premières discussions sur l'approche technique

### Livrables Scrum à remplir
- [ ] `docs/sprint-1/sprint-planning.md` — Objectif : se familiariser avec le sujet
- [ ] `docs/sprint-1/daily-scrums.md` — Résumé des échanges
- [ ] `docs/sprint-1/sprint-review.md` — Résultat : compréhension du sujet acquise
- [ ] `docs/sprint-1/sprint-retro.md` — Keep/Drop/Try

---

## Sprint 2 (demi-journée 2 — aujourd'hui, 19 mars)

**Sprint Goal** : Cadrage Scrum complet et préparation technique

**Qui** : Ianis (seul, pas d'accès Proxmox)

### Étape 1 — Sprint Planning (15 min)
1. Définir le Sprint Goal
2. Sélectionner les stories du Sprint 2 :
   - Mise en place du repo Git et de la structure projet
   - Rédaction du Product Backlog complet
   - Définition du DoR et DoD
   - Création du board Kanban
   - Préparation technique : choix des images Docker, plan d'infra
3. Remplir `docs/sprint-2/sprint-planning.md`

### Étape 2 — Exécution (2h)

#### Tâche 2.1 : Mise en place du repo et outils (FAIT)
- [x] Créer le repo GitHub public
- [x] Inviter les collaborateurs
- [x] Créer les milestones (4 sprints)
- [x] Créer les labels (types, priorités, points)
- [x] Créer le board Kanban (GitHub Project)
- [x] Pousser la structure de fichiers (docs/, k8s/)

#### Tâche 2.2 : Rédaction du Product Backlog (FAIT)
- [x] Rédiger les User Stories au format standard
- [x] Définir les critères d'acceptation pour chaque story
- [x] Estimer en points Fibonacci
- [x] Créer les 18 issues GitHub avec labels et milestones

#### Tâche 2.3 : Définition DoR et DoD (FAIT)
- [x] Rédiger la Definition of Ready
- [x] Rédiger la Definition of Done
- [x] Documenter dans docs/dor-dod.md

#### Tâche 2.4 : Préparation technique (A FAIRE)
- [ ] Choisir les images Docker exactes :
  - Backend : `bitnami/symfony` ou `php:8.2-fpm` + `nginx`
  - Frontend : `node:18-alpine` servant un build React via nginx
  - Base de données : `mysql:8.0` ou `postgres:15`
- [ ] Préparer les manifestes YAML de base dans k8s/ :
  - `k8s/namespace.yaml`
  - `k8s/backend-deployment.yaml`
  - `k8s/frontend-deployment.yaml`
  - `k8s/backend-service.yaml`
  - `k8s/frontend-service.yaml`
- [ ] Documenter le plan d'infra Proxmox (VMs, IPs, specs)

#### Tâche 2.5 : Attribution des rôles Scrum
- [ ] Décider avec l'équipe (par message si besoin) :
  - Product Owner : ???
  - Scrum Master : ???
  - Dev Team : ???, ???
- [ ] Mettre à jour CLAUDE.md et README.md avec les rôles

### Étape 3 — Daily Scrum (5 min)
- Remplir `docs/sprint-2/daily-scrums.md`
- Format : Fait / A faire / Blocages

### Étape 4 — Sprint Review (10 min)
- Lister ce qui a été accompli
- Remplir `docs/sprint-2/sprint-review.md`

### Étape 5 — Rétrospective (10 min)
- Remplir `docs/sprint-2/sprint-retro.md` (Keep/Drop/Try)
- Ex: Keep = "bon cadrage en amont" / Drop = "attendre le dernier moment" / Try = "mieux se coordonner"

---

## Sprint 3 (demi-journée 3 — prochaine séance, équipe complète + Proxmox)

**Sprint Goal** : Cluster K8s opérationnel avec applications déployées et accessibles

**Qui** : Toute l'équipe (4 personnes)

### Étape 1 — Sprint Planning (15 min)
1. Le PO présente les stories prioritaires
2. Planning Poker pour valider/ajuster les estimations
3. Sélectionner les stories :
   - #1 Créer les VMs sur Proxmox (3 pts)
   - #2 Installer K3s (3 pts)
   - #3 Déployer le backend Symfony (5 pts)
   - #4 Déployer le frontend React (3 pts)
   - #6 Exposer le backend via NodePort (2 pts)
   - #7 Exposer le frontend via NodePort (2 pts)
   - #8 Configurer Ingress Traefik (5 pts)
4. Assigner les stories aux membres
5. Remplir `docs/sprint-3/sprint-planning.md`

### Étape 2 — Exécution (2h)

**Travail en parallèle :**

#### Binôme A (Infra) — 2 personnes
**Phase 1 (30 min) : VMs + K3s**
1. Se connecter au Proxmox
2. Créer 3 VMs Ubuntu 22.04 :
   - k8s-master (VMID 1100, IP .100)
   - k8s-worker1 (VMID 1101, IP .101)
   - k8s-worker2 (VMID 1102, IP .102)
3. Configurer les IPs statiques sur vmbr10
4. Installer K3s :
   ```
   # Sur master
   curl -sfL https://get.k3s.io | sh -
   cat /var/lib/rancher/k3s/server/node-token

   # Sur chaque worker
   curl -sfL https://get.k3s.io | K3S_URL=https://192.168.10.100:6443 K3S_TOKEN=<token> sh -
   ```
5. Vérifier : `kubectl get nodes` → 3 nodes Ready

**Phase 2 (45 min) : Déploiement apps**
6. Appliquer les manifestes YAML préparés au Sprint 2 :
   ```
   kubectl apply -f k8s/namespace.yaml
   kubectl apply -f k8s/backend-deployment.yaml
   kubectl apply -f k8s/frontend-deployment.yaml
   ```
7. Vérifier : `kubectl get pods -n prod` → pods Running

**Phase 3 (30 min) : Exposition**
8. Appliquer les Services :
   ```
   kubectl apply -f k8s/backend-service.yaml
   kubectl apply -f k8s/frontend-service.yaml
   ```
9. Configurer l'Ingress Traefik :
   ```
   kubectl apply -f k8s/ingress.yaml
   ```
10. Tester l'accès depuis une autre machine du réseau

#### Binôme B (Documentation) — 2 personnes
1. Documenter chaque étape au fur et à mesure
2. Rédiger `docs/sprint-3/daily-scrums.md` (toutes les heures)
3. Prendre des captures d'écran des résultats
4. Mettre à jour les issues GitHub (déplacer dans le board)
5. Fermer les issues terminées avec un commentaire

### Étape 3 — Daily Scrum (toutes les heures, 5 min)
- Tour de table : Fait / A faire / Blocages
- Remplir `docs/sprint-3/daily-scrums.md`

### Étape 4 — Sprint Review (15 min avant la fin)
- Démo : montrer le cluster, les pods, l'accès web
- Remplir `docs/sprint-3/sprint-review.md`

### Étape 5 — Rétrospective (10 min)
- Keep / Drop / Try
- Remplir `docs/sprint-3/sprint-retro.md`

---

## Sprint 4 (demi-journée 4 — dernière séance, avant le 8 avril 12h)

**Sprint Goal** : Plateforme sécurisée, scalable, monitorée + démo finale

**Qui** : Toute l'équipe (4 personnes)

### Étape 1 — Sprint Planning (15 min)
1. Bilan du Sprint 3 : qu'est-ce qui manque ?
2. Sélectionner les stories :
   - #10 Namespaces dev/prod (2 pts)
   - #11 SSL/TLS (5 pts)
   - #12 ConfigMaps (3 pts)
   - #13 Secrets (3 pts)
   - #14 HPA autoscaling (5 pts)
   - #15 Test résilience (3 pts)
   - #16 Logs centralisés (5 pts)
   - #17 Monitoring (5 pts)
   - #18 Démo finale (2 pts)
3. Si des stories du Sprint 3 n'ont pas été finies, les reprioriser
4. Remplir `docs/sprint-4/sprint-planning.md`

### Étape 2 — Exécution (2h)

**Travail en parallèle :**

#### Binôme A (Sécurité + Config) — 45 min
1. Créer les Namespaces :
   ```
   kubectl apply -f k8s/namespaces.yaml
   ```
2. Configurer SSL avec Traefik (cert self-signed) :
   ```
   kubectl apply -f k8s/tls-secret.yaml
   kubectl apply -f k8s/ingress-tls.yaml
   ```
3. Créer les ConfigMaps :
   ```
   kubectl apply -f k8s/configmap-backend.yaml
   kubectl apply -f k8s/configmap-frontend.yaml
   ```
4. Créer les Secrets :
   ```
   kubectl apply -f k8s/secrets.yaml
   ```

#### Binôme B (Scalabilité + Monitoring) — 45 min
1. Configurer le HPA :
   ```
   kubectl apply -f k8s/hpa-backend.yaml
   kubectl apply -f k8s/hpa-frontend.yaml
   ```
2. Vérifier metrics-server : `kubectl top nodes`
3. Déployer le dashboard K8s ou Grafana :
   ```
   kubectl apply -f k8s/monitoring/
   ```

#### Ensemble (30 min) — Tests + Documentation
1. Tester la résilience :
   ```
   kubectl delete pod <nom-pod>
   kubectl get pods -w  # observer la recréation
   ```
2. Tester les logs : `kubectl logs -f <pod>`
3. Tester l'accès HTTPS
4. Documenter tous les résultats

#### Ensemble (30 min) — Démo finale
1. Préparer un scénario de démo :
   - Montrer le cluster (kubectl get nodes)
   - Montrer les apps (accès web frontend + API backend)
   - Montrer l'Ingress et le routage
   - Montrer le SSL/HTTPS
   - Montrer le scaling (kubectl get hpa)
   - Montrer le self-healing (delete pod → recréation)
   - Montrer les logs
   - Montrer le monitoring
2. Documenter dans `docs/sprint-4/sprint-review.md`

### Étape 3 — Daily Scrum (toutes les heures)
- Remplir `docs/sprint-4/daily-scrums.md`

### Étape 4 — Sprint Review finale (15 min)
- Démonstration de la plateforme complète
- Remplir `docs/sprint-4/sprint-review.md`

### Étape 5 — Rétrospective finale (10 min)
- Keep / Drop / Try sur l'ensemble du projet
- Remplir `docs/sprint-4/sprint-retro.md`

### Étape 6 — Livraison
- [ ] Vérifier que tous les fichiers docs/ sont remplis
- [ ] Vérifier que toutes les issues sont fermées ou commentées
- [ ] Vérifier que le board est à jour
- [ ] Soumettre le lien du repo sur Teams

---

## Checklist finale (avant le 8 avril 12h)

### Livrables Scrum
- [ ] Product Backlog complet (issues GitHub)
- [ ] DoR et DoD définis (docs/dor-dod.md)
- [ ] 4x Sprint Planning (docs/sprint-X/sprint-planning.md)
- [ ] 4x Daily Scrums (docs/sprint-X/daily-scrums.md)
- [ ] 4x Sprint Review (docs/sprint-X/sprint-review.md)
- [ ] 4x Sprint Retrospective (docs/sprint-X/sprint-retro.md)
- [ ] Estimations Planning Poker visibles (labels points)
- [ ] Board Kanban à jour

### Traçabilité Git
- [ ] Repo public
- [ ] Commits réguliers de chaque membre
- [ ] Issues utilisées et fermées
- [ ] Milestones avec progression visible

### Technique
- [ ] Manifestes K8s dans k8s/
- [ ] Documentation d'installation
- [ ] Architecture réseau documentée
