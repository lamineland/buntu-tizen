# PRD 17 — Accessibilité

- **ID :** `PRD-17`
- **Prérequis :** `PRD-14`, `PRD-15`
- **Origine Buntu :** `BUNTU-PRD-31`, `docs/BRANDING_BUNTU.md` § 2 (Teraanga)
- **Couches impactées :** `src/ui/`, `src/platform/a11y/`, `config.xml`
- **Statut :** `[ ]`
- **Objectif :** navigation complète avec le guide vocal de la TV, et dossier d'accessibilité prêt pour l'UE.

---

## 1. Contexte & Objectif

L'accessibilité n'est pas une option de confort : une déclaration de conformité à l'European Accessibility Act est demandée pour une publication dans l'UE, et les groupes de modèles FCC exigent sous-titres et synthèse vocale `[DOC]`.

## 2. Responsabilité

**Fait :** libellés, annonces, contraste, sous-titres visibles, dossier de conformité.

**Ne fait pas :** réglage des pistes (`PRD-31`), dépôt de la déclaration (`PRD-52`).

## 3. Spécifications

- Déclarer `http://samsung.com/tv/metadata/use.voiceguide` `[DOC]`. Sa mise en œuvre exacte se vérifie sur TV.
- Chaque élément focalisable a un nom accessible : rôle, nom, état (« France 2, 4 sur 200, favori »).
- Les changements d'état importants sont annoncés (« Lecture », « Reconnexion »).
- Pas d'information par la couleur seule, pas de texte dans une image.
- Taille de texte `grand` (`PRD-15`).
- Délais des bandeaux réglables ou à fermeture manuelle pour tout message important.
- Sous-titres : si le flux en a, ils sont accessibles en deux actions depuis le lecteur.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Une fonction n'est pas terminée si elle ne passe pas au guide vocal.
- On ne déclare pas une conformité sans essai avec des personnes concernées ou un audit.

## 5. Critères d'Acceptation

1. Parcours Ajouter une source > Lancer une chaîne au guide vocal, sur TV réelle.
2. Audit de contraste automatique sans erreur.
3. Modèle de déclaration EAA rempli et revu avant le dépôt.

## 6. Sources

- [Configuring TV Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/configuring-tv-applications.html)
- [Entering Application Information](https://developer.samsung.com/tv-seller-office/guides/applications/entering-application-information.html)
- [Distributing Applications](https://developer.samsung.com/tv-seller-office/guides/applications/distributing-application.html)
