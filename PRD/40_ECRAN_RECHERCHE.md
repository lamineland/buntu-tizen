# PRD 40 — Écran Recherche

- **ID :** `PRD-40`
- **Prérequis :** `PRD-16`, `PRD-18`, `PRD-25`
- **Origine Buntu :** `BUNTU-PRD-05`, `apple/Sources/CndIptvKit/Shared/`
- **Couches impactées :** `src/ui/screens/search/`
- **Statut :** `[ ]`
- **Objectif :** un résultat utile après 3 lettres.

---

## 1. Contexte & Objectif

Taper à la télécommande est lent : la recherche doit répondre dès les premières lettres.

## 2. Responsabilité

**Fait :** saisie, résultats groupés, navigation vers un élément.

**Ne fait pas :** index (`PRD-25`).

## 3. Spécifications

- Résultats groupés : Chaînes, Films, Séries.
- Déclenchement après 2 lettres, avec une attente de 250 ms après la frappe.
- Le champ reste accessible, et Bas quitte le clavier vers les résultats.
- Dernières recherches conservées par profil, effaçables.
- Aucun résultat : message et rappel des sources couvertes. Si l'index est partiel, l'écran le dit.
- La saisie vocale n'est pas prévue en v1 (voir `PRD-46`).

## 4. Contraintes Non-Négociables (Guardrails IA)

- La recherche d'un profil Enfants reste limitée à son périmètre.

## 5. Critères d'Acceptation

1. Résultat sous le budget sur la TV la plus faible.
2. Un résultat ouvre la bonne fiche ou la bonne chaîne.

## 6. Sources

- [Keyboard / IME](https://developer.samsung.com/smarttv/develop/guides/user-interaction/keyboardime.html)
