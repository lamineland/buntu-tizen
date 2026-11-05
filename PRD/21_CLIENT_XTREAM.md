# PRD 21 — Client Xtream Codes

- **ID :** `PRD-21`
- **Prérequis :** `PRD-06`, `PRD-07`, `PRD-09`, `PRD-19`, `PRD-20`
- **Origine Buntu :** `crates/cnd-core/src/iptv/xtream.rs`, `BUNTU-PRD-03`
- **Couches impactées :** `src/engine/ingest/xtream/`
- **Statut :** `[ ]`
- **Objectif :** tester un compte, importer son catalogue et charger une série à la demande.

---

## 1. Contexte & Objectif

`player_api.php` fournit des listes structurées, bien plus légères qu'un M3U complet. Les réponses JSON peuvent cependant atteindre des dizaines de Mo, et le corps entier ne doit pas être gardé en mémoire.

## 2. Responsabilité

**Fait :** dialogue avec `player_api.php`, informations de compte, catégories, flux, séries, repli M3U+.

**Ne fait pas :** stockage du mot de passe (`PRD-07`), guide (`PRD-23`), URL de replay (`PRD-33`), écran (`PRD-35`).

## 3. Spécifications

### 3.1. Appels

Comme `xtream.rs` : authentification, `get_live_categories`, `get_vod_categories`, `get_series_categories`, `get_live_streams`, `get_vod_streams`, `get_series`, `get_series_info`. Repli automatique sur `get.php?type=m3u_plus` si l'API répond 403, 404 ou un JSON invalide.

### 3.2. Adaptations

- **JSON en flux :** analyse incrémentale du tableau, élément par élément, dans le Worker. Si le coût est trop élevé, appels par `category_id` pour découper le travail.
- **Séquentiel :** Buntu lance les appels en parallèle. Sur TV, 2 au plus (`PRD-09`), pour ménager mémoire et serveur.
- **URL :** `stream_id`, `kind`, `ext` seulement (`PRD-07`).
- **Désérialisation tolérante :** `rating`, `stream_id`, `backdrop_path` peuvent changer de type, comme dans les cas de test de Buntu.
- **Séries :** `get_series_info` à l'ouverture seulement, avec un cache de 24 h. Jamais de boucle sur toutes les séries, pour ne pas risquer un bannissement.

### 3.3. Compte

Conserver `status`, `exp_date`, `max_connections`, `active_cons`, `timezone`, `time_now`. `max_connections` borne la lecture (`PRD-28`, `PRD-49`). L'heure du serveur sert au replay (`PRD-33`).

## 4. Contraintes Non-Négociables (Guardrails IA)

- Le mot de passe n'apparaît jamais en base ni en clair dans les journaux.
- Une erreur d'identifiants se distingue d'une erreur réseau.
- Un champ mal typé n'interrompt jamais l'import.

## 5. Critères d'Acceptation

1. Les cas de `xtream.rs`, dont `get_series_info` en objet et en tableau vide, passent en fixtures.
2. Un faux serveur local valide authentification, repli et erreurs.
3. Un compte de démonstration s'importe sur TV réelle.

## 6. Sources

- Module Buntu cité en en-tête. Xtream n'est pas un protocole Samsung.
- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
