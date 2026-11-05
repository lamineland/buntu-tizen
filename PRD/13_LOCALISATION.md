# PRD 13 — Localisation

- **ID :** `PRD-13`
- **Prérequis :** `PRD-05`
- **Origine Buntu :** `docs/BRANDING_BUNTU.md` § 7.1
- **Couches impactées :** `src/ui/i18n/`
- **Statut :** `[ ]`
- **Objectif :** aucun texte visible en dur ; français et anglais dès la v1.

---

## 1. Contexte & Objectif

Buntu écrit d'abord en français. La fiche Seller Office exige une langue par défaut, et un Public Seller ne vend qu'aux États-Unis `[DOC]` : l'anglais est obligatoire dès la v1.

## 2. Responsabilité

**Fait :** dictionnaires, choix de la langue, formats de date et de nombre.

**Ne fait pas :** textes de boutique (`PRD-52`).

## 3. Spécifications

- Dictionnaires JSON par langue, clés stables, repli sur l'anglais pour une clé absente.
- Langue initiale : celle de la TV, sinon `fr`, modifiable (`PRD-10`).
- `Intl` n'est utilisé qu'après contrôle sur le palier le plus ancien. Sinon, un petit formateur interne pour les dates et les heures.
- Dates du guide en heure locale de la TV.
- Le ton suit la marque : pas de jargon, un message d'erreur dit quoi faire.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune chaîne visible en dur : un test le vérifie.
- Les slogans en wolof ne sont pas utilisés avant leur validation (`docs/BRANDING_BUNTU.md` § 3.2).

## 5. Critères d'Acceptation

1. Toute l'interface s'affiche en français et en anglais.
2. Aucun texte ne déborde dans la langue la plus longue.

## 6. Sources

- [Entering Application Information](https://developer.samsung.com/tv-seller-office/guides/applications/entering-application-information.html)
