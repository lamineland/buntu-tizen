# PRD 01 — Périmètre et matrice de portage

- **ID :** `PRD-01`
- **Prérequis :** aucun
- **Origine Buntu :** `README.md`, `PRD/00_INDEX_ROADMAP.md`, `PRD/14_ETAT_DES_LIEUX_ET_ROADMAP_INNOVATION.md`, `docs/BRANDING_BUNTU.md`
- **Couches impactées :** documentation seulement
- **Statut :** `[ ]`
- **Objectif :** chaque fonction de Buntu a une décision écrite : portée, remplacée ou abandonnée.

---

## 1. Contexte & Objectif

Buntu repose sur SwiftUI, sur le moteur Rust `cnd-core` et sur deux lecteurs (`AVPlayer`, `libmpv`). Rien de cela ne s'exécute sur une TV Samsung. Ce PRD fixe ce que la version TV doit faire, pour qu'aucun autre PRD n'ait à le décider.

## 2. Responsabilité

**Fait :** tenir la liste des fonctions et leur décision de portage.

**Ne fait pas :** spécifier une fonction (chaque ligne renvoie à son PRD) ni choisir une technologie (`PRD-02`, `PRD-06`).

## 3. Spécifications

### 3.1. Fonctions portées

| Fonction Buntu | PRD Buntu | Décision TV | PRD | Lot |
| :--- | :---: | :--- | :---: | :---: |
| Sources M3U (URL) | 02, 09 | Portée | 20, 35 | v1 |
| Sources M3U (fichier) | 09 | Remplacée : fichier sur clé USB, pas de glisser-déposer | 42, 43 | v1.1 |
| Comptes Xtream et test du compte | 03 | Portés | 21, 35 | v1 |
| Normalisation séries et titres | 02 | Portée | 19 | v1 |
| Live TV, catégories, zapping | 09 | Portés | 36 | v1 |
| Guide TV XMLTV | 04, 09 | Porté, fenêtre réduite | 23, 37 | v1 |
| Films et séries | 10 | Portés | 38, 39 | v1 |
| Recherche plein texte (FTS5) | 05 | Remplacée : index maison en IndexedDB | 25, 40 | v1 |
| Favoris, récents, reprise | 05 | Portés | 26 | v1 |
| Enrichissement OMDb / TMDB | 06 | Porté, à la demande | 27 | v1 |
| Pistes audio et sous-titres intégrés | 08 | Portés selon AVPlay | 31 | v1 |
| Reconnexion et surveillance de lecture | 08 | Portées | 30 | v1 |
| Replay et « Revoir depuis le début » | 15 | Portés (replay serveur) | 33 | v1 |
| Réglages et console des journaux | 11 | Portés | 41 | v1 |
| Profils, bouquets, contrôle parental | 20 | Portés. Le masquage adulte est obligatoire en v1 | 45 | v1 |
| Bascule vers un miroir | 17 | Portée, sans sonde de fond | 34 | v1.1 |
| Import d'un partage `.cndshare` | 26 | Porté en lecture seule | 43 | v1.1 |
| Assistant de chaînes | 13 | Porté, palette à la télécommande | 46 | v1.1 |
| « À suivre » et sagas | 27 | Portés sans fournisseur IA | 47 | v1.1 |
| Sous-titres OpenSubtitles | 18 | Portés, rendus en HTML | 48 | v1.1 |
| Médiathèque locale | 32 | Remplacée : parcours d'une clé USB, sans index persistant | 44 | v1.1 |
| Mosaïque 2 à 4 flux | 16 | Conditionnelle : 2 flux au plus si `S11` réussit | 49 | Option |
| Accessibilité | 31 | Portée sur les outils de la TV | 17 | v1 |
| Taille d'interface | 30 | Remplacée : grille fixe 1920×1080, 2 tailles de texte | 15 | v1 |

### 3.2. Fonctions non portées

