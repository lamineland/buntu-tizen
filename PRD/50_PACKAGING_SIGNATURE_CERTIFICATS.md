# PRD 50 — Paquet, signature et certificats

- **ID :** `PRD-50`
- **Prérequis :** `PRD-02`, `PRD-04`
- **Origine Buntu :** `BUNTU-PRD-23`
- **Couches impactées :** `config.xml`, scripts de build, coffre hors dépôt
- **Statut :** `[ ]`
- **Objectif :** un `.wgt` reproductible, et un certificat d'auteur qu'on ne peut pas perdre.

---

## 1. Contexte & Objectif

Une mise à jour doit être signée avec le **même certificat d'auteur** que la première version. Sinon, elle est considérée comme une autre application `[DOC]`. Perdre ce certificat, c'est perdre le lien avec les installations existantes.

## 2. Responsabilité

**Fait :** identité de l'app, `config.xml` final, certificats, paquet, versions.

**Ne fait pas :** certificat de développement sur TV (`PRD-02`), dépôt (`PRD-52`).

## 3. Spécifications

### 3.1. Identité

- Tizen ID et Package ID générés au premier build, puis figés. Une mise à jour garde le même ID `[DOC]`. Package ID sans caractère spécial `[DOC]`.
- Nom affiché : **Buntu**, sous réserve de la vérification de marque.
- `required_version` au format `x.y` `[DOC]`, selon le plancher du `PRD-02`.
- Le titre de la langue par défaut du Seller Office égale le nom du `config.xml` `[DOC]`.

### 3.2. Version

Format `[0-255].[0-255].[0-65535]`, toujours croissant, jamais réutilisé `[DOC]`. Numéro issu d'une seule source dans le dépôt.

### 3.3. Privilèges

Liste minimale, chacun justifié : `tv.inputdevice`, `network.public`, `productinfo`, `widgetdata`, `unlimitedstorage`, `filesystem.read`, et `download` si `S1` l'impose. Pas de `drmplay`, ni de `tizen:service`, ni de privilège partenaire. `keymanager`, `systemmanager` et `websetting` ne sont pas utilisés `[DOC]`.

### 3.4. Certificats

- Profil de type Samsung, appareil TV `[DOC]`, avec Samsung Certificate Extension 2.0.73 ou plus récente `[DOC]`.
- Certificat distributeur de niveau Public en v1. Partner seulement si le statut et un besoin le justifient `[DOC]`.
- Durée de validité : à relever au moment de la création, car elle n'est pas indiquée dans les pages lues.
- **Sauvegarde :** `author.p12`, mot de passe et profil rangés dans deux lieux chiffrés, hors du dépôt. Procédure de restauration essayée une fois.

### 3.5. Paquet

`.wgt` seul, sans architecture multiple. Extensions en minuscules, nom de 100 octets au plus, seuls espaces et `_` comme caractères spéciaux `[DOC]`. Sans source maps, sans fichiers de test, sans secrets. Contrôle automatique du contenu au build.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun certificat ni mot de passe dans le dépôt ou les journaux de build.
- Une version publiée ne se reconstruit pas avec un autre certificat d'auteur.
- Aucun privilège partenaire sans accord écrit, sinon blocage au pré-test `[DOC]`.

## 5. Critères d'Acceptation

1. Le build produit un `.wgt` signé qui s'installe sur une TV réelle.
2. Une mise à jour signée installée par-dessus conserve les données.
3. La restauration du certificat depuis la sauvegarde fonctionne.

## 6. Sources

- [Creating Certificates](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/creating-certificates.html)
- [Installing TV SDK](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/installing-tv-sdk.html)
- [Configuring TV Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/configuring-tv-applications.html)
- [Privileges Q&A](https://developer.samsung.com/smarttv/develop/faq/privileges.html)
- [Launch Checklist](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/launch-checklist.html)
- [Registering Applications](https://developer.samsung.com/tv-seller-office/guides/applications/registering-application.html)
