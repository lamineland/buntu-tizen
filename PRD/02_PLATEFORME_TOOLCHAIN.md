# PRD 02 — Plateforme cible et chaîne d'outils

- **ID :** `PRD-02`
- **Prérequis :** `PRD-01`
- **Origine Buntu :** `Taskfile.yml`, `BUNTU-PRD-23`
- **Couches impactées :** outils de développement, `config.xml`
- **Statut :** `[ ]`
- **Objectif :** un poste installe l'environnement et déploie une application vide sur une vraie TV en moins d'une heure `[CIBLE]`.

---

## 1. Contexte & Objectif

Trois choix conditionnent tout le reste : le type d'application, les années de TV prises en charge et la manière de développer depuis un Mac Apple Silicon, que Samsung ne documente pas.

## 2. Responsabilité

**Fait :** type d'application, langage, versions Tizen, outils, déploiement de développement.

**Ne fait pas :** signature de publication (`PRD-50`), structure du code (`PRD-05`), plan de test (`PRD-51`).

## 3. Spécifications

### 3.1. Type d'application

| Option | Pour | Contre | Décision |
| :--- | :--- | :--- | :---: |
| **Web `.wgt` en TypeScript** | Documentation AVPlay en JavaScript, toutes les API TV, itération rapide, test partiel dans un navigateur | Moteur web figé par année, performance à surveiller | **Retenue** |
| Tizen .NET `.tpk` | C#, modèles 2018 et plus | Peu d'exemples TV, pile Xamarin.Forms, aucun code Buntu réutilisable | Rejetée |
| Flutter `.tpk` | Une base de code possible | Tizen 6.0 et plus, SDK 2026 obligatoire, intégration AVPlay à bâtir | Rejetée |
| Web hébergé | Mise à jour sans certification | Lancement dépendant du réseau, pas de simulateur, contraire au local d'abord | Rejetée |
| Natif C/C++ | Réemploi de `cnd-core` | Possibilité pour un tiers non établie | Rejetée |

### 3.2. Années prises en charge

| Palier | Années | Tizen | Chromium | Traitement |
| :--- | :---: | :---: | :---: | :--- |
| Cœur | 2021 à 2026 | 6.0 à 10.0 | M76 à M130 | Pris en charge et testé |
| Étendu | 2020 | 5.5 | M69 | Selon les résultats de `S7` et `S8` |
| Exclu | 2019 et avant | 5.0 et moins | M63 et moins | Non proposé |

**Recommandation : plancher 2021.** Le bêta-test Seller Office demande des TV 2021 ou plus récentes `[DOC, résumé]`. Un rapport communautaire signale des échecs de HLS fMP4 en direct sur 2020 `[COMM]`. Décision à confirmer par l'utilisateur (index § 6).

Le code cible ES2018 et n'utilise que des fonctions confirmées sur le palier le plus ancien. Les TV 2023 et plus récentes peuvent recevoir un OS plus récent : l'app détecte les capacités et ne déduit rien de l'année `[DOC]`.

### 3.3. Outils

- Tizen Studio avec TV Extensions et Samsung Certificate Extension 2.0.73 ou plus récente `[DOC]`.
- CLI `tizen` et `sdb` pour construire, signer et installer. Le build quotidien ne passe pas par l'IDE.
- L'extension VS Code « Tizen TV Extension » est facultative : sa page ne décrit pas de flux web complet et ne dit rien de macOS.
- Node.js LTS, TypeScript, un bundler, un linter, un lanceur de tests. Les versions sont figées dans le dépôt.

### 3.4. Poste de développement

Samsung ne cite que le Mac Intel `[DOC]`. Des utilisateurs ont installé Tizen Studio sur Mac M1 en 2021 en contournant l'installeur, et un utilisateur signale en 2022 que l'émulateur TV ne s'y lance pas `[COMM]`. Ces retours sont anciens : `S13` tranche. Stratégie, de la plus simple à la plus lourde :

1. Interface et logique dans Chrome sur le Mac, avec les adaptateurs simulés du `PRD-05`.
2. Déploiement sur une **vraie TV** en mode développeur depuis le Mac, si `S13` confirme que la CLI et `sdb` fonctionnent.
3. Sinon, une machine x86-64 Windows ou Ubuntu pour la CLI et l'émulateur.

L'émulateur ne fonctionne pas dans Parallels, VMware, VirtualBox ni en bureau à distance `[DOC]`. Une machine virtuelle sur Mac n'est donc pas une solution.

### 3.5. Mode développeur de la TV

Applications > App Settings > saisir `12345` > Developer mode sur On > IP de l'ordinateur > redémarrer `[DOC]`. Puis Device Manager, ou `sdb`, avec un certificat contenant le DUID de la TV `[DOC]`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de dépendance à une fonction web plus récente que le plancher sans détection et sans repli.
- Aucun certificat dans le dépôt.
- Les versions d'outils sont notées dans le dépôt.

## 5. Critères d'Acceptation

1. La procédure installe l'environnement depuis un poste vierge.
2. Une application vide tourne sur une TV réelle du palier Cœur.
3. Le résultat de `S13` est noté, et le poste retenu est écrit dans ce PRD.

## 6. Sources

- [Web Engine Specifications](https://developer.samsung.com/smarttv/develop/specifications/web-engine-specifications.html)
- [General Specifications](https://developer.samsung.com/smarttv/develop/specifications/general-specifications.html)
- [Installing TV SDK](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/installing-tv-sdk.html)
- [TV Extension Prerequisites](https://developer.samsung.com/smarttv/develop/tools/prerequisites.html)
- [TV Device](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html)
- [Flutter](https://developer.samsung.com/smarttv/develop/native/flutter.html)
- [VS Code extension](https://developer.samsung.com/smarttv/develop/tools/additional-tools/vscode-extension-new.html)
- [Distributing Applications](https://developer.samsung.com/tv-seller-office/guides/applications/distributing-application.html)
- `[COMM]` [Forum Tizen Studio et macOS](https://forum.developer.samsung.com/t/tizen-studio-and-mac-os-issues/9279), [Moonfin, issue 232](https://github.com/Moonfin-Client/Smart-TV/issues/232)
