# PRD 37 — Écran Guide TV

- **ID :** `PRD-37`
- **Prérequis :** `PRD-18`, `PRD-23`, `PRD-33`
- **Origine Buntu :** `BUNTU-PRD-09`, `BUNTU-PRD-15`
- **Couches impactées :** `src/ui/screens/guide/`
- **Statut :** `[ ]`
- **Objectif :** une grille horaire fluide, case par case, à la télécommande.

---

## 1. Contexte & Objectif

La grille de Buntu n'a jamais été adaptée au focus sur Apple TV. Sur TV Samsung, elle est à concevoir pour la télécommande dès le départ.

## 2. Responsabilité

**Fait :** grille, fiche d'un programme, action Regarder ou Replay.

**Ne fait pas :** données (`PRD-23`), URL de replay (`PRD-33`).

## 3. Spécifications

- Virtualisation en deux dimensions : seules les lignes et les heures visibles existent dans le DOM.
- Colonne de chaînes fixe, repère de l'heure courante.
- Gauche/Droite : programme précédent ou suivant. Haut/Bas : chaîne, en gardant l'heure.
- Fenêtre affichée égale à la fenêtre stockée (`PRD-23`). La grille ne promet pas plus.
- Fiche : titre, horaire, description, Regarder (en cours), Revoir (si replay).
- Sans données : la chaîne reste listée, avec « Pas de programme disponible ».
- Filtre par catégorie, pour réduire la grille.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de rendu complet de la grille.
- Le guide ne déclenche pas de téléchargement du XMLTV à son ouverture sans cache vide ni action de l'utilisateur.

## 5. Critères d'Acceptation

1. Navigation dans 2 000 chaînes × 42 h sans saccade sur la TV la plus faible.
2. Ouverture d'un replay depuis la grille.

## 6. Sources

- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
