# PRD 48 — Sous-titres en ligne

- **ID :** `PRD-48`
- **Prérequis :** `PRD-03`, `PRD-07`, `PRD-09`, `PRD-31`, `PRD-32`
- **Origine Buntu :** `crates/cnd-core/src/subtitles/`, `BUNTU-PRD-18`
- **Couches impactées :** `src/engine/subtitles/`, `src/ui/player/`
- **Statut :** `[ ]`
- **Objectif :** trouver un sous-titre OpenSubtitles et l'afficher synchronisé, si `S6` réussit.

---

## 1. Contexte & Objectif

AVPlay n'accepte pas le SRT comme format documenté et ne lit que des fichiers externes locaux `[DOC]`. Buntu a déjà une surcouche de sous-titres : c'est aussi la bonne approche sur TV.

## 2. Responsabilité

**Fait :** recherche, téléchargement, analyse SRT, affichage, décalage.

**Ne fait pas :** pistes intégrées (`PRD-31`).

## 3. Spécifications

- Recherche par IMDb, par titre, ou par `moviehash` si le serveur accepte les requêtes `Range`. Port de `subtitles/hash.rs`.
- Connexion et clé fournies par l'utilisateur, dans le coffre (`PRD-07`).
- Port de `srt.rs`. Les repères restent en mémoire, jamais sur la flash.
- Affichage HTML cadencé par `oncurrentplaytime`. Recherche dichotomique du repère courant.
- Décalage par pas de 0,25 s, comme dans Buntu.
- Option de repli : convertir en SAMI dans `wgt-private-tmp`, pour le rendu d'AVPlay, si la couche HTML échoue.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Sous-titres synchronisés, exigence Samsung `[DOC]`.
- Aucun envoi d'information sans action de l'utilisateur.

## 5. Critères d'Acceptation

1. Cas de test de `subtitles/` en fixtures de parité.
2. Synchronisation correcte après pause et saut sur TV réelle.

## 6. Sources

- [Subtitles](https://developer.samsung.com/smarttv/develop/guides/multimedia/subtitles.html)
- [Mandatory Features](https://developer.samsung.com/smarttv/develop/development-checklist/mandatory-features.html)
