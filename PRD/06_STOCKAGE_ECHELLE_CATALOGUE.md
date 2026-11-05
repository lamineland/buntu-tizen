# PRD 06 — Stockage et échelle du catalogue

- **ID :** `PRD-06`
- **Prérequis :** `PRD-03`, `PRD-05`
- **Origine Buntu :** `crates/cnd-core/src/db/schema.rs`, `db/ingestion_writer.rs`, `BUNTU-PRD-02`, `BUNTU-PRD-05`
- **Couches impactées :** `src/engine/store/`
- **Statut :** `[ ]`
- **Objectif `[CIBLE]` :** 100 000 éléments par TV, page de 50 éléments en moins de 100 ms, à confirmer par `S8`.

---

## 1. Contexte & Objectif

C'est la décision d'architecture centrale. Sur Apple, SQLite WAL et FTS5 servent 150 000 éléments en moins de 5 ms. La TV n'a ni Rust ni SQLite, sa mémoire est partagée et sa limite n'est pas publiée `[NON DOC]`, et sa flash est « limited and shared » `[DOC]`.

## 2. Responsabilité

**Fait :** choix du moteur de stockage, schéma, écriture par lots, migrations, quota et purge.

**Ne fait pas :** requêtes métier (`PRD-24`), index de recherche (`PRD-25`), secrets (`PRD-07`).

## 3. Spécifications

### 3.1. Options

| Option | Pour | Contre | Verdict |
| :--- | :--- | :--- | :---: |
| **A. TypeScript + IndexedDB dans un Worker** | Persistant, API standard, couvert par `unlimitedstorage`, écriture incrémentale | Réécriture, requêtes moins riches, pas de recherche native, vitesse à mesurer | **Recommandée** |
| B. `cnd-core` en WASM + SQLite WASM | Réemploi de la logique | `tokio`, `reqwest` et `rusqlite` à remplacer ; pas d'OPFS documenté, donc base en mémoire et sauvegardes complètes sur la flash ; tas WASM non documenté ; SDK Samsung de 2021 | Rejetée, `S14` peut la rouvrir |
| C. Serveur compagnon | TV légère, base réelle | Contraire au local d'abord ; identifiants hors de la TV ; coût ; exposition juridique | Rejetée |
| D. Rien de persistant | Simple | Lancement de plus de 10 secondes, pas de recherche globale | Rejetée |
| E. Web SQL | SQL | Technologie abandonnée par Chromium, sa version par année n'est pas lisible dans le tableau Samsung | Rejetée |
| F. Fichiers maison dans `wgt-private` | Contrôle total | Index et transactions à écrire, API filesystem dépendante de l'année | Repli si `S8` échoue |

**Recommandation : A, avec chargement à la demande pour Xtream (§ 3.4).** Le WASM reste possible plus tard pour une fonction isolée et mesurée, pas pour le socle.

### 3.2. Schéma

Une base `buntu`, versionnée. Les enregistrements sont compacts : clés courtes, pas de champ vide, pas de copie du texte de recherche.

| Magasin | Clé | Index | Origine Buntu |
| :--- | :--- | :--- | :--- |
| `sources` | `uuid` | | `iptv_sources` sans secret |
| `items` | `id` | `[kind, group, sort]`, `[sourceId, generation]`, `tvgId`, `hash` | `iptv_items` |
| `series` | `id` | `[sourceId, group, sort]` | `series_catalog` |
| `groups` | `[sourceId, kind, name]` | | facettes précalculées |
| `epg` | `[tvgId, start]` | `end` | `epg_programs` |
| `searchTokens` | voir `PRD-25` | | `iptv_items_fts` |
| `favorites`, `history` | `[profileId, hash]` | `lastWatchedAt` | `favorites`, `watch_history` |
| `profiles`, `hidden`, `order`, `folders` | | | `BUNTU-PRD-20` |
| `metadataCache` | `lookupKey` | `fetchedAt` | `media_metadata_cache` |
| `settings` | `key` | | `app_settings` |

Sont exclus : `download_*`, `recordings`, `sync_tombstones`, `library_*`, `stream_health` en tâche de fond.

### 3.3. Identifiant stable

`stable_hash = blake3("{source_uuid}:{stream_url}")` protège favoris et historique. Sur TV, pour une source Xtream, la clé part de `{source_uuid}:{kind}:{stream_id}`, car l'URL ne contient plus les identifiants (`PRD-07`). La fonction de hachage est choisie à la mesure : BLAKE3 par un petit module WASM, ou un hachage 64 bits non cryptographique. Pas de synchronisation avec Apple en v1 : l'égalité avec Buntu n'est pas requise.

### 3.4. Catalogue à la demande

- **Xtream :** les catégories sont chargées à la synchronisation, les listes complètes sont indexées dans le budget, et les épisodes d'une série ne se chargent qu'à son ouverture (déjà le cas dans Buntu).
- **Si le catalogue dépasse le plafond :** on garde le direct en priorité, puis les films et les séries jusqu'à la limite. L'interface annonce clairement la partie non indexée et l'accès reste possible par catégorie.

### 3.5. Écriture sans coupure

Comme `ingestion_writer.rs` : lots transactionnels (2 000 éléments au départ, à ajuster), écriture d'une génération `N+1`, bascule atomique du pointeur, purge de la génération `N` par petites tranches. Une interruption laisse la génération `N` intacte. Favoris et historique ne sont jamais touchés.

### 3.6. Quota et usure

- Lire `navigator.storage.estimate()` si disponible et déclarer `unlimitedstorage`.
- Pas de rafraîchissement automatique plus d'une fois par 24 h (`PRD-22`).
- Comparer avant d'écrire : un élément inchangé n'est pas réécrit.
- Cache de métadonnées et d'images plafonné, éviction LRU, bouton « Vider le cache » (`PRD-41`).
- Le `localStorage` ne contient que de petits réglages de démarrage : limite de 5 Mo `[DOC]`.

### 3.7. Migrations et désinstallation

Migrations numérotées et testées. En cas d'échec, l'app conserve la source et reconstruit le catalogue. Toutes les données sont dans IndexedDB ou `wgt-private`, jamais dans `documents`, qui survit à la désinstallation `[DOC]`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun catalogue complet en mémoire, ni dans le Worker ni dans l'interface.
- Aucun secret dans IndexedDB.
- Toute lecture de liste passe par un curseur borné.

## 5. Critères d'Acceptation

1. Ingestion de 100 000 éléments de test sur TV du bas de palier, chiffres consignés.
2. Coupure de courant simulée pendant une synchronisation : ancien catalogue intact.
3. Mise à jour de l'app : données conservées.
4. Désinstallation et réinstallation : aucune donnée de l'ancienne installation.

## 6. Sources

- [Web App Flash Storage Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-flash-storage-optimization-guide.html)
- [Using Web Storage](https://developer.samsung.com/smarttv/develop/guides/data-handling/using-web-storage.html)
- [File Operations](https://developer.samsung.com/smarttv/develop/guides/data-handling/managing-file-operations.html)
- [Configuring TV Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/configuring-tv-applications.html)
- [WebAssembly](https://developer.samsung.com/smarttv/develop/extension-libraries/webassembly/webassembly.html), [Samsung Emscripten SDK](https://developer.samsung.com/smarttv/develop/extension-libraries/webassembly/download.html)
- [Web Engine Specifications](https://developer.samsung.com/smarttv/develop/specifications/web-engine-specifications.html)
