# PRD 24 — Requêtes de catalogue

- **ID :** `PRD-24`
- **Prérequis :** `PRD-06`, `PRD-19`
- **Origine Buntu :** `crates/cnd-core/src/db/catalog_repo.rs`, `BUNTU-PRD-05`
- **Couches impactées :** `src/engine/catalog/`
- **Statut :** `[ ]`
- **Objectif :** toute liste se pagine sans charger le catalogue.

---

## 1. Contexte & Objectif

Buntu a corrigé des défauts où un favori au rang 500 n'apparaissait jamais, parce que le tri se faisait sur une page en mémoire. Le même piège existe avec IndexedDB : un tri absent des index force à tout lire.

## 2. Responsabilité

**Fait :** catégories avec compteurs, listes paginées, tris, séries, saisons, épisodes.

**Ne fait pas :** texte libre (`PRD-25`), favoris et historique (`PRD-26`).

## 3. Spécifications

- `getGroups(kind, sourceId)` lit les compteurs précalculés à l'ingestion.
- `queryCatalog({ kind, sourceId, group, sort, cursor, limit })` utilise un **curseur par clé**, pas `offset` : aller à la page 100 ne doit pas parcourir les 99 premières.
- Tris proposés : ordre de la source, nom, numéro de chaîne, ajout récent, note. Chaque tri a son index. Un tri sans index n'est pas proposé.
- `getSeriesSeasons`, `getSeriesEpisodes`, `getItemById`.
- Point d'extension `visibilityFilter` appliqué ici, dans le moteur et jamais dans l'interface. Ses règles sont définies par le `PRD-45`.
- Filtre multi-sources : fusion de curseurs triés.

`get_hosts` n'est pas porté : le filtrage par hôte n'apparaît pas dans l'interface TV.

## 4. Contraintes Non-Négociables (Guardrails IA)

- `limit` ≤ 200.
- Aucune opération `getAll()` sur `items`.

## 5. Critères d'Acceptation

1. Page 1 et page 500 dans le budget sur le catalogue de référence.
2. Un élément exclu par `visibilityFilter` ne fuit ni dans les compteurs ni dans les listes.
3. Parité de résultat avec `catalog_repo.rs` sur les jeux de test.

## 6. Sources

- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
