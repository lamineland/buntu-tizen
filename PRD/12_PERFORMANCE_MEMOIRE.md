# PRD 12 — Budgets de performance et de mémoire

- **ID :** `PRD-12`
- **Prérequis :** `PRD-03`, `PRD-05`
- **Origine Buntu :** valeurs SLA des PRD Buntu, `docs/BRANDING_BUNTU.md` § 2 (Gaaw)
- **Couches impactées :** build, tests de performance, règles d'interface
- **Statut :** `[ ]`
- **Objectif :** fixer des budgets chiffrés, les mesurer sur la TV la plus faible et bloquer une version qui les dépasse.

---

## 1. Contexte & Objectif

Samsung ne publie pas de limite de mémoire par application `[NON DOC]`, mais la mémoire est partagée et le système tue des applications sous pression `[DOC]`. Les promesses chiffrées de Buntu (50 ms, 50 000 éléments) ne valent que pour Apple : elles ne sont pas reprises sans mesure.

## 2. Responsabilité

**Fait :** budgets, méthode de mesure, règles communes.

**Ne fait pas :** optimiser un écran précis, plan de QA général (`PRD-51`).

## 3. Spécifications

### 3.1. Budgets

| Mesure | Limite | Nature |
| :--- | :--- | :--- |
| Lancement à froid jusqu'à la première page | 10 s | `[DOC]` obligatoire |
| Lancement à froid | 5 à 7 s | `[DOC]` recommandé, notre objectif |
| Réponse à une action | 30 s | `[DOC]` plafond certification |
| Réponse au déplacement du focus | 100 ms | `[CIBLE]` |
| Zapping jusqu'à l'image | 2 s | `[CIBLE]`, selon le flux |
| Page de 50 éléments | 100 ms | `[CIBLE]` |
| Recherche | 300 ms | `[CIBLE]` |
| Mémoire du processus | Fixée après `S10` | `[CIBLE]` provisoire : 300 Mo |
| Paquet `.wgt` | 5 Mo | `[CIBLE]` |
| JavaScript initial | 500 Ko non compressé | `[CIBLE]` |

### 3.2. Règles

- **Listes :** toute liste de plus de 100 éléments est virtualisée `[DOC, résumé]`.
- **Images :** une image FHD décodée coûte environ 8,3 Mo `[DOC]`. Affiches redimensionnées à la taille d'affichage quand le serveur le permet, pas plus de 60 images décodées `[CIBLE]`, annulation hors écran, fond neutre en attente.
- **Animations :** `transform` et `opacity` seulement, pas de flou ni d'ombre animée.
- **DOM :** réemploi des nœuds, 1 500 nœuds au plus par écran `[CIBLE]`.
- **Arrière-plan :** libérer images et grilles quand l'app est cachée `[DOC, résumé]`.
- **Démarrage :** ressources locales, pas d'appel réseau avant la première page, code des écrans chargé à la demande `[DOC]`.
- **Worker :** analyse et indexation par tranches, avec un temps de repos pour le décodage vidéo si une lecture est en cours.

### 3.3. Mesure

Sur la TV la plus faible du palier Cœur. Outils de débogage par `sdb`, chronométrage dans une version de mesure, 5 répétitions, médiane et pire cas. Résultats dans `docs/perf/`.

### 3.4. Catalogue de référence

Fixture générée sans contenu réel : 10 000 chaînes, 60 000 films, 5 000 séries, 500 catégories, et un XMLTV de 200 Mo décompressé.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune version n'est soumise avec un budget `[DOC]` dépassé.
- Un budget `[CIBLE]` ne devient une promesse publique qu'après mesure.

## 5. Critères d'Acceptation

1. Les valeurs provisoires sont remplacées après `S8` et `S10`.
2. La taille du paquet est contrôlée au build.
3. Une navigation de 30 minutes sur le catalogue de référence ne fait pas croître la mémoire de plus de 10 % `[CIBLE]`.

## 6. Sources

- [Memory Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-memory-optimization-guide.html)
- [Launch Time Optimization](https://developer.samsung.com/smarttv/develop/guides/application-performance-improvement/launch-time-optimization.html)
- [Application Performance Improvement](https://developer.samsung.com/smarttv/develop/guides/application-performance-improvement/application-performance-improvement.html)
- [Mandatory Features](https://developer.samsung.com/smarttv/develop/development-checklist/mandatory-features.html)
- [Common checklist](https://developer.samsung.com/smarttv/develop/development-checklist/common.html)
- [Web App Flash Storage Optimization](https://developer.samsung.com/smarttv/develop/guides/web-app-flash-storage-optimization-guide.html)
