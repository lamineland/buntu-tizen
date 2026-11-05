# PRD 15 — Système visuel « 10-foot »

- **ID :** `PRD-15`
- **Prérequis :** `PRD-02`, `PRD-05`
- **Origine Buntu :** `docs/BRANDING_BUNTU.md` § 4, `BUNTU-PRD-30`, `BUNTU-PRD-21`
- **Couches impactées :** `src/ui/design/`
- **Statut :** `[ ]`
- **Objectif :** une interface lisible à 3 mètres, fidèle à la marque, en une seule grille de 1920×1080.

---

## 1. Contexte & Objectif

La résolution d'application est 1920×1080 sur tous les modèles `[DOC]`. Pas de mise en page adaptative : une grille fixe, avec des jetons.

## 2. Responsabilité

**Fait :** jetons de couleur, de type, d'espacement, zone sûre, états visuels, composants de base.

**Ne fait pas :** déplacement du focus (`PRD-14`), contenu des écrans.

## 3. Spécifications

### 3.1. Couleurs

Issues de la marque, avec leurs contrastes vérifiés :

| Jeton | Valeur | Usage |
| :--- | :--- | :--- |
| `bg` (Buntu Midnight) | `#0B0E14` | Fond |
| `surface` (Buntu Slate) | `#161B26` | Cartes |
| `accent` (Teranga Gold) | `#FF9F0A` | Action, « En direct », favori |
| `focus` (Leer Cyan) | `#00D2FF` | Halo de focus |
| `text` (Pure White) | `#F5F7FA` | Texte |
| `textMuted` (Muted Silver) | `#8E95A5` | Métadonnées |

Un texte sur bouton doré est en `#0B0E14`, jamais en blanc. L'état n'est jamais signalé par la couleur seule. Thème sombre seulement.

### 3.2. Type

SF Pro n'est pas disponible sur TV et sa licence le réserve aux plateformes Apple. Police proposée : la police système de la TV, ou une police libre à chasse variable embarquée si son poids tient dans le budget. Taille minimale du corps 28 px, métadonnées 24 px `[CIBLE]`. Réglage `normal` / `grand` (`PRD-10`).

### 3.3. Grille

Zone sûre de 48 px en haut et en bas et de 96 px sur les côtés `[CIBLE]`. Pas de 8 px. Affiches 2:3, vignettes 16:9, rangées à hauteur fixe pour que la virtualisation reste simple.

### 3.4. Focus

Halo cyan de 4 px et mise à l'échelle de 1,05 par `transform`. L'élément au focus est visible sans deviner : forme plus couleur. Animation de 120 ms au plus.

### 3.5. Composants

Bouton, carte, ligne de liste, rangée, onglet, fenêtre modale, bandeau de message, jauge de progression, indicateur de chargement, état vide, état d'erreur.

### 3.6. Icône et logo

Le « B » en deux arches. Les fichiers de boutique sont du ressort du `PRD-52`. Pas de dessin Apple réutilisé.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune couleur hors jetons. Un contrôle au build le vérifie.
- Rien d'interactif hors de la zone sûre.
- Pas de `box-shadow` ni de `filter` animés.

## 5. Critères d'Acceptation

1. Une page de démonstration affiche tous les composants et états, sur TV réelle.
2. Contrastes ≥ 4,5:1 pour tout texte.
3. Texte lisible à 3 mètres sur une TV de 43 pouces, essai humain consigné.

## 6. Sources

- [General Specifications](https://developer.samsung.com/smarttv/develop/specifications/general-specifications.html)
- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
