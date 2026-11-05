# PRD 36 — Écran Live TV

- **ID :** `PRD-36`
- **Prérequis :** `PRD-18`, `PRD-23`, `PRD-24`, `PRD-26`, `PRD-32`
- **Origine Buntu :** `BUNTU-PRD-09`, `BUNTU-PRD-21` § 2.3
- **Couches impactées :** `src/ui/screens/live/`
- **Statut :** `[ ]`
- **Objectif :** de l'ouverture de l'app à une chaîne en moins de 3 actions.

---

## 1. Contexte & Objectif

C'est l'écran principal du produit. Sur TV, on zappe en plein écran et on ne revient à la liste que pour chercher.

## 2. Responsabilité

**Fait :** catégories, liste des chaînes, programme en cours, accès aux favoris et récents.

**Ne fait pas :** commandes de lecture (`PRD-32`), guide complet (`PRD-37`).

## 3. Spécifications

- Trois colonnes : catégories, chaînes (liste virtualisée), aperçu de la chaîne au focus.
- Sections fixes : Récents, Favoris, puis catégories avec compteurs.
- Chaque ligne : logo, numéro, nom, programme en cours, progression.
- Now/Next chargés par lots pour les lignes visibles seulement, après une pause de 200 ms dans le défilement.
- OK : lecture plein écran. Appui long ou menu : favori, masquer, signaler comme cassé, voir le guide.
- Aperçu vidéo facultatif : désactivé par défaut, car il consomme une connexion et de la mémoire.
- Retour au plein écran depuis la liste, sur la chaîne en cours.
- Pré-tampon de la chaîne suivante, uniquement si `S11` le valide et si `max_connections` le permet.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun Now/Next demandé pendant un défilement rapide.
- Logos chargés à la demande et de taille bornée (`PRD-12`).

## 5. Critères d'Acceptation

1. Défilement de 10 000 chaînes dans le budget sur la TV la plus faible.
2. Zapping dans une catégorie de 200 chaînes sans perte de focus.

## 6. Sources

- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
- [AVPlayStore API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplaystore-api.html)
