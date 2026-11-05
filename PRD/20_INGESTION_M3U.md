# PRD 20 — Ingestion M3U en flux

- **ID :** `PRD-20`
- **Prérequis :** `PRD-06`, `PRD-07`, `PRD-09`, `PRD-19`
- **Origine Buntu :** `crates/cnd-core/src/iptv/m3u_parser.rs`, `db/ingestion_writer.rs`, `BUNTU-PRD-02`
- **Couches impactées :** `src/engine/ingest/m3u/`
- **Statut :** `[ ]`
- **Objectif `[CIBLE]` :** 100 000 entrées ingérées en moins de 3 minutes sur TV du bas de palier, mémoire stable.

---

## 1. Contexte & Objectif

Une playlist peut peser plus de 100 Mo. Buntu a été écrit contre l'erreur d'origine, qui était de la charger entière. La règle vaut encore plus sur TV. Le chiffre de 2 secondes pour 100 000 entrées ne s'applique pas ici.

## 2. Responsabilité

**Fait :** analyser un flux M3U ligne par ligne et émettre des lots d'éléments.

**Ne fait pas :** transport (`PRD-09`), classement (`PRD-19`), planification (`PRD-22`), écriture bas niveau (`PRD-06`).

## 3. Spécifications

Port de `m3u_parser.rs`, avec les mêmes directives :

- `#EXTM3U` : `x-tvg-url`, `url-tvg`, `catchup`, `catchup-source`.
- `#EXTINF` : `tvg-id`, `tvg-name`, `tvg-logo`, `logo`, `group-title`, `tvg-country`, `tvg-language`, `tvg-chno`, `catchup`, `catchup-source`, `catchup-days`, `timeshift`, `epg-shift`, indices IMDb, nom après la dernière virgule hors guillemets.
- `#EXTGRP`, `#EXTVLCOPT` (`http-user-agent`, `http-referrer`), `#KODIPROP`.
- Ligne URL : `host`, `container_ext`, identifiants retirés (`PRD-07`), hachage stable (`PRD-06`).

**Différences avec Buntu :**

- Analyseur à états dans le Worker, sans construire de tableau complet.
- `referer` et `headers_json` sont conservés : le `PRD-29` sait ainsi marquer un flux que la TV ne peut pas lire avec ses en-têtes.
- Le texte long (`plot`) n'est gardé que s'il est court. Le reste vient des métadonnées (`PRD-27`).
- Un temps de repos entre les lots si une lecture est en cours.
- Annulation et progression à chaque lot.

Le parseur reçoit un flux d'octets, quelle que soit son origine : réseau ici, clé USB par le point d'extension utilisé par le `PRD-43`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de `response.text()` sur une playlist.
- Encodage tolérant : une ligne invalide est ignorée et comptée, jamais fatale.
- Un élément au-delà du plafond du `PRD-06` n'est pas écrit, et le compte est rendu.

## 5. Critères d'Acceptation

1. Fixtures issues des tests de `m3u_parser.rs` : résultats identiques, sauf écarts listés.
2. Playlist de référence ingérée sur TV sans croissance mémoire.
3. Une coupure réseau au milieu conserve l'ancien catalogue.

## 6. Sources

- [Web App Flash Storage Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-flash-storage-optimization-guide.html)
- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
