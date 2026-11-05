# Registre des faits Tizen

Relevé le **5 novembre 2026** sur la documentation Samsung. Les pages consultées n'affichent pas de date de mise à jour.

## Niveaux de certitude

| Marque | Sens |
| :--- | :--- |
| `[DOC]` | Écrit dans une page officielle Samsung ou Tizen. |
| `[NON DOC]` | Cherché dans les pages officielles lues et non trouvé. Cela ne prouve pas que la fonction n'existe pas. |
| `[COMM]` | Source communautaire, tierce ou commerciale. Indice seulement, jamais une base de décision. |
| `[CIBLE]` | Objectif interne du projet, à mesurer. Ce n'est pas une exigence Samsung. |

Un fait `[NON DOC]` ou `[COMM]` dont dépend une décision a une preuve technique dans le [`PRD-03`](../PRD/03_SPIKES_VALIDATION_TV_REELLE.md).

## 1. Versions

`[DOC]` [Web Engine Specifications](https://developer.samsung.com/smarttv/develop/specifications/web-engine-specifications.html), recoupé par les User-Agent de [Retrieving Platform Information](https://developer.samsung.com/smarttv/develop/guides/fundamentals/retrieving-platform-information.html).

| Année TV | Tizen | Moteur web |
| :---: | :---: | :---: |
| 2026 | 10.0 | Chromium M130 |
| 2025 | 9.0 | Chromium M120 |
| 2024 | 8.0 | M108 |
| 2023 | 7.0 | M94 |
| 2022 | 6.5 | M85 |
| 2021 | 6.0 | M76 |
| 2020 | 5.5 | M69 |
| 2019 | 5.0 | M63 |
| 2018 | 4.0 | M56 |
| 2017 | 3.0 | M47 |

- `[DOC]` Résolution d'application : 1920×1080 sur tous les modèles ([General Specifications](https://developer.samsung.com/smarttv/develop/specifications/general-specifications.html)).
- `[DOC]` Mise à niveau de l'OS à partir de la gamme 2023 (même page). Une TV 2023 peut donc tourner sur un Tizen plus récent que celui de son année.
- `[DOC]` WebAssembly à partir de Tizen 5.5, modèles 2020 ([WebAssembly](https://developer.samsung.com/smarttv/develop/extension-libraries/webassembly/webassembly.html)).
- `[NON DOC]` Lignes du tableau pour WebAssembly, fils WASM, CompressionStream et OPFS. L'extraction du tableau n'a pas permis non plus d'attribuer une version à Web Crypto, IndexedDB, ReadableStream et Service Worker. Ces fonctions se testent sur TV (`S7`, `S8`, `S9`).

## 2. Outils

- `[DOC]` Dernière image TV Extension : 10.0.0 du 26 février 2026, image 64 bits, Chromium M130, .NET 8 ([Release History](https://developer.samsung.com/smarttv/develop/tools/tv-extension/release-history.html)).
- `[DOC]` Samsung Certificate Extension 2.0.73 ou plus récente requise pour créer des certificats après septembre 2025 ([Installing TV SDK](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/installing-tv-sdk.html)).
- `[DOC]` Les prérequis citent macOS « on Intel-based hardware » et aucune version au-delà de 10.12. Apple Silicon n'est pas mentionné ([TV Extension Prerequisites](https://developer.samsung.com/smarttv/develop/tools/prerequisites.html)).
- `[COMM]` Sur le [forum Samsung](https://forum.developer.samsung.com/t/tizen-studio-and-mac-os-issues/9279), un représentant de Samsung écrit en décembre 2020 que Tizen Studio ne prend en charge que le matériel x64. Des utilisateurs décrivent en 2021 une installation sur Mac M1 en copiant le JDK à la main. En novembre 2022, un utilisateur signale que l'émulateur TV ne se lance pas sur M1, sans confirmation de Samsung. Rien de plus récent n'a été trouvé.
- `[DOC]` Seuls les `.wgt` s'installent sur TV par la CLI ; l'installation par USB n'est plus possible ([CLI](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/command-line-interface.html), [TV Device](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html)).
- `[DOC]` Flutter à partir de Tizen 6.0, avec le SDK 2026 obligatoire ([Flutter](https://developer.samsung.com/smarttv/develop/native/flutter.html)).
- `[DOC]` La documentation Tizen migre : `docs.tizen.org` redirige vers `samsungtizenos.com/docs`. Les sources restent lisibles sur [Samsung/tizen-docs](https://github.com/Samsung/tizen-docs).

## 3. Lecture

- `[DOC]` États AVPlay : `NONE`, `IDLE`, `READY`, `PLAYING`, `PAUSED` ([Using AVPlay](https://developer.samsung.com/smarttv/develop/guides/multimedia/media-playback/using-avplay.html)).
- `[DOC]` `setDisplayRect` raisonne en 1920×1080 quelle que soit la résolution de l'application ([AVPlay API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplay-api.html)).
- `[DOC]` Propriétés de requête HTTP de `setStreamingProperty` : `COOKIE` et `USER_AGENT` seulement (même page).
- `[NON DOC]` En-têtes arbitraires et `Referer` sur les requêtes de média.
- `[NON DOC]` Flux MPEG-TS sans fin sur `http://`. TS figure comme conteneur et comme format de segment HLS.
- `[COMM]` Un lecteur IPTV Tizen libre dit lire du MPEG-TS par AVPlay et passer par un proxy local pour les en-têtes ([Tizen-IPTV-Player](https://github.com/Rusty24h/Tizen-IPTV-Player)).
- `[DOC]` DTS n'est pas pris en charge sur les TV 2020, 2021, 2022, 2024, 2025 et 2026 ([spécifications 2020](https://developer.samsung.com/smarttv/develop/specifications/media-specifications/2020-tv-video-specifications.html), [2026](https://developer.samsung.com/smarttv/develop/specifications/media-specifications/2026-tv-video-specifications.html)). La page 2023 n'a pas été lue.
- `[DOC]` WMV, VC-1 et WMA ne sont plus pris en charge à partir de 2025 ; RA, RV et RMVB à partir de 2026 (spécifications 2026).
- `[DOC]` MSE limité à la FHD, et le seul conteneur cité pour MSE est WebM ([Media Specifications](https://developer.samsung.com/smarttv/develop/specifications/media-specifications.html)).
- `[DOC]` Sous-titres AVPlay : SAMI (UTF-8), SMPTE-TT, DFXP. Fichiers externes en chemin local absolu seulement ([Subtitles](https://developer.samsung.com/smarttv/develop/guides/multimedia/subtitles.html)).
- `[DOC]` `avplaystore` : jusqu'à 4 instances pour le pré-tampon ([AVPlayStore API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplaystore-api.html)).
- `[NON DOC]` Plusieurs instances à l'état `PLAYING` en même temps.
- `[DOC]` `IN_APP_MULTIVIEW` à partir de Tizen 7.0 (AVPlay API).
- `[DOC]` La fenêtre PiP de `tizen.tvwindow` se place au-dessus du HTML et la page ne décrit pas de lecture d'un flux de l'application ([Managing PiP](https://developer.samsung.com/smarttv/develop/guides/pip-picture-in-picture/managing-pip-windows.html)).
- `[NON DOC]` Superposition HTML au-dessus de la zone AVPlay, et réglage applicatif de la fréquence d'image.

## 4. Stockage et sécurité

- `[DOC]` Web Storage : 5 Mo au plus ([Using Web Storage](https://developer.samsung.com/smarttv/develop/guides/data-handling/using-web-storage.html)).
- `[DOC]` `wgt-private` est supprimé à la désinstallation ; `documents` ne l'est pas ; `wgt-private-tmp` est vidé à l'arrêt de l'application ou du runtime ([File Operations](https://developer.samsung.com/smarttv/develop/guides/data-handling/managing-file-operations.html)).
- `[DOC]` Le privilège `unlimitedstorage` couvre IndexedDB (partiel) ([Configuring TV Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/configuring-tv-applications.html)).
- `[NON DOC]` Quota IndexedDB chiffré, limite de mémoire par application, limite du tas WASM, limite d'écriture sur la mémoire flash.
- `[DOC]` Une image 1920×1080 décodée pèse environ 8,3 Mo, une image 4K environ 33 Mo. Sous pression mémoire, le système tue des applications en arrière-plan ([Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)).
- `[DOC]` `webapis.widgetdata` : « application secure storage », données chiffrées, 20 000 caractères par écriture, niveau Public, méthodes à partir de Tizen 4.0 ([WidgetData API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/widgetdata-api.html)).
- `[NON DOC]` Algorithme, portée de la clé et isolation de `widgetdata`.
- **Contradiction** `[DOC]` : `keymanager` est marqué obligatoire sur TV dans l'[index des API](https://developer.samsung.com/smarttv/develop/api-references/tizen-web-device-api-references.html), alors que le [Launch Checklist](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/launch-checklist.html) et [Registering Applications](https://developer.samsung.com/tv-seller-office/guides/applications/registering-application.html) le disent « no longer available ». Pour la boutique, c'est la seconde source qui prime.
- `[DOC]` Le guide CORS exige l'en-tête côté serveur ; WARP interdit le réseau par défaut ([CORS](https://github.com/Samsung/tizen-docs/blob/master/docs/application/web/guides/w3c/security/cors.md), Configuring TV Applications).
- `[NON DOC]` Dispense de CORS par `<access origin="*">` dans un `.wgt`, et règle sur le HTTP non chiffré.
- `[DOC]` `tizen:service` est réservé aux partenaires (Registering Applications).
- `[DOC]` Les données personnelles et les identifiants doivent disparaître à la désinstallation ([Mandatory Features](https://developer.samsung.com/smarttv/develop/development-checklist/mandatory-features.html), [Account](https://developer.samsung.com/smarttv/develop/development-checklist/account.html)).

## 5. Comportements exigés

- `[DOC]` Lancement en 10 secondes au plus, 5 secondes idéalement (Mandatory Features). Réponse en 30 secondes au plus ([Common](https://developer.samsung.com/smarttv/develop/development-checklist/common.html)).
- `[DOC]` Sur la page d'accueil, Retour affiche une confirmation de sortie construite par l'application ([Terminating Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/terminating-applications.html)).
- `[DOC]` Le multitâche est obligatoire ; le JavaScript est suspendu quand l'application est cachée ([Multitasking](https://developer.samsung.com/smarttv/develop/guides/fundamentals/multitasking.html)).
- `[DOC]` Une perte de réseau doit afficher un message ; l'écran de veille ne doit pas apparaître pendant la lecture (Mandatory Features).

## 6. Boutique

- `[DOC]` Un **Public Seller ne peut publier qu'aux États-Unis**. Publier ailleurs demande le statut Partner Seller, donc un contrat hors ligne avec Samsung et l'accord d'un Content Manager ([Seller Office member](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-seller-office-member.html), [Becoming Partner](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-partner.html)).
- `[DOC]` Les applications ne doivent contenir ni violence, ni pornographie, ni langage injurieux ; un classement 18+ ne peut être publié en Corée, en Arabie saoudite et aux Émirats ([Age Rating](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/age-rating.html), [Application Registration Q&A](https://developer.samsung.com/tv-seller-office/faq/application-registration.html)).
- `[NON DOC]` Toute règle publique sur les lecteurs IPTV, les playlists fournies par l'utilisateur ou la preuve des droits. La page Age Rating renvoie à la section 3 d'un « Samsung Apps Certification Guide » qui n'a pas été trouvé, et les conditions d'utilisation du Seller Office n'ont pas été lues.
- `[COMM]` [OTTV](https://www.ottv.org/docs/setup-iptv-on-samsung-tv), vendeur d'abonnements, affirme sans source que Samsung a retiré des applications IPTV au Royaume-Uni et en Italie à partir de 2023. Non vérifié.
- `[COMM]` [IPTV One](https://www.iptv-one.app/en/blog/best-iptv-player-samsung-tv-v2), éditeur d'un lecteur concurrent, écrit que les lecteurs IPTV dédiés sont presque absents de la boutique Tizen.
- `[DOC]` En 2026, Samsung interdit les SDK de proxy résidentiel (Registering Applications ; [TechCrunch, 3 août 2026](https://techcrunch.com/2026/08/03/samsung-bans-smart-tv-apps-that-share-users-internet-connections-with-strangers/)).
- `[DOC]` Pas de délai garanti pour la certification ([Certification Process](https://developer.samsung.com/tv-seller-office/faq/certification-process.html)).
- `[DOC]` Une déclaration de conformité à l'European Accessibility Act est demandée pour l'UE ([Entering Application Information](https://developer.samsung.com/tv-seller-office/guides/applications/entering-application-information.html)).
