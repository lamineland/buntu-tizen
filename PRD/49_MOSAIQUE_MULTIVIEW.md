# PRD 49 — Mosaïque (conditionnelle)

- **ID :** `PRD-49`
- **Prérequis :** `PRD-03`, `PRD-28`, `PRD-36`
- **Origine Buntu :** `BUNTU-PRD-16`
- **Couches impactées :** `src/player/multiview/`, `src/ui/screens/multiview/`
- **Statut :** `[ ]` conditionnel à `S11`
- **Objectif :** deux directs à l'écran sur les TV qui le permettent, sinon rien.

---

## 1. Contexte & Objectif

Buntu affiche jusqu'à 4 flux, décodés par `libmpv` et `AVPlayer`. Sur TV, `avplaystore` permet jusqu'à 4 instances, mais sa page ne parle que de pré-tampon `[DOC]`. `IN_APP_MULTIVIEW`, à partir de Tizen 7.0, choisit un scaler principal ou secondaire `[DOC]`. Aucune page n'annonce deux lectures simultanées `[NON DOC]`.

## 2. Responsabilité

**Fait :** disposition 1+1, son exclusif, détection de capacité.

**Ne fait pas :** 2×2 et 1+3 sans preuve sur TV.

## 3. Spécifications

- Proposé si `S11` réussit, sur Tizen 7.0 ou plus, et si `max_connections ≥ 2`.
- Deux instances AVPlay, `VIDEO_VIEW_RESOURCE_MAIN` et `VIDEO_VIEW_RESOURCE_SUB`.
- Son exclusif sur le flux au focus.
- Sortie vers le plein écran sur un flux.
- `onresourceconflicted` provoque un retour propre à un seul flux.

Si `S11` échoue, ce PRD est abandonné et retiré de la matrice du `PRD-01`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- La limite de connexions n'est jamais dépassée.
- Pas de mosaïque par une vidéo décodée en logiciel.

## 5. Critères d'Acceptation

1. 30 minutes de double lecture sans plantage sur chaque modèle annoncé.
2. La fonction est invisible sur une TV non compatible.

## 6. Sources

- [AVPlayStore API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplaystore-api.html)
- [AVPlay API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplay-api.html)
