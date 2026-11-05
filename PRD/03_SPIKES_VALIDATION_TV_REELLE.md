# PRD 03 — Preuves techniques sur TV réelle

- **ID :** `PRD-03`
- **Prérequis :** `PRD-02`
- **Origine Buntu :** sans équivalent
- **Couches impactées :** prototypes jetables, hors de `src/`
- **Statut :** `[ ]`
- **Objectif :** répondre à chaque inconnue bloquante avant d'écrire le produit. Les preuves `S1`, `S3` et `S8` décident de la suite du projet.

---

## 1. Contexte & Objectif

La documentation ne répond pas à plusieurs questions dont dépend l'architecture (`docs/TIZEN_FACTS.md`). Un lecteur IPTV sur TV dépend surtout de deux points non documentés : joindre des serveurs sans en-têtes CORS, et lire leurs flux.

## 2. Responsabilité

**Fait :** définir, exécuter et consigner les preuves, puis indiquer la décision qui en découle.

**Ne fait pas :** livrer du code de production. Les prototypes sont jetés.

## 3. Spécifications

### 3.1. Matériel minimal

Une TV 2021 ou 2022 (bas de palier), une TV 2024 ou plus récente, et idéalement une TV 2020. Des flux de test libres de droits ou dont l'utilisateur détient les droits : HLS, MPEG-TS, MP4, MKV, sur `http://` et `https://`.

### 3.2. Liste des preuves

| ID | Question | Réussite | Si échec | PRD touchés |
| :---: | :--- | :--- | :--- | :--- |
| **S1** | Un `.wgt` avec `<access origin="*">` peut-il lire la réponse d'un serveur sans CORS ? | `fetch` ou XHR lit le corps | Essayer `tizen.download` puis lecture du fichier. Si les deux échouent : **arrêt ou révision du projet** | 09, 20, 21, 23 |
| S2 | `fetch` et AVPlay acceptent-ils `http://` depuis un `.wgt` ? | Oui aux deux | Sources HTTPS seules, couverture très réduite | 09, 28, 04 |
| **S3** | AVPlay lit-il un flux MPEG-TS sans fin en `http://…/x.ts`, et un HLS Xtream ? | 30 minutes sans coupure, au moins un des deux formats | HLS seul ; si aucun des deux : **arrêt** | 28, 29 |
| S4 | AVPlay lit-il et avance-t-il en MP4 et MKV distants ? | Lecture et saut corrects | Liste réduite de formats | 29, 38 |
| S5 | Quel User-Agent part avec `fetch`, et `USER_AGENT` d'AVPlay est-il appliqué ? | Valeurs relevées côté serveur | Documenter les serveurs refusés | 09, 28 |
| S6 | Un élément HTML s'affiche-t-il au-dessus d'AVPlay ? | Bandeau et sous-titres visibles | Revoir l'interface du lecteur | 30, 31, 32, 48 |
| S7 | `Response.body` et les Web Workers fonctionnent-ils sur le palier le plus ancien ? | Analyse progressive d'un fichier de 100 Mo sans hausse mémoire | Découpage par `Range` ou fichier intermédiaire | 05, 20, 23 |
| **S8** | IndexedDB tient-il 100 000 éléments et leur index, et survit-il à une mise à jour ? | Écriture et lecture dans les budgets du `PRD-12`, données intactes | Plafond réduit, ou stratégie à la demande | 06, 12 |
| S9 | `widgetdata` et `crypto.subtle` sont-ils utilisables ? `keymanager` passe-t-il le pré-test ? | `widgetdata` persiste et disparaît à la désinstallation | Chiffrement JavaScript et clé dans `widgetdata` | 07, 43, 45 |
| S10 | Quand l'app est-elle tuée sous pression mémoire ? | Plafond pratique relevé | Budgets abaissés | 12 |
| S11 | Deux instances AVPlay peuvent-elles jouer ensemble ? Le pré-tampon accélère-t-il le zapping ? | Deux vidéos visibles, gain mesuré | `PRD-49` abandonné | 36, 49 |
| S12 | Après `hide`, `suspend` et `restore` suffisent-ils pour le direct et la VOD ? | Reprise fiable | Fermeture et réouverture | 11, 28 |
| S13 | La CLI et `sdb` marchent-elles sur Mac Apple Silicon ? | Installation sur TV depuis le Mac | Machine x86-64 | 02 |
| S14 | Une base SQLite en WASM tient-elle les mêmes budgets ? (facultatif) | Gain d'au moins 3× sur IndexedDB sans dépasser la mémoire | Rester sur la décision du `PRD-06` | 06 |
| S15 | `listStorages` voit-il une clé USB, et AVPlay lit-il un chemin `removable_*` ? | Parcours et lecture | `PRD-42` et `PRD-44` abandonnés | 42, 44 |

### 3.3. Protocole

Chaque preuve est consignée dans `docs/spikes/Sxx.md` : modèle exact, version Tizen, firmware, protocole, chiffres, conclusion, décision. Une preuve est répétée sur les deux TV au moins.

### 3.4. Porte de décision

- **Go :** `S1`, `S3` et `S8` réussissent, éventuellement par un repli.
- **Révision :** une preuve réussit seulement par un repli coûteux. Les PRD touchés sont amendés avant de continuer.
- **Arrêt :** `S1` ou `S3` échoue sans repli.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun flux protégé ni compte tiers dans les tests ou les fichiers du dépôt.
- Aucune URL contenant des identifiants dans `docs/spikes/`.
- Un résultat de simulateur ne vaut pas preuve.

## 5. Critères d'Acceptation

1. Chaque preuve bloquante a une fiche avec au moins deux TV.
2. La décision Go, Révision ou Arrêt est écrite dans l'index.
3. `docs/TIZEN_FACTS.md` est mis à jour avec les résultats.

## 6. Sources

- [CORS (Tizen)](https://github.com/Samsung/tizen-docs/blob/master/docs/application/web/guides/w3c/security/cors.md)
- [Configuring TV Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/configuring-tv-applications.html)
- [AVPlay API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplay-api.html)
- [Downloading Data](https://developer.samsung.com/smarttv/develop/guides/data-handling/downloading-data.html)
- `[COMM]` [Tizen-IPTV-Player](https://github.com/Rusty24h/Tizen-IPTV-Player), [NuvioTVSmart, issue 821](https://github.com/NuvioMedia/NuvioTVSmart/issues/821)
