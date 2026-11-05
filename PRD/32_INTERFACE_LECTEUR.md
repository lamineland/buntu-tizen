# PRD 32 — Interface du lecteur

- **ID :** `PRD-32`
- **Prérequis :** `PRD-14`, `PRD-15`, `PRD-18`, `PRD-28`, `PRD-30`, `PRD-31`
- **Origine Buntu :** `BUNTU-PRD-08`, `BUNTU-PRD-21` § 2.3
- **Couches impactées :** `src/ui/player/`
- **Statut :** `[ ]`
- **Objectif :** toutes les commandes de lecture exigées par la checklist Video, à la télécommande.

---

## 1. Contexte & Objectif

Les fonctions de lecture sont testées une à une par Samsung `[DOC]`. Sur TV, le lecteur est aussi le mode principal d'usage : plein écran, sans pointeur.

## 2. Responsabilité

**Fait :** bandeau, commandes, indicateur de chargement, mini-lecteur, menu des pistes.

**Ne fait pas :** logique du moteur, liste des chaînes (`PRD-36`).

## 3. Spécifications

### 3.1. Direct

- ChannelUp/Down et Haut/Bas : chaîne suivante ou précédente, en gardant la catégorie courante.
- OK ou Info : bandeau avec chaîne, Now/Next, progression.
- Chiffres : numéro de chaîne, s'ils existent sur la télécommande.
- Retour : ferme le bandeau, puis quitte le plein écran.

### 3.2. VOD

- Lecture/pause, ±10 s avec Gauche/Droite ou Rewind/FastForward, barre de progression, reprise.
- `VI-PC-05` : la touche pause ne ferme jamais la vidéo `[DOC, résumé]`.
- `VI-PC-03` : des sauts répétés restent fluides `[DOC, résumé]` : ils sont cumulés et envoyés une fois.
- Fin d'un épisode : carte « Épisode suivant » (`PRD-47`, si livré).

### 3.3. Commun

- Indicateur de chargement qui disparaît au début de lecture `[DOC]`.
- Menu pistes audio et sous-titres.
- Bandeau de reconnexion non bloquant (« Reconnexion 1/3… »).
- Masquage automatique après 5 s, sauf si un menu est ouvert.

### 3.4. Mini-lecteur

Remplace l'image dans l'image. Dans une liste de chaînes ou une fiche, la vidéo se place dans une zone par `setDisplayRect`. Il reste dans l'app, et s'arrête si l'app est cachée.

### 3.5. Si le HTML ne passe pas au-dessus de la vidéo

Si `S6` échoue, le bandeau réduit la zone vidéo avec `setDisplayRect` et s'affiche à côté, au lieu de la recouvrir.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de mise à jour du DOM à chaque image : progression à 1 Hz.
- Une commande de lecture reste possible pendant le chargement.

## 5. Critères d'Acceptation

1. Tests `VI-GP` et `VI-PC` du checklist Video réussis sur TV réelle.
2. Écran de veille désactivé pendant la lecture (`PRD-11`).

## 6. Sources

- [Video checklist](https://developer.samsung.com/smarttv/develop/development-checklist/video.html)
- [Remote Control](https://developer.samsung.com/smarttv/develop/guides/user-interaction/remote-control.html)
- [AVPlay API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplay-api.html)
