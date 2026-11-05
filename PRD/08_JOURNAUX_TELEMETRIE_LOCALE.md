# PRD 08 — Journaux et masquage des secrets

- **ID :** `PRD-08`
- **Prérequis :** `PRD-05`
- **Origine Buntu :** `crates/cnd-core/src/telemetry/mod.rs`, `BUNTU-PRD-01`
- **Couches impactées :** `src/engine/telemetry/`
- **Statut :** `[ ]`
- **Objectif :** un anneau de journaux en mémoire, sans secret, sans envoi extérieur.

---

## 1. Contexte & Objectif

La console de Buntu explique les échecs de lecture et de réseau. Sur TV, c'est encore plus utile : l'équipe de certification ne fournit pas de journaux de plateforme, mais une version qui produit des journaux console est acceptée `[DOC]`.

## 2. Responsabilité

**Fait :** collecter, borner, masquer et lire les journaux.

**Ne fait pas :** écran de console (`PRD-41`).

## 3. Spécifications

- Anneau en mémoire de 1 000 entrées `[CIBLE]`, identifiant croissant, niveau, catégorie, message, URL masquée. Lecture par `getLogsSince(afterId, limit)`, comme Buntu.
- Rien n'est écrit sur la flash par défaut.
- Port direct de `redact_url` et `redact_text` : `user:pass@`, chemins `/live/{u}/{p}/`, `/movie/`, `/series/`, `/timeshift/`, paramètres sensibles, `Authorization`, `x-api-key`, `x-goog-api-key`, `key=`.
- Le masquage se fait **à l'entrée** de l'anneau, avant toute copie.
- Catégories : `net`, `sync`, `player`, `store`, `app`.
- Pas d'analytique, pas d'envoi de crash, pas de SDK tiers.

## 4. Contraintes Non-Négociables (Guardrails IA)

- `console.log` n'est jamais appelé avec une valeur non masquée.
- Un journal ne contient ni le DUID ni le nom de l'utilisateur.

## 5. Critères d'Acceptation

1. Les cas de `telemetry/mod.rs` passent en fixtures de parité.
2. L'anneau reste borné après 100 000 entrées.

## 6. Sources

- [Certification Defects Q&A](https://developer.samsung.com/tv-seller-office/faq/certification-defects.html)
