# PRD 10 — Réglages persistants

- **ID :** `PRD-10`
- **Prérequis :** `PRD-05`, `PRD-06`
- **Origine Buntu :** `crates/cnd-core/src/db/settings_repo.rs`, `BUNTU-PRD-01`
- **Couches impactées :** `src/engine/settings/`
- **Statut :** `[ ]`
- **Objectif :** des réglages typés, relus avant le premier rendu.

---

## 1. Contexte & Objectif

`RuntimeSettings` contient surtout des réglages de téléchargement qui n'ont plus de sens. La version TV garde un jeu réduit et en ajoute quelques-uns.

## 2. Responsabilité

**Fait :** modèle, valeurs par défaut, validation, lecture, écriture, notification.

**Ne fait pas :** écran (`PRD-41`), secrets (`PRD-07`).

## 3. Spécifications

| Réglage | Défaut | Origine |
| :--- | :--- | :--- |
| `defaultUserAgent` | `VLC/3.0.21 LibVLC/3.0.21` | Buntu, appliqué à AVPlay |
| `connectTimeoutMs`, `receiveTimeoutMs` | 10 000, 30 000 | Buntu |
| `showNetworkConsole` | `false` | Buntu |
| `liveStreamPreference` | `hls` | Nouveau (`PRD-29`) |
| `autoRefreshHours` | 24, minimum 24 | Par source, dans Buntu |
| `locale` | Langue de la TV, sinon `fr` | Nouveau |
| `textSize` | `normal` | Nouveau (`PRD-15`) |
| `adultContentVisible` | `false` | Buntu |
| `activeProfileId` | 1 | Buntu |

`segments_per_file`, `part_size_bytes`, `max_active_files`, `download_dir` et `speed_smoothing` sont retirés.

Une copie minimale (langue, taille du texte, profil actif) reste dans `localStorage` pour le premier rendu sans attendre IndexedDB. Écritures groupées, pour limiter les accès à la flash.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Une valeur invalide ou inconnue revient au défaut, sans plantage.
- Aucun secret dans les réglages.

## 5. Critères d'Acceptation

1. Les réglages survivent à un redémarrage et à une mise à jour.
2. Une valeur corrompue n'empêche pas le lancement.

## 6. Sources

- [Using Web Storage](https://developer.samsung.com/smarttv/develop/guides/data-handling/using-web-storage.html)
