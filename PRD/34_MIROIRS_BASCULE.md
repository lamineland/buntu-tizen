# PRD 34 — Miroirs et bascule automatique

- **ID :** `PRD-34`
- **Prérequis :** `PRD-24`, `PRD-30`
- **Origine Buntu :** `crates/cnd-core/src/health/mirrors.rs`, `BUNTU-PRD-17`
- **Couches impactées :** `src/engine/mirrors/`, `src/player/failover/`
- **Statut :** `[ ]`
- **Objectif :** après l'échec des reconnexions, proposer ou lancer une chaîne équivalente.

---

## 1. Contexte & Objectif

La bascule est utile sur TV. La sonde périodique de Buntu ne l'est pas : pas de tâche de fond, et chaque sonde consomme une connexion de l'abonnement.

## 2. Responsabilité

**Fait :** trouver les équivalents, les ordonner, basculer.

**Ne fait pas :** détection de panne (`PRD-30`).

## 3. Spécifications

- Équivalence par `tvg_id` normalisé, sinon par jetons du nom, comme `mirrors.rs`.
- Qualité déduite du nom : 4K > FHD > HD > SD.
- Santé apprise pendant la lecture seulement : succès et échec récents, en mémoire de session ou dans un petit magasin borné.
- Bascule après l'échec des reconnexions ou 2 décrochages en moins de 60 s, 2 bascules par session au plus, bandeau non bloquant.
- Marquage manuel d'un flux comme cassé (`toggle_broken`) conservé.

`probe_stream`, `start_health_checks` et le worker périodique ne sont pas portés.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de requête vers un flux sans demande de lecture.
- `visibilityFilter` (`PRD-24`) s'applique aux miroirs.

## 5. Critères d'Acceptation

1. Cas de parité de `mirrors.rs`.
2. Bascule en lecture sur un jeu de deux flux dont l'un est coupé.

## 6. Sources

- Modules Buntu cités en en-tête.
