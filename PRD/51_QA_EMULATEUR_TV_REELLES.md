# PRD 51 — QA sur émulateur et TV réelles

- **ID :** `PRD-51`
- **Prérequis :** `PRD-03`, `PRD-12`, `PRD-50`
- **Origine Buntu :** `BUNTU-PRD-00` § 2.5, `task check`
- **Couches impactées :** `tests/`, `docs/qa/`
- **Statut :** `[ ]`
- **Objectif :** avant chaque soumission, toutes les checklists Samsung sont passées sur de vraies TV.

---

## 1. Contexte & Objectif

L'émulateur et le navigateur valident la logique et l'interface, pas la lecture ni les performances. Les défauts `A` bloquent la publication `[DOC]`.

## 2. Responsabilité

**Fait :** stratégie de test, matrice de TV, procédure de recette, critères de version.

**Ne fait pas :** écrire les tests propres à chaque PRD.

## 3. Spécifications

### 3.1. Niveaux

| Niveau | Outil | Vérifie | Ne vérifie pas |
| :--- | :--- | :--- | :--- |
| Unitaire et parité | Node | Moteur, normalisation, fixtures Buntu | Plateforme |
| Interface | Chrome, adaptateurs simulés | Focus, écrans, messages | AVPlay, mémoire réelle |
| Simulateur TV | TV Simulator | API web simples | Pas de lecture fiable, pas d'app hébergée `[DOC]` |
| Émulateur | QEMU, machine x86 | Clés, multitâche, USB simulé | Performances, codecs réels ; `MediaTrackPrevious/Next` indisponibles `[DOC]` |
| Remote Test Lab | TV réelle à distance | Vérification ponctuelle | Liste de modèles et limites non confirmées ; ne remplace pas une TV en propre |
| **TV réelles** | Mode développeur | **Tout, et seules à faire foi** | |

### 3.2. Matrice minimale

| TV | Rôle |
| :--- | :--- |
| 2021 ou 2022, entrée de gamme, FHD ou UHD | Plancher de performance, TV la plus faible |
| 2023 ou 2024 | Milieu |
| 2025 ou 2026 | Plus récent, DTS et codecs retirés |
| 2020 | Seulement si le palier Étendu est retenu |

Plus une TV de chaque type de télécommande : Smart Remote et télécommande à chiffres. Chaque TV à jour de son firmware `[DOC]`.

### 3.3. Recette de version

1. Checklists Samsung [Common](https://developer.samsung.com/smarttv/develop/development-checklist/common.html), [Video](https://developer.samsung.com/smarttv/develop/development-checklist/video.html), [Account](https://developer.samsung.com/smarttv/develop/development-checklist/account.html), [Mandatory Features](https://developer.samsung.com/smarttv/develop/development-checklist/mandatory-features.html), un résultat par ligne et par TV.
2. Matrice format × TV du `PRD-29`.
3. Budgets du `PRD-12`, dont le lancement.
4. Soak : 8 h de direct, 2 h de navigation, 100 cycles cacher et reprendre.
5. Réseau : coupure, Wi-Fi faible, DNS en panne, serveur lent.
6. Données : mise à jour par-dessus, désinstallation et réinstallation sans résidu.
7. Conformité : aucune catégorie adulte sans PIN, aucune source dans le paquet.

### 3.4. Bêta

Le bêta-test Seller Office va jusqu'à 90 jours, extensible à 180, avec jusqu'à 1 000 codes d'activation, sur des TV 2021 ou plus récentes, après accord du Content Manager `[DOC, résumé]`. L'alpha est réservée aux partenaires : 30 jours, 50 DUID, TV 2020 ou plus récentes `[DOC, résumé]`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun PRD de lecture n'est terminé sans essai sur TV réelle.
- Tests automatiques sans réseau extérieur ni identifiant réel.
- Un test sauté se déclare dans le rapport.

## 5. Critères d'Acceptation

1. Rapport de recette archivé pour chaque version soumise.
2. Zéro défaut de niveau `A` connu à la soumission.

## 6. Sources

- [TV Emulator](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-emulator.html)
- [TV Simulator](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-simulator.html)
- [TV Device](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html)
- [Remote Test Lab](https://developer.samsung.com/remote-test-lab)
- [Remote Control](https://developer.samsung.com/smarttv/develop/guides/user-interaction/remote-control.html)
- [Distributing Applications](https://developer.samsung.com/tv-seller-office/guides/applications/distributing-application.html)
- [Certification Defects Q&A](https://developer.samsung.com/tv-seller-office/faq/certification-defects.html)
