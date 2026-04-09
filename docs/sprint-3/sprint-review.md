# Sprint 3 - Review

**Date** : 8 avril 2026
**Durée du sprint** : 3 → 8 avril 2026
**Scrum Master** : Blaise
**Product Owner** : Ianis

---

## Sprint Goal

> Sécuriser et structurer l'infrastructure K3s : configurer Ingress (Traefik), activer SSL/TLS, créer des Namespaces dev/prod, et centraliser la configuration via ConfigMaps et Secrets Kubernetes.

---

## Stories complétées

| # Issue | User Story | Points | Status |
|---------|-----------|--------|--------|
| #10 | Créer des Namespaces (dev, prod) | 2 | Done |
| #13 | Stocker les credentials dans des Secrets K8s | 3 | Done |
| #8 | Configurer Ingress (Traefik) pour router le trafic | 5 | Done |
| #12 | Utiliser ConfigMaps pour la configuration applicative | 3 | Done |
| #11 | Configurer SSL/TLS pour sécuriser les accès | 5 | Partiel |
| #9 | Tester et documenter l'accès externe | 2 | Done |

**Points livrés : 15 / 20**

---

## Démonstration

Les éléments suivants ont été présentés au Product Owner :

- **Namespaces** : `kubectl get namespaces` → namespaces `dev` et `prod` visibles, toutes les ressources déployées en dehors de `default`.
- **Ingress Traefik** : routage HTTP fonctionnel — `http://app.k3s.local/` redirige vers le frontend React, `http://app.k3s.local/api` vers le backend Symfony.
- **ConfigMaps** : variables de configuration applicative externalisées (`kubectl get configmap -n prod`).
- **Secrets** : credentials base de données et clés d'API stockés en Secrets K8s (`kubectl get secrets -n prod`).
- **SSL/TLS** : HTTPS fonctionnel avec certificat auto-signé (`https://app.k3s.local/`). Avertissement navigateur attendu en environnement lab (Let's Encrypt non disponible sur réseau isolé).

---

## Feedback

**Ianis (Product Owner) :**
> "L'Ingress fonctionne bien et le routage est propre. Le SSL avec certificat auto-signé est acceptable pour l'environnement lab — l'important est que le HTTPS est actif. Les ConfigMaps et Secrets sont bien structurés. La story #11 est validée comme partielle : le certificat auto-signé répond au besoin de chiffrement en transit, même sans Let's Encrypt."

**Équipe :**
> La configuration Traefik a pris plus de temps que prévu (annotations IngressRoute spécifiques à K3s). La migration des variables d'env vers ConfigMaps a demandé de retravailler les manifestes du Sprint 2. SSL a été le point le plus complexe à cause du réseau isolé.

---

## Stories reportées

| # Issue | Raison du report |
|---------|-----------------|
| #11 (partiel) | Certificat auto-signé livré. Let's Encrypt non activable en réseau lab isolé — pas de résolution DNS publique ni d'accès HTTP:80 depuis Internet. Sera noté dans la doc technique. |

Aucune story intégralement non faite. L'objectif principal du sprint (infrastructure sécurisée et structurée) est atteint.
