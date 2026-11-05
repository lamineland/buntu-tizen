# PRD 35 — Écrans de gestion des sources

- **ID :** `PRD-35`
- **Prérequis :** `PRD-16`, `PRD-18`, `PRD-22`
- **Origine Buntu :** `BUNTU-PRD-09`, `apple/Sources/CndIptvKit/Features/Sources/`
- **Couches impactées :** `src/ui/screens/sources/`
- **Statut :** `[ ]`
- **Objectif :** ajouter une source en moins de 3 minutes à la télécommande `[CIBLE]`.

---

## 1. Contexte & Objectif

C'est le seul chemin vers le contenu, et il doit passer la certification : un testeur doit pouvoir atteindre toutes les fonctions `[DOC]`.

## 2. Responsabilité

**Fait :** liste des sources, formulaires, test du compte, état de synchronisation, erreurs.

**Ne fait pas :** logique de synchronisation (`PRD-22`), import d'un partage (`PRD-43`).

## 3. Spécifications

- **Liste :** nom, type, dernière synchronisation, nombre d'éléments, état, expiration du compte.
- **Ajouter M3U :** nom, URL, URL de guide facultative.
- **Ajouter Xtream :** nom, serveur, utilisateur, mot de passe, avec **test du compte** avant enregistrement.
- **Actions :** modifier, synchroniser, supprimer après confirmation.
- **Vide :** explication « Buntu lit vos sources » et bouton « Ajouter une source ».
- **Non chiffré :** une source `http://` porte un avertissement discret (`PRD-07`).
- **Erreurs :** identifiants incorrects, compte expiré, serveur injoignable, serveur qui refuse la TV, format non reconnu, chacune avec une action.

Le test `AC-UL-02` (erreur sur nom ou mot de passe incorrect) et le test `AC-UL-03` s'appliquent si Samsung traite une source comme une connexion `[DOC]`. L'App UI Description l'explique (`PRD-52`).

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune suggestion ni aucun exemple de source réelle.
- Le mot de passe n'est jamais réaffiché après enregistrement.

## 5. Critères d'Acceptation

1. Ajout, modification et suppression à la télécommande, sur TV réelle.
2. Chaque erreur simulée produit son message.

## 6. Sources

- [Account checklist](https://developer.samsung.com/smarttv/develop/development-checklist/account.html)
- [Application UI Description](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/application-ui-description.html)
