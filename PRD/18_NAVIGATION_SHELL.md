# PRD 18 — Coquille de navigation et premier lancement

- **ID :** `PRD-18`
- **Prérequis :** `PRD-11`, `PRD-13`, `PRD-14`, `PRD-15`
- **Origine Buntu :** `apple/Sources/CndIptvKit/App/`, `BUNTU-PRD-21` § 2.3
- **Couches impactées :** `src/ui/shell/`
- **Statut :** `[ ]`
- **Objectif :** premier écran utile sous le budget de lancement, pile de navigation cohérente avec Retour.

---

## 1. Contexte & Objectif

L'app a besoin d'une structure commune pour que chaque écran ne réinvente pas Retour, le chargement ou les erreurs.

## 2. Responsabilité

**Fait :** menu principal, pile d'écrans, chargement différé, écran de premier lancement, fenêtre de sortie.

**Ne fait pas :** contenu des écrans.

## 3. Spécifications

### 3.1. Sections

Menu latéral replié : **Live TV, Guide, Films, Séries, Recherche, Sources, Réglages**. « Mes fichiers » s'ajoute si le `PRD-44` est livré. Dernière section ouverte conservée. L'accueil est Live TV s'il existe une source, sinon l'écran de bienvenue.

### 3.2. Pile

Retour dépile. En bas de pile, le focus revient d'abord au menu, puis Retour ouvre la fenêtre de sortie (`PRD-11`). Le lecteur plein écran est un niveau de pile.

### 3.3. Premier lancement

1. Bienvenue : « Buntu lit vos sources. Il ne fournit aucun contenu. »
2. Acceptation des conditions d'utilisation (`PRD-04`).
3. Ajout d'une source (`PRD-35`).

### 3.4. Chargement

Écran d'accueil rendu avec les données en cache, avant le réseau. Code de chaque section chargé à la première ouverture. État vide et état d'erreur partout, avec une action.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun écran ne gère Retour en dehors de la pile, sauf une fenêtre modale.
- Rien ne bloque le premier rendu sur le réseau.

## 5. Critères d'Acceptation

1. Lancement sous le budget, avec et sans réseau.
2. Retour depuis n'importe quel écran rejoint l'accueil, puis la fenêtre de sortie.

## 6. Sources

- [Terminating Applications](https://developer.samsung.com/smarttv/develop/guides/fundamentals/terminating-applications.html)
- [Launch Time Optimization](https://developer.samsung.com/smarttv/develop/guides/application-performance-improvement/launch-time-optimization.html)
