# PRD 33 — Replay et « Revoir depuis le début »

- **ID :** `PRD-33`
- **Prérequis :** `PRD-21`, `PRD-23`, `PRD-28`, `PRD-29`
- **Origine Buntu :** `crates/cnd-core/src/iptv/catchup.rs`, `BUNTU-PRD-15`
- **Couches impactées :** `src/engine/catchup/`
- **Statut :** `[ ]`
- **Objectif :** mêmes URL de replay que `catchup.rs`, à partir d'un programme du guide.

---

## 1. Contexte & Objectif

Le replay serveur est le seul moyen de revenir en arrière sur TV, puisque la pause locale et le PVR sont abandonnés. Il ne dépend que de l'URL : la logique se porte telle quelle.

## 2. Responsabilité

**Fait :** dire si un programme est disponible en replay, construire son URL.

**Ne fait pas :** grille (`PRD-37`), lecture (`PRD-28`).

## 3. Spécifications

Port de `catchup.rs` :

- `isCatchupSupported`, `isCatchupAvailable`, `buildCatchupUrl`.
- Formats : Xtream `/timeshift/{u}/{p}/{duration}/{YYYY-MM-DD:HH-MM}/{id}.ext`, Flussonic `timeshift_abs-{utc}.m3u8`, modes `default`, `append`, `shift`, `flussonic`.
- Gestion de l'heure du serveur : `server_utc_offset` à partir de `time_now` et `timestamp_now`.
- `catchup_days` absent vaut 7 jours.
- L'URL de replay se construit au dernier moment, avec les identifiants du coffre (`PRD-07`).
- Dans le lecteur, un replay se comporte comme une VOD si AVPlay rapporte une durée, sinon sans saut.
- « Revoir depuis le début » : replay du programme en cours.

Pause du direct : pas de tampon local. La touche pause propose le replay si la chaîne le permet. Sinon, elle indique que la chaîne ne peut pas être mise en pause. Le saut dans un direct est limité aux segments gardés par le serveur `[DOC]`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- La date d'un replay ne se calcule pas avec un fuseau local sans offset serveur.
- Pas d'URL de replay dans les journaux sans masquage.

## 5. Critères d'Acceptation

1. Tous les cas de `catchup.rs` passent en fixtures de parité.
2. Un replay Xtream se lit sur TV réelle, sur une source de démonstration qui l'expose.

## 6. Sources

- [Adaptive Streaming](https://developer.samsung.com/smarttv/develop/guides/multimedia/adaptive-streaming.html)
