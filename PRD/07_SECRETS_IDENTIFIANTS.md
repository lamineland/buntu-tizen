# PRD 07 — Stockage sécurisé des identifiants

- **ID :** `PRD-07`
- **Prérequis :** `PRD-03`, `PRD-05`
- **Origine Buntu :** `apple/Sources/CndIptvKit/Security/KeychainStore.swift`, `BUNTU-PRD-03` § 3.1, `crates/cnd-core/src/sharing/credentials.rs`
- **Couches impactées :** `src/platform/secrets/`
- **Statut :** `[ ]`
- **Objectif :** aucun mot de passe en clair dans IndexedDB, les journaux ou les fichiers.

---

## 1. Contexte & Objectif

Sur Apple, le mot de passe est dans le Trousseau et SQLite ne garde qu'un `password_ref`. Sur TV, il n'y a pas de Trousseau, et Samsung demande de chiffrer les données sensibles sans nommer d'API dans son guide de sécurité `[DOC]`.

## 2. Responsabilité

**Fait :** ranger, lire et supprimer les secrets (mots de passe Xtream, URL de playlist avec identifiants, clés d'API, hachage du PIN), et retirer les identifiants des URL stockées.

**Ne fait pas :** formulaire de saisie (`PRD-35`), masquage dans les journaux (`PRD-08`).

## 3. Spécifications

### 3.1. Options

| Option | Constat | Verdict |
| :--- | :--- | :---: |
| **`webapis.widgetdata`** | « Application secure storage », écriture chiffrée, niveau Public, Tizen 4.0 et plus, 20 000 caractères par écriture `[DOC]`. Algorithme et isolation `[NON DOC]` | **Recommandée** |
| `tizen.keymanager` | Conçue pour cet usage, mais dite « no longer available » par le Launch Checklist `[DOC]` | Rejetée, sauf démenti de `S9` |
| Chiffrement WebCrypto, clé dans IndexedDB | La clé est à côté des données : pas de protection réelle | Rejetée |
| `localStorage` en clair | Contraire au guide de sécurité | Interdite |

### 3.2. Modèle

Un seul coffre JSON écrit dans `widgetdata` : `{ "v": 1, "secrets": { "<ref>": "<valeur>" } }`, lu une fois au lancement et gardé en mémoire dans le fil principal. Le moteur reçoit un secret seulement le temps d'un appel, comme le passage de `password` à Rust.

Si le coffre approche 20 000 caractères, il passe en mode enveloppe : une clé maîtresse aléatoire reste dans `widgetdata`, et les secrets sont chiffrés en AES-GCM dans `wgt-private`. L'AES est fourni par `crypto.subtle` s'il est disponible (`S9`), sinon par une bibliothèque JavaScript auditée. Ce mode ne s'active que si le besoin est mesuré.

Références : `cnd.xtream.<uuid>`, `cnd.m3u.<uuid>`, `cnd.omdb`, `cnd.tmdb`, `cnd.ai.<fournisseur>`, `cnd.opensubtitles`, `cnd.pin.<profil>`.

### 3.3. URL sans identifiants

Amélioration par rapport à Buntu, où les URL de flux Xtream sont stockées complètes.

- **Xtream :** on stocke `stream_id`, `kind` et `ext`. L'URL est reconstruite juste avant la lecture.
- **M3U :** l'URL de la playlist va dans le coffre. Dans les URL de flux, les segments égaux aux identifiants de la source sont remplacés par `{u}` et `{p}`, selon la logique de `strip_credentials`.
- Les paramètres sensibles restants sont listés par `SENSITIVE_KEYS`. Un flux à jeton unique reste tel quel : c'est un risque connu, noté dans la fiche de sécurité.

### 3.4. Limites à dire honnêtement

`widgetdata` protège mal d'un accès avec privilèges système, et une URL de lecture contient le mot de passe en mémoire et sur le réseau pendant la lecture. Beaucoup de serveurs n'offrent que `http://`. L'interface signale une source non chiffrée sans promettre une sécurité absolue, conformément au vocabulaire de la marque.

### 3.5. Effacement

Suppression d'une source : secret supprimé. Désinstallation : aucune donnée ne doit rester `[DOC]`, ce qui se vérifie sur TV (`S9`). Si le coffre est illisible après une mise à jour, l'app redemande les identifiants sans perdre les sources.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de secret dans `documents`, `localStorage`, IndexedDB, une URL de journal ou un message d'erreur.
- Pas de clé d'API intégrée au paquet.
- Le privilège `http://developer.samsung.com/privilege/widgetdata` est déclaré.

## 5. Critères d'Acceptation

1. Une recherche d'un mot de passe de test dans un export d'IndexedDB ne trouve rien.
2. Le coffre survit à une mise à jour et disparaît à la désinstallation, sur TV réelle.
3. Une URL de lecture est reconstruite à l'identique pour les cas de `credentials.rs`.

## 6. Sources

- [WidgetData API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/widgetdata-api.html)
- [Application Security](https://developer.samsung.com/smarttv/develop/guides/fundamentals/application-security.html)
- [Launch Checklist](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/launch-checklist.html)
- [Tizen Web Device API](https://developer.samsung.com/smarttv/develop/api-references/tizen-web-device-api-references.html)
- [Secure Key Management (Tizen)](https://github.com/Samsung/tizen-docs/blob/master/docs/application/web/guides/security/secure-key.md)
- [Account checklist](https://developer.samsung.com/smarttv/develop/development-checklist/account.html)
