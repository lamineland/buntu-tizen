# PRD 11 — Cycle de vie de l'application

- **ID :** `PRD-11`
- **Prérequis :** `PRD-02`, `PRD-05`
- **Origine Buntu :** sans équivalent direct
- **Couches impactées :** `src/platform/lifecycle/`
- **Statut :** `[ ]`
- **Objectif :** respecter sans exception les règles de sortie, de multitâche et de perte de réseau, car ces points bloquent la certification.

---

## 1. Contexte & Objectif

Une app TV peut être cachée à tout moment. Son JavaScript est alors suspendu `[DOC]`. Le multitâche, la touche Retour et l'alerte de perte de réseau font partie des fonctions obligatoires `[DOC]`.

## 2. Responsabilité

**Fait :** démarrage, passage en arrière-plan, reprise, sortie, état du réseau, écran de veille.

**Ne fait pas :** pile de navigation (`PRD-18`), reprise du lecteur (`PRD-28`), reconnexion (`PRD-30`).

## 3. Spécifications

### 3.1. Sortie

- Touche Retour sur l'écran d'accueil : fenêtre de confirmation en HTML. L'app ne se ferme que sur « Oui » `[DOC]`, par `tizen.application.getCurrentApplication().exit()`.
- Appui long sur Retour/Exit : géré par le système, jamais enregistré par l'app `[DOC]`.
- Exception : arrivée par lien profond, Retour quitte sans confirmation `[DOC]`. Sans objet tant que Smart Hub Preview n'est pas implémenté.

### 3.2. Passage en arrière-plan

Sur `visibilitychange` :

- `document.hidden === true` : enregistrer position et état, arrêter minuteries et boucles d'animation, annuler le trafic non essentiel, signaler au lecteur. Si une vidéo joue, appliquer le comportement du Retour pendant la lecture `[DOC]`.
- `document.hidden === false` : vérifier le réseau par `webapis.network.isConnectedToGateway()`, vérifier que les sessions sont encore valides, puis reprendre. Une expiration n'est jamais calculée à partir de l'horloge locale `[DOC]`.

Cet événement se déclenche aussi à la sortie `[DOC]` : l'enregistrement doit être court et idempotent. Une synchronisation en cours s'arrête proprement et reprend au retour (`PRD-22`).

### 3.3. Perte de réseau

Une alerte s'affiche dès la perte de connexion `[DOC]`, avec l'action suivante. Le travail réseau s'arrête. Au retour du réseau, l'alerte se ferme et l'app reprend sans redémarrer.

### 3.4. Écran de veille

`webapis.appcommon.setScreenSaver(SCREEN_SAVER_OFF)` pendant la lecture, et `SCREEN_SAVER_ON` à la pause, à l'arrêt et en arrière-plan `[DOC]`. Le test `VI-GP-08` demande que la lecture reprenne après l'écran de veille pendant une pause `[DOC]`.

### 3.5. Métadonnées

`prelaunch.support` n'est pas activé en v1 : il impose un comportement de lancement différé à tester à part.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de fermeture directe sur Retour depuis l'accueil.
- Aucune minuterie active quand l'app est cachée.
- Pas d'hypothèse de travail en arrière-plan.

## 5. Critères d'Acceptation

1. Les tests du checklist Common et Video sur la sortie, le multitâche et le réseau passent sur TV réelle.
2. Dix cycles cacher et reprendre pendant une lecture ne laissent ni fuite ni écran noir.
3. La perte de réseau est signalée en moins de 5 secondes `[CIBLE]`.

## 6. Sources

- [Terminating Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/terminating-applications.html)
- [Multitasking](https://developer.samsung.com/smarttv/develop/guides/fundamentals/multitasking.html)
- [Mandatory Features](https://developer.samsung.com/smarttv/develop/development-checklist/mandatory-features.html)
- [Video checklist](https://developer.samsung.com/smarttv/develop/development-checklist/video.html)
- [AppCommon API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/appcommon-api.html)
- [Network API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/network-api.html)
