# Procédure d'installation — Cluster K3s + Applications

**Rédigé par** : Zaid (Dev Team)
**Date** : 24 mars 2026

---

## 1. Prérequis

- 3 VMs Ubuntu 22.04 créées sur Proxmox
- Accès SSH aux VMs (user : grp2)
- Réseau : master (172.16.60.4), worker1 (172.16.89.45), worker2 (172.16.60.9)

---

## 2. Installation K3s

### Sur le master

```bash
curl -sfL https://get.k3s.io | sh -
# Récupérer le token pour les workers
sudo cat /var/lib/rancher/k3s/server/node-token
```

### Sur chaque worker

```bash
curl -sfL https://get.k3s.io | K3S_URL=https://172.16.60.4:6443 K3S_TOKEN=<token> sh -
```

> **Note** : S'assurer que chaque VM a un hostname unique avant l'installation.
> ```bash
> sudo hostnamectl set-hostname k8s-worker1  # sur worker1
> sudo hostnamectl set-hostname k8s-worker2  # sur worker2
> sudo systemctl restart k3s-agent
> ```

### Vérification

```bash
sudo kubectl get nodes
# Résultat attendu : 3 nœuds en statut Ready
```

---

## 3. Déploiement des applications

### Cloner le repo sur le master

```bash
git clone https://github.com/AnythingLegalConsidered/EPSI-B3-ASRBD-Agile-G2.git
cd EPSI-B3-ASRBD-Agile-G2
```

### Appliquer les manifestes

```bash
# Namespaces
sudo kubectl apply -f k8s/namespace.yaml

# Backend Symfony (PHP)
sudo kubectl apply -f k8s/configmap-backend.yaml
sudo kubectl apply -f k8s/secrets.yaml
sudo kubectl apply -f k8s/backend-deployment.yaml
sudo kubectl apply -f k8s/backend-service.yaml

# Frontend React (nginx)
sudo kubectl apply -f k8s/configmap-frontend.yaml
sudo kubectl apply -f k8s/frontend-deployment.yaml
sudo kubectl apply -f k8s/frontend-service.yaml
```

### Vérification

```bash
sudo kubectl get pods -n prod
sudo kubectl get svc -n prod
```

---

## 4. Accès aux applications

| Service | URL | Port |
|---------|-----|------|
| Backend (PHP/Symfony) | http://172.16.60.4:30080 | NodePort 30080 |
| Frontend (React/nginx) | http://172.16.60.4:30081 | NodePort 30081 |

---

## 5. Problèmes rencontrés et solutions

| Problème | Solution |
|----------|----------|
| Workers non visibles dans `kubectl get nodes` | Conflit de hostname : toutes les VMs avaient le même nom "grp2". Fix : renommer les workers avec `hostnamectl set-hostname` |
| `ImagePullBackOff` sur backend | L'image `bitnami/symfony` n'existe plus. Remplacée par `php:8.2-apache` |
| Pods backend en `CrashLoopBackOff` | Readiness probe `httpGet /` retournait 403 (pas de fichier index). Fix : passer les probes en `tcpSocket` |
