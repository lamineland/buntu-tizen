# PRD 25 — Moteur de recherche plein texte

- **ID :** `PRD-25`
- **Prérequis :** `PRD-06`, `PRD-19`, `PRD-24`
- **Origine Buntu :** table `iptv_items_fts` de `schema.rs`, `BUNTU-PRD-05`
- **Couches impactées :** `src/engine/search/`
- **Statut :** `[ ]`
- **Objectif `[CIBLE]` :** résultat en moins de 300 ms sur 100 000 éléments, sans tout charger.

---

## 1. Contexte & Objectif

FTS5 cherche en préfixe, sans accents (`unicode61 remove_diacritics 2`), dans toutes les catégories. IndexedDB n'a pas de recherche de texte : il faut un index maison, et il pèse sur la flash.

## 2. Responsabilité

**Fait :** tokenisation, index, requête, classement.

**Ne fait pas :** écran (`PRD-40`).

## 3. Spécifications

- **Tokenisation :** minuscule, sans diacritiques, découpage sur les non-alphanumériques, retrait des mots de qualité (`HD`, `FHD`, `4K`…), identique à `channel_name_tokens` quand c'est possible.
- **Index :** magasin `searchTokens` de clé `[préfixe, itemId]`, sur les préfixes de 2 à 4 caractères de chaque mot. La vérification finale se fait sur le nom de l'élément.
- **Champs indexés :** nom seulement. `group_title` et `tvg_name` ne sont pas indexés, contrairement à FTS5, pour réduire la taille. Les chercher en repli est décidé après mesure.
- **Requête multi-mots :** intersection à partir du mot le plus rare, plafonnée à 200 résultats.
- **Classement :** correspondance exacte, puis préfixe, puis direct avant VOD, puis favoris.
- **Écriture :** dans le même cycle de génération que le catalogue (`PRD-06`).

**Plan B :** si l'index est trop gros (`S8`), index limité au direct et aux séries, avec les films cherchés par catégorie. Cet écart serait annoncé dans l'interface.

## 4. Contraintes Non-Négociables (Guardrails IA)

- La recherche applique `visibilityFilter` (`PRD-24`).
- Une frappe annule la requête précédente.

## 5. Critères d'Acceptation

1. « france » trouve « France 2 », « FRANCE 3 HD » et « Frânce 24 ».
2. La taille de l'index est mesurée et consignée.
3. Les cas de recherche de `catalog_repo.rs` donnent les mêmes éléments, sauf écarts listés.

## 6. Sources

- [Web App Flash Storage Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-flash-storage-optimization-guide.html)
