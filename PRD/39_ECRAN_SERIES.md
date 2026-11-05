# PRD 39 — Écran Séries

- **ID :** `PRD-39`
- **Prérequis :** `PRD-18`, `PRD-21`, `PRD-24`, `PRD-26`, `PRD-27`, `PRD-32`
- **Origine Buntu :** `BUNTU-PRD-10`
- **Couches impactées :** `src/ui/screens/series/`
- **Statut :** `[ ]`
- **Objectif :** de la série à l'épisode, avec lecture enchaînée de la saison.

---

## 1. Contexte & Objectif

Une série demande une navigation à plusieurs niveaux, et ses épisodes Xtream ne sont chargés qu'à l'ouverture.

## 2. Responsabilité

**Fait :** grille de séries, fiche, saisons, épisodes, lecture de la saison.

**Ne fait pas :** chargement Xtream (`PRD-21`).

## 3. Spécifications

- Grille virtualisée comme les films (`PRD-38`).
- Fiche : affiche, résumé, saisons en onglets, épisodes en liste, progression par épisode.
- Bouton « Reprendre » sur le prochain épisode non terminé.
- Chargement Xtream à l'ouverture : état de chargement et erreur avec « Réessayer ».
- Lecture d'une saison entière : épisode suivant à la fin, avec le temps d'annuler.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de préchargement des épisodes de toutes les séries.

## 5. Critères d'Acceptation

1. Série de 20 saisons fluide.
2. Reprise au bon épisode après avoir quitté l'app.

## 6. Sources

- [Video checklist](https://developer.samsung.com/smarttv/develop/development-checklist/video.html)
