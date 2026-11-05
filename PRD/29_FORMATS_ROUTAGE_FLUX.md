# PRD 29 — Formats pris en charge et routage des flux

- **ID :** `PRD-29`
- **Prérequis :** `PRD-03`, `PRD-21`, `PRD-28`
- **Origine Buntu :** `BUNTU-PRD-08` § 3.1, `CHANGELOG.md`
- **Couches impactées :** `src/player/routing/`
- **Statut :** `[ ]`
- **Objectif :** choisir la meilleure URL et dire clairement quand un flux ne peut pas être lu.

---

## 1. Contexte & Objectif

Sur Apple, `libmpv` rattrape presque tout. Sur TV, il n'y a pas de filet : un format refusé par AVPlay ne sera pas lu. Le produit doit le dire au lieu de laisser un écran noir.

## 2. Responsabilité

**Fait :** décider quelle URL jouer et avec quels paramètres, reconnaître ce qui ne se lit pas, expliquer pourquoi.

**Ne fait pas :** lecture (`PRD-28`), reprise après une coupure (`PRD-30`).

## 3. Spécifications

### 3.1. Capacités

| Format | État | Note |
| :--- | :--- | :--- |
| HLS `.m3u8` | Pris en charge `[DOC]` | TS recommandé pour les anciens modèles ; fMP4 et CMAF à partir de 2020 |
| MPEG-DASH `.mpd` | Pris en charge `[DOC]` | |
| MP4, MOV, MKV, AVI, FLV, TS, WebM, ASF, 3GP, VOB, PS | Listés `[DOC]` | Lecture distante à confirmer par `S3`, `S4` |
| H.264, HEVC, VP9, AV1, MPEG-2 | Listés `[DOC]` | Limites par modèle et par année |
| AAC, MP3, AC3, E-AC3 (DD+), Opus | Listés `[DOC]` | |
| **DTS** | **Non pris en charge** sur 2020, 2021, 2022, 2024, 2025 et 2026 `[DOC]` | Page 2023 non lue |
| WMV, VC-1, WMA | Retirés à partir de 2025 `[DOC]` | |
| RealVideo, RealAudio, RMVB | Retirés à partir de 2026 `[DOC]` | |

Les tableaux de Samsung varient selon le modèle. La table de l'app est une aide : le résultat réel d'AVPlay fait foi.

### 3.2. Règles de routage

**Direct Xtream :** `.m3u8` par défaut, `.ts` en repli ou selon le réglage `liveStreamPreference`. L'ordre final dépend de `S3`. Buntu fait déjà ce choix sur iPhone.

**Film et épisode Xtream :** `container_extension` tel quel. La lecture d'un MKV dépend de `S4`.

**M3U :** URL de la playlist.

**Flux avec en-têtes imposés :** `#EXTVLCOPT` et `#KODIPROP` peuvent exiger `Referer` ou d'autres en-têtes. AVPlay ne documente que `USER_AGENT` et `COOKIE` `[DOC]`. Un tel flux est tenté une fois sans ces en-têtes. S'il échoue, il est marqué comme non lisible avec la raison.

### 3.3. Messages

Message précis et action : « Ce film utilise le son DTS, non pris en charge par votre TV. Choisissez une autre piste si elle existe. » La détection d'une piste DTS passe par `getTotalTrackInfo` (`PRD-31`).

### 3.4. Pistes écartées

- **Proxy d'en-têtes local :** demande `tizen:service`, réservé aux partenaires `[DOC]`. Un projet tiers l'utilise `[COMM]`. Exclu en v1 : à réétudier avec un statut de partenaire et l'accord de Samsung.
- **`hls.js` ou transmux TS vers MSE :** limité à la FHD, et le seul conteneur cité pour MSE est WebM `[DOC]`. Peut s'étudier pour un cas ciblé, sans promesse.
- **WASM Player et démultiplexeur FFmpeg :** l'API permet un démultiplexage par l'application `[DOC]`. Le coût (portage, mémoire, licences LGPL, réseau de l'app) est trop élevé pour la v1. C'est la seule voie crédible vers un équivalent de `libmpv`, à garder en recherche.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas d'écran noir silencieux : tout échec se termine par un message.
- La liste de capacités n'est pas présentée comme une garantie à l'utilisateur.

## 5. Critères d'Acceptation

1. Matrice format × année remplie pour chaque TV de test (`PRD-51`).
2. Chaque cas d'échec simulé produit son message.
3. Une chaîne Xtream est lue sur toutes les TV de test, en `.m3u8` ou en `.ts`.

## 6. Sources

- [2026 TV Video Specifications](https://developer.samsung.com/smarttv/develop/specifications/media-specifications/2026-tv-video-specifications.html)
- [2025](https://developer.samsung.com/smarttv/develop/specifications/media-specifications/2025-tv-video-specifications.html), [2024](https://developer.samsung.com/smarttv/develop/specifications/media-specifications/2024-tv-video-specifications.html), [2022](https://developer.samsung.com/smarttv/develop/specifications/media-specifications/2022-tv-video-specifications.html), [2021](https://developer.samsung.com/smarttv/develop/specifications/media-specifications/2021-tv-video-specifications.html), [2020](https://developer.samsung.com/smarttv/develop/specifications/media-specifications/2020-tv-video-specifications.html)
- [Media Specifications](https://developer.samsung.com/smarttv/develop/specifications/media-specifications.html)
- [Adaptive Streaming](https://developer.samsung.com/smarttv/develop/guides/multimedia/adaptive-streaming.html)
- [Tizen WASM Player Features](https://developer.samsung.com/smarttv/develop/extension-libraries/webassembly/tizen-wasm-player/tizen-wasm-player-features.html)
- `[COMM]` [Tizen-IPTV-Player](https://github.com/Rusty24h/Tizen-IPTV-Player), [NuvioTVSmart, issue 821](https://github.com/NuvioMedia/NuvioTVSmart/issues/821)
