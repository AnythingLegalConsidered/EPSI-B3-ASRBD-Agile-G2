# Sprint 3 - Planning

**Date** : 3 avril 2026
**Durée** : 5 jours (3 → 8 avril 2026, deadline : 8 avril 12h)
**Scrum Master** : Blaise
**Product Owner** : Ianis

---

## Sprint Goal

> Sécuriser et structurer l'infrastructure K3s : configurer Ingress (Traefik) pour router le trafic, activer SSL/TLS, organiser les ressources en Namespaces, et centraliser la configuration applicative via ConfigMaps et Secrets Kubernetes.

---

## Contexte de démarrage

À l'entrée du Sprint 3 :
- Cluster K3s opérationnel (3 nœuds en Ready — fix conflit hostname résolu par Ojvind)
- Backend Symfony exposé via NodePort → `http://172.16.60.4:30080`
- Frontend React exposé via NodePort → `http://172.16.60.4:30081`
- Issues #1 à #7 clôturées et validées par le PO (Ianis)

---

## User Stories sélectionnées

| # Issue | User Story | Points | Assigné à |
|---------|-----------|--------|-----------|
| #8 | Configurer Ingress (Traefik) pour router le trafic | 5 | Ojvind |
| #9 | Tester et documenter l'accès externe | 2 | Zaid |
| #10 | Créer des Namespaces (dev, prod) | 2 | Ojvind |
| #11 | Configurer SSL/TLS pour sécuriser les accès | 5 | Ojvind |
| #12 | Utiliser ConfigMaps pour la configuration applicative | 3 | Zaid |
| #13 | Stocker les credentials dans des Secrets K8s | 3 | Ojvind |

## Estimation totale : 20 points

---

## Répartition des rôles

| Membre | Rôle | Issues |
|--------|------|--------|
| Ojvind | Dev Team — infra & sécurité | #8, #10, #11, #13 |
| Zaid | Dev Team — configuration & documentation | #9, #12 |
| Blaise | Scrum Master — cérémonies & livrables Scrum | — |
| Ianis | Product Owner — priorisation & validation Done | — |

---

## Ordre de réalisation recommandé

1. **#10** — Namespaces `dev` / `prod` (prérequis structurel pour toutes les autres ressources)
2. **#13** — Secrets K8s (prérequis pour #11 — stocker les clés TLS et credentials)
3. **#8** — Ingress Traefik (routage HTTP vers backend et frontend)
4. **#12** — ConfigMaps (déplacer les variables d'env en clair vers des ConfigMaps)
5. **#11** — SSL/TLS (sécurisation HTTPS via Traefik + certificat)
6. **#9** — Tests et documentation de l'accès externe

---

## Notes

- Traefik est inclus par défaut dans K3s : exploiter l'existant plutôt que d'en déployer un nouveau.
- Les certificats SSL seront auto-signés (Let's Encrypt non disponible sur réseau lab isolé 192.168.10.0/24).
- Toutes les ressources doivent être déployées dans les bons Namespaces — aucune ressource dans `default`.
- Les variables d'environnement en clair dans les manifestes Sprint 2 doivent être migrées vers ConfigMaps/Secrets.
