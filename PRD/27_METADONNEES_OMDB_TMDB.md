# PRD 27 — Métadonnées OMDb et TMDB

- **ID :** `PRD-27`
- **Prérequis :** `PRD-06`, `PRD-07`, `PRD-09`, `PRD-24`
- **Origine Buntu :** `crates/cnd-core/src/metadata/mod.rs`, `BUNTU-PRD-06`, `BUNTU-PRD-28`
- **Couches impactées :** `src/engine/metadata/`
- **Statut :** `[ ]`
- **Objectif :** enrichir une fiche à son ouverture, sans jamais la retarder.

---

## 1. Contexte & Objectif

Buntu intègre des clés par défaut à son build. Dans un `.wgt`, une clé intégrée est lisible par tous : on s'en passe.

## 2. Responsabilité

**Fait :** recherche d'une fiche, cache, éviction.

**Ne fait pas :** saisie de la clé (`PRD-41`), affichage (`PRD-38`, `PRD-39`).

## 3. Spécifications

- Clés fournies par l'utilisateur, rangées dans le coffre (`PRD-07`). Sans clé, on affiche ce que donne la source.
- Extraction de titre et d'année identique à `metadata/mod.rs`.
- Enrichissement à l'ouverture d'une fiche seulement, jamais en masse.
- Cache `metadataCache` plafonné à 2 000 fiches `[CIBLE]`, LRU.
- Les serveurs OMDb et TMDB doivent accepter l'appel depuis le contexte de l'app : `S1` le confirme.
- Erreur ou délai : la fiche reste affichée, sans message bloquant.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune clé dans le paquet ni dans le dépôt.
- La clé est masquée dans les journaux (`key=`, `apikey=`).

## 5. Critères d'Acceptation

1. Cas de parité de `metadata/mod.rs`.
2. Une fiche s'affiche sans clé, et sans réseau si elle est en cache.

## 6. Sources

- [Application Security](https://developer.samsung.com/smarttv/develop/guides/fundamentals/application-security.html)
- [Web App Flash Storage Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-flash-storage-optimization-guide.html)
