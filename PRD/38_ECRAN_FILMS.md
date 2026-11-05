# PRD 38 — Écran Films

- **ID :** `PRD-38`
- **Prérequis :** `PRD-18`, `PRD-24`, `PRD-26`, `PRD-27`, `PRD-32`
- **Origine Buntu :** `BUNTU-PRD-10`
- **Couches impactées :** `src/ui/screens/movies/`
- **Statut :** `[ ]`
- **Objectif :** parcourir 60 000 films en grille sans dépasser le budget d'images.

---

## 1. Contexte & Objectif

Une grille d'affiches est le premier risque mémoire de l'app.

## 2. Responsabilité

**Fait :** grille de films, fiche, reprise, favori.

**Ne fait pas :** lecture (`PRD-32`), téléchargement (abandonné).

## 3. Spécifications

- Rangées « Continuer », « Favoris », puis grille par catégorie.
- Grille virtualisée, affiches 2:3, repli sur un fond neutre et le titre.
- Tris : ordre de la source, nom, ajout récent, note.
- Fiche : affiche, titre, année, durée, résumé, note, Regarder, Reprendre, Favori. Enrichissement à l'ouverture (`PRD-27`).
- Aucune action « Télécharger ».
- Format probablement illisible signalé à la lecture (`PRD-29`), pas dans la grille.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune image pleine taille décodée dans une grille.
- Le chargement d'une affiche est annulé quand elle sort de l'écran.

## 5. Critères d'Acceptation

1. Défilement du catalogue de référence, mémoire stable.
2. Reprise au bon endroit (`VI-GP-05`).

## 6. Sources

- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
