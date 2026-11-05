# PRD 41 — Écrans Réglages et console des journaux

- **ID :** `PRD-41`
- **Prérequis :** `PRD-07`, `PRD-08`, `PRD-10`, `PRD-16`, `PRD-18`
- **Origine Buntu :** `BUNTU-PRD-11`
- **Couches impactées :** `src/ui/screens/settings/`
- **Statut :** `[ ]`
- **Objectif :** tous les réglages et le diagnostic sont accessibles à la télécommande.

---

## 1. Contexte & Objectif

Les réglages regroupent les choix de lecture, les clés d'API et la console. La console aide l'utilisateur à comprendre un flux refusé, et aide à déboguer sur TV.

## 2. Responsabilité

**Fait :** écrans de réglages, de clés, d'à-propos, console.

**Ne fait pas :** stockage (`PRD-10`, `PRD-07`), journaux (`PRD-08`).

## 3. Spécifications

- **Lecture :** format de direct préféré, User-Agent, langue audio et sous-titres.
- **Affichage :** langue, taille du texte.
- **Clés :** OMDb, TMDB, fournisseur IA. Valeur masquée, action « Remplacer ».
- **Données :** vider le cache, espace utilisé si disponible, réinitialiser l'app après confirmation (efface base et coffre).
- **Console :** derniers journaux masqués, filtre par catégorie, pause.
- **À propos :** version, modèle, Tizen, texte « Buntu est un lecteur et ne fournit aucun contenu », licences, politique de confidentialité.

`webapis.productinfo` donne le modèle. Le DUID n'est ni lu ni affiché.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun secret à l'écran, y compris dans la console.
- Profil Enfants : pas d'accès aux réglages (`PRD-45`).

## 5. Critères d'Acceptation

1. Chaque réglage prend effet, sans redémarrage sauf mention.
2. « Réinitialiser » rend l'app à son état de premier lancement.

## 6. Sources

- [ProductInfo API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/productinfo-api.html)
- [Web App Flash Storage Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-flash-storage-optimization-guide.html)
