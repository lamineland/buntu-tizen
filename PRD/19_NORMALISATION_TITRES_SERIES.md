# PRD 19 — Normalisation des titres et des séries

- **ID :** `PRD-19`
- **Prérequis :** `PRD-05`
- **Origine Buntu :** `crates/cnd-core/src/iptv/normalizer.rs`, `BUNTU-PRD-02` § 3.2
- **Couches impactées :** `src/engine/normalize/`
- **Statut :** `[ ]`
- **Objectif :** mêmes résultats que `normalizer.rs` sur ses cas de test.

---

## 1. Contexte & Objectif

La détection `live` / `movie` / `episode` et le regroupement en séries sont le fruit de corrections successives dans Buntu. Il faut les reprendre tels quels.

## 2. Responsabilité

**Fait :** fonctions pures qui classent un élément, nettoient un titre, extraient année, saison et épisode, calculent la clé d'une série.

**Ne fait pas :** lecture réseau, écriture en base.

## 3. Spécifications

Port de `normalizer.rs` :

- `episode` si l'URL contient `/series/`, si le groupe contient `series`, `série` ou `serie`, ou si le nom correspond à `SxxExx`, `1x02` ou `Season n … Episode m`.
- `movie` si l'URL contient `/movie/`, si le groupe contient `vod`, `movie` ou `film`, ou si l'extension est `.mkv`, `.mp4`, `.avi`.
- `live` sinon.
- Retrait des préfixes de pays et de qualité (`|FR|`, `[FR]`, `FHD`, `4K`, `HEVC`, `VOSTFR`, `MULTI`…).
- `canonical_key` : minuscule, espaces simples, sans diacritiques ni étiquette de langue.

Expressions régulières compilées une seule fois. Vérifier les propriétés Unicode et les assertions arrière sur le moteur le plus ancien : une expression qui passe dans Chrome récent peut être refusée par M69 ou M76.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Fonctions pures, sans dépendance à la plateforme.
- Aucun changement de règle sans mise à jour des fixtures et de l'écart documenté.

## 5. Critères d'Acceptation

1. Tous les cas de test de `normalizer.rs` passent en fixtures de parité.
2. 100 000 titres traités sans allocation durable et sous le budget sur TV.

## 6. Sources

- Module Buntu cité en en-tête. Pas de dépendance à la documentation Tizen.
