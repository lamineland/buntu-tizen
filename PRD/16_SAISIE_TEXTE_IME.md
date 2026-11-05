# PRD 16 — Saisie de texte à la télécommande

- **ID :** `PRD-16`
- **Prérequis :** `PRD-14`
- **Origine Buntu :** `BUNTU-PRD-21` § 2.3
- **Couches impactées :** `src/ui/forms/`
- **Statut :** `[ ]`
- **Objectif :** une URL de serveur et des identifiants se saisissent sans perte ni piège de focus.

---

## 1. Contexte & Objectif

Saisir une URL, un utilisateur et un mot de passe à la télécommande est le moment le plus pénible du produit. Il n'y a pas ici de clavier iPhone ni d'iCloud pour l'éviter.

## 2. Responsabilité

**Fait :** champs de saisie, ouverture et fermeture du clavier, validation de champ.

**Ne fait pas :** logique des formulaires (`PRD-35`), recherche (`PRD-40`).

## 3. Spécifications

- Le clavier virtuel s'ouvre quand un `input` a le focus `[DOC]`. Un clavier physique le masque `[DOC]`.
- Disposition QWERTY pour email et mot de passe, ABC à partir de 2022 `[DOC]`. Les champs utilisent le bon `type` : `url`, `password`, `search`, `number`.
- Fermeture : Done, Go, Search, Back, ou Haut en bord de clavier selon l'année `[DOC]`.
- Codes Done 65376 et Cancel 65385, autres touches IME 229, à confirmer sur TV : ils viennent d'un résumé de la page `[DOC, résumé]`. `preventDefault()` n'est pas appelé sur ces `keydown`.
- Le focus du moteur `PRD-14` est suspendu pendant la saisie et rendu au même champ.
- Pas d'autocorrection ni de majuscule automatique sur les URL et identifiants.
- Mot de passe : masqué par défaut, avec une action « Afficher ».
- Valeurs de saisie jamais journalisées.
- Une erreur de format reste à côté du champ, avec la correction attendue.
- Le format court `hôte:port` est complété par `http://` à la validation, pour limiter la frappe.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de clavier maison en v1 : le clavier système reste le chemin à tester.
- Back ferme d'abord le clavier, pas l'écran.

## 5. Critères d'Acceptation

1. Une URL de 80 caractères, un utilisateur et un mot de passe se saisissent sur une TV 2021 et une TV 2024.
2. Back, Done et le passage au champ suivant ne perdent ni le texte ni le focus.

## 6. Sources

- [Keyboard / IME](https://developer.samsung.com/smarttv/develop/guides/user-interaction/keyboardime.html)
