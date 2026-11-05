# PRD 43 — Import de sources par fichier (`.cndshare`, `.m3u`)

- **ID :** `PRD-43`
- **Prérequis :** `PRD-07`, `PRD-09`, `PRD-22`, `PRD-35`, `PRD-42`
- **Origine Buntu :** `BUNTU-PRD-26`, `apple/Sources/CndIptvKit/Features/Sharing/ShareEnvelope.swift`, `crates/cnd-core/src/sharing/`
- **Couches impactées :** `src/engine/sharing/`, `src/ui/screens/sources/`
- **Statut :** `[ ]`
- **Objectif :** transformer un fichier en source, sans ressaisie. Pour un partage créé sur Mac ou iPhone, c'est le remplacement d'iCloud.

---

## 1. Contexte & Objectif

Ce PRD évite la saisie la plus pénible. Il accepte deux types de fichiers : une playlist `.m3u` et une enveloppe `.cndshare` produite par l'app Apple, dont le format doit être suivi à l'octet près.

## 2. Responsabilité

**Fait :** reconnaître le fichier, déchiffrer une enveloppe, afficher l'aperçu, créer les sources.

**Ne fait pas :** créer un partage. Aucune modification de Buntu n'est nécessaire pour la v1.

## 3. Spécifications

### 3.1. Format

D'après `ShareEnvelope.swift`, version 1 :

```text
"CNDS1" | longueur de l'en-tête (uint32 big endian) | en-tête JSON | boîte AES-GCM
```

- En-tête JSON : `format` (`cnd-source-share`), `version` (1), `expiresAt`, `sourceCount`, `salt`. Il est produit par `JSONEncoder`, donc `salt` est en base64. Sa taille est inférieure à 4 096 octets.
- Boîte : `nonce (12 octets) | texte chiffré | tag (16 octets)`, avec **les octets exacts de l'en-tête comme données authentifiées**.
- Clé : HKDF-SHA256, code de 16 octets, sel de l'en-tête, info `cnd-source-share-v1`, 32 octets.
- Clair : document JSON compressé par `NSData.compressed(using: .zlib)`. Apple produit ici du DEFLATE brut, sans en-tête zlib : à confirmer par un vecteur de test réel.
- Document : `format`, `version`, `created_at`, `expires_at`, `from`, `sources[]` (`name`, `type`, `server`, `username`, `password`, `url`, `epg_url`, `playlist`, `needs_credentials`). 50 sources au plus.

### 3.2. Code

26 caractères sur l'alphabet `0123456789ABCDEFGHJKMNPQRSTVWXYZ`, 128 bits. Saisie tolérante, comme `parse(code:)` : minuscules, espaces et tirets acceptés, `O` lu `0`, `I` et `L` lus `1`.

### 3.3. Crypto

`crypto.subtle` si `S9` le valide. Sinon AES-GCM et HKDF par une bibliothèque JavaScript auditée, DEFLATE par celle du `PRD-09`.

### 3.4. Arrivée de l'enveloppe

| Canal | Lot | Note |
| :--- | :---: | :--- |
| Fichier `.cndshare` sur clé USB, puis saisie du code | v1.1 | Canal principal |
| URL d'un fichier `.cndshare`, puis saisie du code | v1.1 | Le fichier reste chiffré sur l'hébergement choisi par l'utilisateur |
| Coller un lien `cndiptv://…` | Non | Pas de presse-papiers pratique à la télécommande |
| Schéma d'URL, double-clic | Non | Sans équivalent sur la TV |
| Mac ou iPhone qui expose l'enveloppe sur le réseau local | Évolution | Demande un changement dans le dépôt Buntu, et son propre PRD des deux côtés |

Un QR code affiché sur la TV ne suffit pas : il faudrait un serveur relais, contraire à « sans serveur tiers ».

### 3.5. Import

Comme `BUNTU-PRD-26` § 2.4 : aperçu, doublons (même serveur et utilisateur, ou URL normalisée) avec Ignorer, Remplacer ou Importer en double, confirmation explicite, secret écrit dans le coffre, synchronisation. Une source `needs_credentials` demande son mot de passe. Une playlist jointe passe par le parseur M3U en flux.

### 3.6. Playlist `.m3u` sur clé USB

Le fichier passe par le parseur du `PRD-20`, sans copie intégrale : la clé peut être retirée ensuite. La source est marquée `file` : elle ne se rafraîchit pas seule, et l'action « Réimporter » redemande le fichier. Limite de taille celle du `PRD-06`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Enveloppe expirée ou altérée refusée. Message identique à Apple pour un code faux.
- Rien n'est importé sans confirmation.
- Ni le code ni le clair dans les journaux.

## 5. Critères d'Acceptation

1. Vecteurs de test générés par l'app Apple : déchiffrement correct, y compris un cas altéré et un cas expiré.
2. Import réel depuis un Mac vers une TV par clé USB.
3. Une playlist `.m3u` de la clé donne une source, et la clé peut être retirée ensuite.

## 6. Sources

- [Handling USB Storages](https://developer.samsung.com/smarttv/develop/guides/data-handling/handling-usb-storages.html)
- [Web Engine Specifications](https://developer.samsung.com/smarttv/develop/specifications/web-engine-specifications.html)
