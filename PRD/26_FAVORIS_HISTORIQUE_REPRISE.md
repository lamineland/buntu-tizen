# PRD 26 — Favoris, historique et reprise

- **ID :** `PRD-26`
- **Prérequis :** `PRD-06`, `PRD-24`
- **Origine Buntu :** `db/catalog_repo.rs` (`toggle_favorite`, `record_watch_progress`, `list_recent_items`), `BUNTU-PRD-05`
- **Couches impactées :** `src/engine/library/`
- **Statut :** `[ ]`
- **Objectif :** ces données survivent à chaque rafraîchissement de source.

---

## 1. Contexte & Objectif

La reprise est aussi une exigence de test : revenir sur une vidéo interrompue reprend là où elle s'était arrêtée `[DOC]`. Elle remplace le téléchargement comme réponse aux coupures.

## 2. Responsabilité

**Fait :** favoris, récents, position de lecture, état « terminé ».

**Ne fait pas :** dossiers de favoris (`PRD-45`), rangées d'écran.

## 3. Spécifications

- Clé `[profileId, stableHash]`, indépendante de la pagination, comme dans Buntu.
- Position enregistrée toutes les 30 s `[CIBLE]`, à la pause, à l'arrêt et au passage en arrière-plan. Pas à chaque `oncurrentplaytime`, pour ménager la flash.
- Terminé à 92 % de la durée, comme `record_watch_progress`.
- Récents limités à 100 par profil et par type.
- Un favori dont l'élément a disparu reste listé comme indisponible.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Un rafraîchissement ne supprime rien ici.
- L'écriture de position ne bloque pas la lecture.

## 5. Critères d'Acceptation

1. Test `VI-GP-05` réussi sur TV réelle.
2. Favoris conservés après une nouvelle génération du catalogue.
3. Séparation stricte entre profils.

## 6. Sources

- [Video checklist](https://developer.samsung.com/smarttv/develop/development-checklist/video.html)
