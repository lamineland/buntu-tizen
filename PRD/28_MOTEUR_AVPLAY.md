# PRD 28 — Moteur de lecture AVPlay

- **ID :** `PRD-28`
- **Prérequis :** `PRD-03`, `PRD-05`, `PRD-08`, `PRD-11`
- **Origine Buntu :** `BUNTU-PRD-08`, `apple/Sources/CndIptvKit/Player/`
- **Couches impactées :** `src/player/engine/`, `src/platform/avplay/`
- **Statut :** `[ ]`
- **Objectif :** un seul automate de lecture, qui ne déclenche jamais de méthode AVPlay dans un état interdit.

---

## 1. Contexte & Objectif

AVPlay remplace `AVPlayer` et `libmpv`. C'est un objet unique, avec une machine d'états stricte, et la vidéo ne passe pas par le DOM classique. Une mauvaise séquence provoque des erreurs ou des écrans noirs.

## 2. Responsabilité

**Fait :** enrober AVPlay derrière `PlayerPort`, garantir l'ordre des appels, exposer des événements typés.

**Ne fait pas :** choix du flux (`PRD-29`), reconnexion (`PRD-30`), pistes (`PRD-31`), interface (`PRD-32`).

## 3. Spécifications

### 3.1. États

`NONE` → `open()` → `IDLE` → `prepareAsync()` → `READY` → `play()` → `PLAYING` ↔ `PAUSED`. `stop()` revient à `IDLE`. `close()` revient à `NONE` `[DOC]`.

Règles d'appel `[DOC]` :

| Méthodes | États |
| :--- | :--- |
| `setListener`, `setDisplayRect` | Avant `prepareAsync` |
| `setStreamingProperty`, `setDrm`, `setBufferingParam` | `IDLE` seulement |
| `play` | `READY`, `PLAYING`, `PAUSED` |
| `pause` | `PLAYING`, `PAUSED` |
| `seekTo` | `IDLE`, `READY`, `PLAYING`, `PAUSED` |
| `jumpForward`, `jumpBackward` | `READY` après `prepare` synchrone, `PLAYING`, `PAUSED` |
| `suspend` | `READY`, `PLAYING`, `PAUSED` |
| `getTotalTrackInfo`, `setSelectTrack` | `READY`, `PLAYING`, `PAUSED` |

`prepare()` bloque : seul `prepareAsync()` est utilisé `[DOC]`. Une file interne sérialise les commandes. Une nouvelle commande annule les opérations en attente (zapping rapide).

### 3.2. Affichage

Élément `<object type="application/avplayer">` dans la page `[DOC]`. `setDisplayRect` se calcule dans un repère 1920×1080 `[DOC]`. `setDisplayMethod` utilise `PLAYER_DISPLAY_MODE_AUTO_ASPECT_RATIO` par défaut. Plein écran et mini-lecteur utilisent le même objet. L'affichage de HTML au-dessus dépend de `S6`.

### 3.3. Requêtes

- `setStreamingProperty('USER_AGENT', …)` depuis `PlayableMedia` ou les réglages `[DOC]`.
- `COOKIE` si la source en fournit un `[DOC]`.
- `Referer` et en-têtes libres : non pris en charge `[NON DOC]`. Voir `PRD-29`.
- `ADAPTIVE_INFO` : `STARTBITRATE=…`, sur option, pour démarrer vite.
- `IS_LIVE`, `GET_LIVE_DURATION`, `CURRENT_BANDWIDTH` pour l'état.

### 3.4. Événements

`onbufferingstart`, `onbufferingprogress`, `onbufferingcomplete`, `oncurrentplaytime`, `onstreamcompleted`, `onevent`, `onerror`, `onerrormsg`, `onsubtitlechange` `[DOC]`, convertis en événements internes. `oncurrentplaytime` est ralenti vers l'interface à 1 Hz.

### 3.5. Arrière-plan

Selon `S12`. Par défaut : VOD avec `suspend` puis `restore` si la reprise est fiable, sinon fermeture et réouverture à la dernière position. Le direct ferme et rouvre, car le tampon n'a pas d'intérêt après une pause. Samsung demande le même comportement que Retour pendant la lecture `[DOC]`.

### 3.6. Connexions

Une lecture compte pour une connexion de l'abonnement. Avant d'ouvrir un flux, l'ancien est fermé, sauf si `S11` montre qu'un pré-tampon est sûr et que `max_connections` le permet.

### 3.7. DRM

Non concerné. Buntu ne gère pas de contenu protégé et `drmplay` n'est pas déclaré.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Toute méthode AVPlay passe par l'automate. Pas d'appel direct depuis l'interface.
- `prepare()` synchrone interdit.
- L'URL n'apparaît dans aucun journal sans masquage.
- `close()` à la sortie et à chaque erreur fatale.

## 5. Critères d'Acceptation

1. Test de l'automate avec un faux AVPlay qui refuse tout appel dans un état interdit.
2. 500 changements de chaîne d'affilée sur TV réelle sans écran noir ni fuite.
3. Le test `VI-PC-01` passe : la commande reste possible pendant le chargement.

## 6. Sources

- [Using AVPlay](https://developer.samsung.com/smarttv/develop/guides/multimedia/media-playback/using-avplay.html)
- [AVPlay API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplay-api.html)
- [Adaptive Streaming](https://developer.samsung.com/smarttv/develop/guides/multimedia/adaptive-streaming.html)
- [Multitasking](https://developer.samsung.com/smarttv/develop/guides/fundamentals/multitasking.html)
- [Video checklist](https://developer.samsung.com/smarttv/develop/development-checklist/video.html)
