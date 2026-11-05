# PRD 42 — Accès au stockage USB

- **ID :** `PRD-42`
- **Prérequis :** `PRD-03`, `PRD-05`
- **Origine Buntu :** sans équivalent
- **Couches impactées :** `src/platform/removable/`
- **Statut :** `[ ]`
- **Objectif :** lister une clé USB et lire ses fichiers, en lecture seule.

---

## 1. Contexte & Objectif

La clé USB est le seul moyen simple d'apporter un fichier sur la TV : pas de glisser-déposer ni de sélecteur de fichiers. Deux fonctions en dépendent : l'import d'un partage et les médias locaux.

## 2. Responsabilité

**Fait :** détecter les supports, parcourir un dossier, ouvrir un fichier en lecture par morceaux.

**Ne fait pas :** interpréter un fichier (`PRD-43`, `PRD-44`).

## 3. Spécifications

- Supports dont le nom commence par `removable_` : `tizen.filesystem.listStorages()`, `addStorageStateChangeListener()` `[DOC]`.
- Privilège `http://tizen.org/privilege/filesystem.read` `[DOC]`. La référence 10.0 évoque aussi `mediastorage` et `externalstorage` : à trancher par `S15`.
- L'API a été revue à Tizen 5.0 (`openFile`, `FileHandle`) `[DOC]`. Le guide USB utilise `resolve`. La mise en œuvre choisit l'API qui fonctionne sur le palier le plus ancien et la recouvre derrière `RemovableStoragePort`.
- Retrait de la clé pendant une opération : erreur propre, retour à l'écran précédent.
- Fichiers cachés ignorés. Liens symboliques non suivis.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune écriture sur un support externe.
- `filesystem.write` n'est déclaré que si un autre PRD en a besoin.

## 5. Critères d'Acceptation

1. Branchement, parcours et retrait d'une clé sur deux TV.
2. Lecture d'un fichier de 500 Mo par morceaux sans le charger.

## 6. Sources

- [Handling USB Storages](https://developer.samsung.com/smarttv/develop/guides/data-handling/handling-usb-storages.html)
- [File Operations](https://developer.samsung.com/smarttv/develop/guides/data-handling/managing-file-operations.html)
- [File System (Tizen)](https://github.com/Samsung/tizen-docs/blob/master/docs/application/web/guides/data/file-system.md)
