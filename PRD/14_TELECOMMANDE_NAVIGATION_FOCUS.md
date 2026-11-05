# PRD 14 — Télécommande et navigation au focus

- **ID :** `PRD-14`
- **Prérequis :** `PRD-05`, `PRD-11`
- **Origine Buntu :** `BUNTU-PRD-21` § 2.3
- **Couches impactées :** `src/ui/focus/`, `src/platform/input/`
- **Statut :** `[ ]`
- **Objectif :** tout écran est utilisable avec six touches : quatre flèches, OK, Retour.

---

## 1. Contexte & Objectif

Sur TV, pas de souris ni de geste. Le moteur de focus décide de presque toute la qualité perçue. Il n'existe pas d'équivalent au moteur de focus de tvOS : il faut l'écrire.

## 2. Responsabilité

**Fait :** enregistrement des touches, traduction en commandes, déplacement du focus, mémoire du focus.

**Ne fait pas :** aspect du focus (`PRD-15`), pile d'écrans (`PRD-18`), saisie (`PRD-16`).

## 3. Spécifications

### 3.1. Touches

Reconnues sans enregistrement : flèches, Enter, Back `[DOC]`. Les autres passent par `tizen.tvinputdevice.registerKeyBatch` avec le privilège `tv.inputdevice` `[DOC]`.

| Touche | Code | Commande |
| :--- | :---: | :--- |
| ArrowLeft / Up / Right / Down | 37 / 38 / 39 / 40 | Déplacer le focus |
| Enter | 13 | Valider |
| Back | 10009 | Retour |
| ChannelUp / ChannelDown | 427 / 428 | Chaîne suivante / précédente |
| MediaPlayPause | 10252 | Lecture / pause |
| MediaPlay / MediaPause / MediaStop | 415 / 19 / 413 | Commandes de lecture |
| MediaRewind / MediaFastForward | 412 / 417 | Recul / avance |
| 0 à 9 | 48 à 57 | Saisie d'un numéro de chaîne |
| ColorF0Red / Green / Yellow / Blue | 403 / 404 / 405 / 406 | Raccourcis facultatifs |
| Info, Guide, ChannelList | 457, 458, 10073 | Bandeau, guide, liste |

Enregistrer uniquement les touches utilisées, avec `getSupportedKeys()` en contrôle. Les touches de volume restent au système. Les touches Exit et l'appui long sur Retour ne sont pas enregistrés `[DOC]`.

La Smart Remote n'a que les touches essentielles, les autres passent par un clavier à l'écran `[DOC]`. **Aucune fonction ne dépend d'une touche de couleur ni d'un chiffre.** Les raccourcis ont toujours un équivalent à l'écran.

### 3.2. Moteur de focus

- Modèle par zones : une zone (grille, liste, rangée, barre, fenêtre) gère ses déplacements et passe au voisin à ses bords.
- Déplacement déclaratif plutôt que géométrique, pour rester rapide. Le calcul spatial reste un repli.
- Mémoire par zone : revenir sur une zone rend le dernier élément.
- Retour sur un écran : même élément, même défilement.
- Fenêtre modale : le focus y reste enfermé.
- Répétition : un appui maintenu accélère le défilement. Les événements en excès sont abandonnés pour éviter le retard.
- Un seul écouteur `keydown` global.

### 3.3. Pointeur

Certaines TV ont un pointeur. `pointing-device-support` est désactivé en v1 `[DOC]` : un seul mode d'interaction à tester.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas d'écran inatteignable, pas de piège de focus, toujours exactement un élément au focus.
- `keydown` ne fait jamais de lecture de stockage ni de réseau de façon synchrone.

## 5. Critères d'Acceptation

1. Parcours complet avec les six touches sur TV réelle, sans souris.
2. Déplacement sous le budget du `PRD-12` dans une grille de 10 000 éléments.
3. Test automatique : chaque écran a un focus initial et chaque élément interactif est atteignable.

## 6. Sources

- [Remote Control](https://developer.samsung.com/smarttv/develop/guides/user-interaction/remote-control.html)
- [Terminating Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/terminating-applications.html)
- [Configuring TV Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/configuring-tv-applications.html)
