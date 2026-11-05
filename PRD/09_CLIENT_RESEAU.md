# PRD 09 — Client réseau

- **ID :** `PRD-09`
- **Prérequis :** `PRD-03`, `PRD-05`, `PRD-08`
- **Origine Buntu :** `crates/cnd-core/src/http_client.rs`, `BUNTU-PRD-01`
- **Couches impactées :** `src/engine/net/`, `config.xml`
- **Statut :** `[ ]`
- **Objectif :** une seule porte d'accès au réseau, en flux continu, annulable et bornée.

---

## 1. Contexte & Objectif

`reqwest` choisit ses en-têtes et ignore CORS. Le moteur web de la TV, lui, applique ses règles : WARP bloque le réseau par défaut `[DOC]`, CORS demande un en-tête côté serveur `[DOC]`, et l'effet de `<access>` n'est pas écrit `[NON DOC]`. Les panneaux IPTV n'envoient généralement pas d'en-têtes CORS. Ce PRD dépend donc directement de `S1`, `S2`, `S5` et `S7`.

## 2. Responsabilité

**Fait :** requêtes, flux, délais, redirections, nouvelles tentatives, décompression, déclarations réseau du `config.xml`.

**Ne fait pas :** analyse des contenus (`PRD-20`, `PRD-21`, `PRD-23`), requêtes du lecteur (`PRD-28`).

## 3. Spécifications

### 3.1. Transports

Un port `HttpTransport`, deux implémentations possibles :

1. **`fetch` dans le Worker**, transport par défaut si `S1` réussit.
2. **`tizen.download` vers un fichier temporaire**, puis lecture par morceaux, si `S1` montre que CORS bloque `fetch`. Il exige le privilège `download`, reste facultatif sur TV et demande un test de capacité `http://tizen.org/feature/download` `[DOC]`. Ce repli convient aux gros fichiers, pas aux petits appels d'API : son coût est à mesurer.

Le service Node local et le proxy d'en-têtes sont exclus : `tizen:service` est réservé aux partenaires `[DOC]`, et un proxy risque d'être confondu avec les pratiques que Samsung vient d'interdire.

### 3.2. En-têtes

`User-Agent` et `Referer` ne se fixent pas librement dans `fetch`. Le réglage `default_user_agent` de Buntu (`VLC/3.0.21 LibVLC/3.0.21`) ne s'applique donc qu'aux requêtes d'AVPlay. `S5` relève ce qui part réellement. Un serveur qui refuse le User-Agent de la TV reçoit un message clair (`PRD-35`).

### 3.3. Flux continu

- Lecture par `Response.body` et décodage progressif en UTF-8 tolérant.
- Gzip : détection par les octets `1f 8b`, comme Buntu, décompression en flux par une petite bibliothèque JavaScript. `DecompressionStream` n'est pas documenté et n'est pas utilisé sans détection.
- Si `S7` échoue : découpage par `Range` si le serveur l'accepte, sinon fichier intermédiaire.

### 3.4. Paramètres

Délais de connexion et de réception repris de Buntu (10 s et 30 s), 10 redirections au plus, 3 essais avec attente progressive pour les appels idempotents. Annulation par `AbortController`. Limite de téléchargements simultanés : 2 sur TV `[CIBLE]`, car un panneau limite les connexions et peut bannir une adresse IP.

### 3.5. `config.xml`

Déclarer `<access origin="*" subdomains="true"/>`, les privilèges nécessaires et `http://samsung.com/tv/metadata/use.network`. Une CSP restrictive sur les scripts, mais ouverte sur `connect-src`, puisque les serveurs sont choisis par l'utilisateur. Le passage en `http://` est signalé à l'utilisateur (`PRD-07` § 3.4).

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun `fetch` hors de ce module, sauf la lecture vidéo et les images.
- Pas de corps complet en mémoire pour un fichier dont la taille est inconnue ou supérieure à 5 Mo.
- Toute requête passe par le masquage du `PRD-08`.

## 5. Critères d'Acceptation

1. Sur TV réelle, lecture d'un fichier de 100 Mo depuis un serveur sans CORS, en HTTP et en HTTPS, sans hausse de mémoire.
2. Une annulation arrête le trafic en moins d'une seconde.
3. Un gzip sans extension `.gz` est reconnu.

## 6. Sources

- [Configuring TV Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/configuring-tv-applications.html)
- [CORS (Tizen)](https://github.com/Samsung/tizen-docs/blob/master/docs/application/web/guides/w3c/security/cors.md)
- [Downloading Data](https://developer.samsung.com/smarttv/develop/guides/data-handling/downloading-data.html)
- [Service Application (Tizen)](https://github.com/Samsung/tizen-docs/blob/master/docs/application/web/guides/applications/service-app.md)
- [Registering Applications](https://developer.samsung.com/tv-seller-office/guides/applications/registering-application.html)
