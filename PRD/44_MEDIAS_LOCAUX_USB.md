# PRD 44 — Médias locaux sur clé USB

- **ID :** `PRD-44`
- **Prérequis :** `PRD-18`, `PRD-28`, `PRD-42`
- **Origine Buntu :** `BUNTU-PRD-32`, `crates/cnd-core/src/library/`
- **Couches impactées :** `src/ui/screens/files/`
- **Statut :** `[ ]`
- **Objectif :** lire vidéos, musique et photos d'une clé USB, si `S15` réussit.

---

## 1. Contexte & Objectif

Sur Apple, « Mes fichiers » indexe des dossiers autorisés et extrait les métadonnées. Sur TV, la version utile est bien plus simple : parcourir la clé et lire. Un index persistant userait la flash pour un support qui peut disparaître.

## 2. Responsabilité

**Fait :** explorateur, lecture audio et vidéo, visionneuse d'images.

**Ne fait pas :** accès bas niveau (`PRD-42`).

## 3. Spécifications

- Explorateur par dossier, avec un filtre Vidéo, Audio, Images. Type déduit de l'extension, selon la liste de `classify.rs`.
- Vidéo et audio par AVPlay : chemin absolu local `[DOC]`.
- Images : `<img>` à la taille de l'écran, une image décodée à la fois, 8,3 Mo en FHD `[DOC]`.
- Sous-titres voisins : `.smi` par AVPlay, `.srt` par la couche du `PRD-48` si elle est livrée.
- Reprise : clé issue du chemin relatif, de la taille et de la date du fichier.

Non portés : index, recherche, albums, artistes, miniatures, pochettes, listes de lecture, partages réseau (SMB, DLNA), mémoire interne de la TV.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun fichier copié sur la TV.
- Pas d'accès en dehors des supports `removable_`.

## 5. Critères d'Acceptation

1. Lecture d'une vidéo, d'un MP3 et d'une photo sur TV réelle.
2. Retrait de la clé pendant une lecture : message, retour sans plantage.

## 6. Sources

- [Using AVPlay](https://developer.samsung.com/smarttv/develop/guides/multimedia/media-playback/using-avplay.html)
- [Subtitles](https://developer.samsung.com/smarttv/develop/guides/multimedia/subtitles.html)
- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