| Fonction Buntu | PRD Buntu | Pourquoi elle ne se porte pas | Remplacement sur TV |
| :--- | :---: | :--- | :--- |
| Moteur Rust `cnd-core`, UniFFI | tous | Pas de code natif prévu ; tokio, reqwest et rusqlite ne visent pas la TV | Réécriture TypeScript (`PRD-05`, `PRD-06`) |
| SQLite WAL + FTS5 | 01, 05 | Même raison | IndexedDB et index maison (`PRD-06`, `PRD-25`) |
| Téléchargeur multi-segments | 07 | Le JavaScript s'arrête quand l'app est cachée, le service de fond est réservé aux partenaires, la flash est partagée et sans `pwrite` | **Abandon.** Reprise de lecture à la place |
| PVR, enregistrement programmé | 19 | Même raison, et une TV éteinte n'exécute rien | **Abandon.** Replay serveur (`PRD-33`) |
| Pause du direct locale | 24 | Tampon `libmpv`, pas d'équivalent documenté | Pause si le flux la permet, sinon replay (`PRD-33`) |
| `libmpv` | 08 | Pas de code natif prévu | AVPlay (`PRD-28`). Les formats refusés sont signalés (`PRD-29`) |
| Lecteurs externes (VLC, Infuse…) | 08 | N'existent pas sur la TV | **Abandon** |
| Dialogue Boost, mode nuit | 18 | Filtres `mpv` | **Abandon.** Réglages son de la TV |
| Fréquence d'image automatique | 25 | Aucune commande documentée | **Abandon** tant qu'aucune API n'est trouvée |
| Image dans l'image | 12 | `tvwindow` n'est pas décrite pour un flux de l'app | Mini-lecteur interne avec `setDisplayRect` (`PRD-32`) |
| AirPlay | 12 | Protocole Apple, l'app TV n'en est pas l'émetteur | **Abandon** |
| Now Playing, Raccourcis, gestes | 12 | Propres à Apple | Touches média (`PRD-14`) |
| iCloud, CloudKit | 12, 26 | Propres à Apple | Import `.cndshare` (`PRD-43`). Pas de synchronisation en v1 |
| Trousseau | 03 | Propre à Apple | `webapis.widgetdata` (`PRD-07`) |
| Face ID, Touch ID | 20 | Propres à Apple | Code PIN seul (`PRD-45`) |
| Export d'un partage | 26 | Sans intérêt à la télécommande | **Abandon.** Import seulement |
| Transfert Bonjour à proximité | 26 | Pas de découverte locale documentée pour une app web | Option par récupération HTTP sur le réseau local (`PRD-43`) |
| Live Activities, visionOS | 22 | Propres à Apple | **Abandon** |
| Fenêtre au premier plan | 29 | Propre au Mac | **Abandon** |
| Sonde de santé périodique | 17 | Pas de tâche de fond, et chaque sonde consomme une connexion de l'abonnement | Mesure pendant la lecture (`PRD-34`) |
| Clés d'API intégrées | 28 | Extractibles d'un `.wgt`, et hors du principe « votre clé » | Clés saisies par l'utilisateur (`PRD-07`, `PRD-27`) |

### 3.3. Hors périmètre, comme sur Apple

Buntu Xibaar et Buntu Hub restent des idées sans PRD. La synchronisation entre appareils est une évolution possible, jamais une promesse de la v1.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune fonction « abandonnée » ne réapparaît dans un autre PRD sans modification de cette matrice.
- L'app ne fournit aucun contenu. Aucune source n'est livrée dans le paquet.
- La marque ne promet que ce qui est livré (`docs/BRANDING_BUNTU.md` § 1.3 de Buntu).

## 5. Critères d'Acceptation

1. Toutes les fonctions du tableau du `README.md` de Buntu figurent dans § 3.1 ou § 3.2.
2. Chaque ligne portée renvoie à un PRD présent dans l'index.
3. Chaque ligne abandonnée donne sa raison.

## 6. Sources

- [Multitasking](https://developer.samsung.com/smarttv/develop/guides/fundamentals/multitasking.html)
- [Registering Applications](https://developer.samsung.com/tv-seller-office/guides/applications/registering-application.html)
- [Registre des faits](../docs/TIZEN_FACTS.md)
