# PRD 30 — Surveillance et reconnexion

- **ID :** `PRD-30`
- **Prérequis :** `PRD-11`, `PRD-28`, `PRD-29`
- **Origine Buntu :** `BUNTU-PRD-08` § 3.3, `CHANGELOG.md` (Diagnostic)
- **Couches impactées :** `src/player/resilience/`
- **Statut :** `[ ]`
- **Objectif :** détecter un flux figé sous 4 secondes et tenter 3 reconnexions avec attente progressive.

---

## 1. Contexte & Objectif

Un direct tombe souvent. Le comportement de Buntu est connu et testé : on le reprend avec les moyens de la TV.

## 2. Responsabilité

**Fait :** détecter un blocage, classer l'erreur, tenter de reprendre, arrêter quand c'est inutile.

**Ne fait pas :** bascule vers un autre flux (`PRD-34`), affichage (`PRD-32`).

## 3. Spécifications

- **Détection :** `oncurrentplaytime` n'avance plus pendant 4 s à l'état `PLAYING`, hors pause et hors tampon annoncé ; ou `onerror`.
- **Reconnexion :** 3 essais à 1 s, 2 s et 4 s, par `close` puis `open`, à la dernière position pour la VOD et au direct pour le live.
- **Réseau :** `webapis.network` remplace `NWPathMonitor`. Sans réseau, on attend son retour au lieu de consommer les essais.
- **Classement :** HTTP 403, 404, 429, DNS et délai si AVPlay les révèle par `onerrormsg`. Sinon le message reste générique et honnête, avec le code dans les journaux. Une erreur inutile à répéter (format, compte expiré) arrête les essais, comme dans Buntu.
- **Limite de connexions :** un refus probablement dû à `max_connections` produit le message : « Votre abonnement n'autorise que N connexions. »

## 4. Contraintes Non-Négociables (Guardrails IA)

- Pas de boucle infinie : les essais sont bornés et visibles.
- Aucun essai quand l'app est cachée.

## 5. Critères d'Acceptation

1. Le test `VI-GP-02` passe : message adapté quand le réseau ou la vidéo ne sont pas disponibles.
2. Une coupure de 10 s du réseau reprend la lecture sans action.
3. Un flux définitivement mort arrête les essais et propose de revenir.

## 6. Sources

- [Video checklist](https://developer.samsung.com/smarttv/develop/development-checklist/video.html)
- [AVPlay API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/avplay-api.html)
- [Network API](https://developer.samsung.com/smarttv/develop/api-references/samsung-product-api-references/network-api.html)
