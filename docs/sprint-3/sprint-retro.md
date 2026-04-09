# Sprint 3 - Rétrospective

**Date** : 8 avril 2026
**Facilitateur** : Blaise (Scrum Master)
**Participants** : Blaise, Ojvind, Zaid, Ianis

---

## Keep (On continue)

- La structuration en Namespaces dès le début du sprint a rendu le travail plus lisible et les erreurs plus faciles à isoler.
- La communication entre Ojvind et Zaid pour coordonner les changements de manifestes (ConfigMaps impactent backend et frontend) a bien fonctionné.
- Rédiger la documentation au fur et à mesure (Zaid sur #9) plutôt qu'en fin de sprint évite le rush final.
- L'ordre de réalisation défini au planning (Namespaces → Secrets → Ingress → SSL) était logique et a été respecté.

## Drop (On arrête)

- Commencer la configuration SSL sans avoir vérifié en amont les contraintes réseau (besoin DNS public, port 80 joignable depuis Internet pour ACME/Let's Encrypt). On a perdu du temps avant de basculer sur le certificat auto-signé.
- Laisser des variables d'environnement en clair dans les manifestes d'un sprint à l'autre — ça crée une dette technique immédiate.

## Try (On essaie)

- En début de sprint, faire un rapide audit des contraintes réseau et sécurité avant de choisir l'approche technique (ex : "Let's Encrypt est-il utilisable dans ce réseau ?").
- Générer et valider le certificat auto-signé dès le premier jour du sprint pour ne pas bloquer la config SSL en fin de sprint.
- Pour le Sprint 4, prévoir un point de synchronisation à mi-sprint (pas seulement les dailys) pour détecter les blocages plus tôt.

---

## Actions concrètes

| Action | Responsable | Deadline |
|--------|------------|----------|
| Documenter la limitation Let's Encrypt en réseau lab dans le README | Zaid | Avant le Sprint 4 |
| Vérifier les contraintes réseau du Sprint 4 (HPA, monitoring) avant le planning | Ojvind | Sprint 4 Planning |
| Mettre à jour le board GitHub : fermer #8, #9, #10, #12, #13 — marquer #11 partiel | Ianis | 8 avril 2026 |
| S'assurer que tous les manifestes k8s/ sont poussés et à jour | Ojvind + Zaid | 8 avril 2026 |
