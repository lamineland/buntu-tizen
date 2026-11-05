# PRD 31 — Pistes audio et sous-titres intégrés

- **ID :** `PRD-31`
- **Prérequis :** `PRD-03`, `PRD-28`
- **Origine Buntu :** `BUNTU-PRD-08` § 3.4
- **Couches impactées :** `src/player/tracks/`
- **Statut :** `[ ]`
- **Objectif :** choisir l'audio et les sous-titres disponibles, et garder les sous-titres synchronisés.

---

## 1. Contexte & Objectif

`libmpv` affiche ASS, SRT et PGS. AVPlay gère SAMI, SMPTE-TT et DFXP, et ses fichiers externes doivent être locaux `[DOC]`. Le rendu des sous-titres doit donc être pensé autrement.

## 2. Responsabilité

**Fait :** liste des pistes, choix, langue préférée, affichage des sous-titres envoyés par AVPlay.

**Ne fait pas :** recherche en ligne (`PRD-48`), menu du lecteur (`PRD-32`).

## 3. Spécifications

- `getTotalTrackInfo()` à l'état `READY` ou plus. `setSelectTrack('AUDIO', i)` et `setSelectTrack('TEXT', i)` `[DOC]`. `TEXT` n'est pas disponible en DASH, et `AUDIO` l'est à partir de Tizen 5.0 `[DOC, résumé]`.
- Si la langue préférée existe, elle est choisie à l'ouverture.
- Piste DTS : indiquée comme non lisible et jamais sélectionnée automatiquement (`PRD-29`).
- `onsubtitlechange` reçoit le texte. `setSilentSubtitle(true)` coupe le rendu natif et l'app affiche le texte dans une couche HTML si `S6` le permet. Taille et fond suivent la zone sûre du `PRD-15`.
- `setSubtitlePosition(ms)` sert au décalage quand AVPlay rend lui-même les sous-titres.
- Synchronisation pendant pause et reprise : test `VI-SY-03` `[DOC, résumé]`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de texte de sous-titre dans les journaux.
- Une piste absente n'affiche pas un menu vide.

## 5. Critères d'Acceptation

1. Changement de piste audio et de sous-titres pendant la lecture, sur MP4, MKV et HLS.
2. Sous-titres synchronisés après pause et saut.

## 6. Sources

- [Subtitles](https://developer.samsung.com/smarttv/develop/guides/multimedia/subtitles.html)
- [AVPlay API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplay-api.html)
- [Video checklist](https://developer.samsung.com/smarttv/develop/development-checklist/video.html)
