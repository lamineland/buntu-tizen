# PRD 46 — Assistant de chaînes

- **ID :** `PRD-46`
- **Prérequis :** `PRD-07`, `PRD-09`, `PRD-16`, `PRD-23`, `PRD-25`, `PRD-36`
- **Origine Buntu :** `crates/cnd-core/src/ai/`, `BUNTU-PRD-13`
- **Couches impactées :** `src/engine/assistant/`, `src/ui/screens/assistant/`
- **Statut :** `[ ]`
- **Objectif :** commande locale résolue en moins de 500 ms `[CIBLE]`, sans réseau.

---

## 1. Contexte & Objectif

La palette `⌘K` n'existe pas sur TV et taper une phrase à la télécommande est lent. Le mode local garde sa valeur. La voix serait le vrai gain, mais reste à évaluer.

## 2. Responsabilité

**Fait :** résolution locale, appel au fournisseur de l'utilisateur, validation de la réponse, action.

**Ne fait pas :** saisie de la clé (`PRD-41`).

## 3. Spécifications

### 3.1. Résolution locale

Port de `local_matcher.rs` : nom normalisé, numéro de chaîne, suivante, précédente, retour, programme en cours. Seuil de confiance de 0,85, repris de `BUNTU-PRD-13`.

### 3.2. Fournisseur

Port de `providers.rs`, `prompt.rs`, `candidates.rs` : Gemini, compatible OpenAI (dont Ollama et LM Studio sur le réseau local) et Anthropic. Mêmes plafonds : 40 candidats, 8 s de délai, sortie JSON validée, et tout `item_id` doit appartenir aux candidats. Noms de modèle modifiables.

- Appels sous réserve de `S1`. Une API peut demander un en-tête propre aux appels depuis un navigateur : à tester par fournisseur.
- Clé dans le coffre, sans clé intégrée.
- Le premier usage distant demande un accord explicite : les noms de chaînes et la phrase partent chez le fournisseur choisi.

### 3.3. Accès

Bouton dans le menu, touche de couleur en raccourci facultatif.

### 3.4. Voix

`webapis.voiceinteraction` existe : Tizen 6.0 et plus, niveau Public `[DOC]`. Il expose des rappels de navigation et de lecture pour l'assistant de la TV, pas une dictée libre. Hors périmètre v1.1 : une étude séparée dira s'il sert à « mets France 2 ». `smartcontroller.microphone` est réservé aux partenaires `[DOC]`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Seule une chaîne du catalogue visible peut être lancée.
- Sans fournisseur, l'assistant fonctionne en local.
- Pas d'assistant distant pour un profil Enfants.

## 5. Critères d'Acceptation

1. Cas de test de `src/ai/tests.rs` en fixtures de parité.
2. Une réponse hors candidats est refusée.
3. Repli local si le fournisseur échoue ou tarde.

## 6. Sources

- [VoiceInteraction API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/voiceinteraction-api.html)
- [Launch Checklist](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/launch-checklist.html)
