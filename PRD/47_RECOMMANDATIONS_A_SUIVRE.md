# PRD 47 — Recommandations « À suivre »

- **ID :** `PRD-47`
- **Prérequis :** `PRD-24`, `PRD-26`, `PRD-27`, `PRD-32`
- **Origine Buntu :** `crates/cnd-core/src/recommend/`, `BUNTU-PRD-27`
- **Couches impactées :** `src/engine/recommend/`, `src/ui/`
- **Statut :** `[ ]`
- **Objectif :** proposer la suite logique sans parcourir tout le catalogue.

---

## 1. Contexte & Objectif

La carte « Film suivant » est pensée pour le salon. Le score et les sagas de Buntu fonctionnent localement. La partie assistée par un fournisseur IA n'est pas reprise.

## 2. Responsabilité

**Fait :** calcul des suggestions, rangée « À suivre », carte de fin de lecture, raison affichée.

**Ne fait pas :** données de fiche (`PRD-27`).

## 3. Spécifications

- Port de `recommend/scoring.rs` et `saga.rs` : suite de saga, épisode suivant, même réalisateur, même acteur, genre proche, reprise.
- Candidats bornés : même catégorie et métadonnées déjà en cache. Pas de parcours complet.
- Raison toujours affichée (« Suite de la saga · 2/4 »).
- « Pas intéressé » conservé par profil.
- Tables `sagas` et `saga_members` réduites aux sagas détectées localement. `llm_saga.rs` n'est pas porté.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Calcul dans le Worker, jamais pendant le démarrage d'une lecture.
- Vocabulaire de marque : « suggestions », pas « l'IA sait ce que vous aimez ».

## 5. Critères d'Acceptation

1. Cas de test de `recommend/` en fixtures de parité, hors LLM.
2. La carte de fin apparaît sans retarder la fin de lecture.

## 6. Sources

- Modules Buntu cités en en-tête.
