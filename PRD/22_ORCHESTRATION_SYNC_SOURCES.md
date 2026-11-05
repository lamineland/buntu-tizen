# PRD 22 — Gestion et synchronisation des sources

- **ID :** `PRD-22`
- **Prérequis :** `PRD-07`, `PRD-10`, `PRD-11`, `PRD-20`, `PRD-21`
- **Origine Buntu :** `crates/cnd-core/src/lib.rs` (`add_source`, `refresh_source`, `delete_source`), `BUNTU-PRD-09`
- **Couches impactées :** `src/engine/sources/`
- **Statut :** `[ ]`
- **Objectif :** créer, modifier, supprimer et rafraîchir une source sans jamais perdre le catalogue courant.

---

## 1. Contexte & Objectif

Sans service de fond, la synchronisation n'a lieu que lorsque l'app est ouverte et visible.

## 2. Responsabilité

**Fait :** cycle de vie d'une source, file de synchronisation, planification, progression.

**Ne fait pas :** analyse (`PRD-20`, `PRD-21`), guide (`PRD-23`), formulaires (`PRD-35`).

## 3. Spécifications

- **File :** une source à la fois. Les demandes en double sont fusionnées.
- **Planification :** au lancement, si la dernière synchronisation date de plus de `autoRefreshHours`, et après la première page. Jamais pendant le zapping, ni à moins de 24 h d'intervalle sauf action de l'utilisateur.
- **Interruption :** si l'app est cachée, le travail s'arrête, la génération partielle reste non publiée et la synchronisation reprend au retour (`PRD-11`).
- **Suppression :** secret, catalogue et guide sont retirés. Favoris orphelins conservés 30 jours `[CIBLE]`, puis purgés.
- **Compte Xtream expiré :** l'erreur le dit et le catalogue reste consultable.
- **État visible :** en attente, en cours (progression), à jour, erreur et motif.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de synchronisation automatique sans source active, ni en boucle après un échec : attente progressive, et 3 essais par lancement.
- Pas de synchronisation automatique pendant une lecture sur la même source si `max_connections` est 1.

## 5. Critères d'Acceptation

1. Cacher l'app au milieu d'une synchronisation ne corrompt rien.
2. Deux demandes simultanées ne lancent qu'un travail.
3. Une source supprimée ne laisse ni secret ni élément.

## 6. Sources

- [Multitasking](https://developer.samsung.com/smarttv/develop/guides/fundamentals/multitasking.html)
- [Registering Applications](https://developer.samsung.com/tv-seller-office/guides/applications/registering-application.html)
