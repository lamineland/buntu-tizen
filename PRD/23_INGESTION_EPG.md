# PRD 23 — Guide TV : ingestion et requêtes

- **ID :** `PRD-23`
- **Prérequis :** `PRD-06`, `PRD-09`, `PRD-22`
- **Origine Buntu :** `crates/cnd-core/src/iptv/epg.rs`, `db/epg_repo.rs`, `BUNTU-PRD-04`
- **Couches impactées :** `src/engine/epg/`
- **Statut :** `[ ]`
- **Objectif `[CIBLE]` :** Now/Next de 50 chaînes en moins de 100 ms, mémoire stable sur un XMLTV de 800 Mo décompressé.

---

## 1. Contexte & Objectif

Les fichiers XMLTV atteignent 100 à 800 Mo. Sur TV, tout stocker n'est ni utile ni raisonnable pour la flash.

## 2. Responsabilité

**Fait :** lecture du XMLTV, filtre, fenêtre, requêtes Now/Next et grille.

**Ne fait pas :** écran (`PRD-37`), calcul du replay (`PRD-33`).

## 3. Spécifications

### 3.1. Ingestion XMLTV

- Plusieurs URL séparées par des virgules, gzip détecté par les octets magiques (`PRD-09`).
- Analyseur XML en flux dans le Worker, sans DOM.
- Filtre précoce : seuls les `tvg_id` présents dans le catalogue sont gardés, comme `epg.rs`.
- Horodatage `YYYYMMDDHHMMSS +HHMM`, avec `epg_shift`.
- Champs : titre, sous-titre, catégorie. Description tronquée à 500 caractères `[CIBLE]`.

### 3.2. Fenêtre

Buntu garde 6 h de passé, voire 14 jours pour le replay. Sur TV : de −6 h à +36 h `[CIBLE]`, ajustable après mesure. Les programmes passés pour le replay se demandent à la chaîne, à la demande.

### 3.3. Xtream : à la demande

Nouveau par rapport à Buntu. Pour une source Xtream, `get_short_epg` et `get_simple_data_table` peuvent servir pour les chaînes visibles, avec un cache court, plutôt qu'un fichier géant. Ces appels ne sont pas utilisés aujourd'hui dans `xtream.rs` et leur format se valide sur de vrais serveurs. Stratégie : à la demande par défaut pour Xtream, XMLTV si l'utilisateur fournit une URL ou si la demande échoue. Débit limité pour ne pas saturer le serveur.

### 3.4. Requêtes

`getEpgNowNext(tvgIds)` en lots, `getChannelEpg(tvgId, from, to)`, purge par tranches. Le guide ne se rafraîchit pas plus d'une fois par 24 h, sauf action de l'utilisateur.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Le fichier XML n'est jamais chargé en entier.
- Un programme invalide est ignoré et compté.
- Le guide ne bloque jamais la lecture d'une chaîne.

## 5. Critères d'Acceptation

1. Fixtures de `epg.rs` et de `epg_repo.rs`, dont gzip sans extension et décalage horaire.
2. XMLTV de référence ingéré sur TV sans croissance mémoire.
3. Sans guide, Live TV reste utilisable.

## 6. Sources

- [Web App Flash Storage Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-flash-storage-optimization-guide.html)
