# Buntu pour Samsung TV (Tizen)

Spécifications de la version Samsung Smart TV de [Buntu](https://github.com/lamineland/buntu), le lecteur de médias natif pour les plateformes Apple.

Buntu est un lecteur : il ne fournit aucun contenu, il lit les sources que l'utilisateur ajoute. Cette règle vaut aussi sur la TV.

> **État du dépôt : documentation seulement.** Aucun code d'application n'a été écrit. Le développement commence par les preuves techniques du [`PRD-03`](PRD/03_SPIKES_VALIDATION_TV_REELLE.md).

## Où commencer

| Document | Rôle |
| :--- | :--- |
| [`PRD/00_INDEX_ROADMAP.md`](PRD/00_INDEX_ROADMAP.md) | Liste des 52 PRD, dépendances, ordre de construction, décisions ouvertes |
| [`PRD/01_PERIMETRE_MATRICE_PORTAGE.md`](PRD/01_PERIMETRE_MATRICE_PORTAGE.md) | Ce qui est porté, remplacé ou abandonné par rapport à Buntu sur Apple |
| [`docs/TIZEN_FACTS.md`](docs/TIZEN_FACTS.md) | Faits Tizen vérifiés, avec leur source et leur niveau de certitude |

## Principes

- **Un PRD, une responsabilité.** Chaque PRD fixe ce qu'il fait, ce qu'il ne fait pas et de quels PRD il dépend. Un PRD ne dépend que de PRD de numéro inférieur : la numérotation est l'ordre de construction.
- **Pas de Rust ni de SQLite sur la TV.** Le moteur `cnd-core` sert de référence fonctionnelle et il est réécrit en TypeScript (voir [`PRD-06`](PRD/06_STOCKAGE_ECHELLE_CATALOGUE.md)).
- **Local d'abord.** Pas de serveur Buntu, pas de compte, pas de pistage, comme sur Apple.
- **Ce qui n'est pas documenté se mesure.** Les PRD séparent les faits documentés par Samsung, les hypothèses et les retours de la communauté.

## Licence

À définir au début du développement. Buntu est sous `MIT OR Apache-2.0`.
