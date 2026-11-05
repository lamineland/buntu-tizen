# PRD 45 — Profils et contrôle parental

- **ID :** `PRD-45`
- **Prérequis :** `PRD-04`, `PRD-07`, `PRD-18`, `PRD-24`, `PRD-25`
- **Origine Buntu :** `crates/cnd-core/src/db/profile_repo.rs`, `BUNTU-PRD-20`
- **Couches impactées :** `src/engine/profiles/`, `src/ui/screens/profiles/`
- **Statut :** `[ ]`
- **Objectif :** aucune catégorie pour adultes visible sans PIN. Ce point conditionne la soumission à Samsung.

---

## 1. Contexte & Objectif

Une app sans contenu propre peut quand même afficher du contenu pour adultes venant d'une source, alors que Samsung interdit la pornographie dans les applications `[DOC]`. Le masquage par défaut est donc une condition du `PRD-04`.

## 2. Responsabilité

**Fait :** profils, PIN, profil Enfants, catégories et chaînes masquées, ordre personnalisé, dossiers de favoris, règles de `visibilityFilter`.

**Ne fait pas :** filtrage bas niveau (`PRD-24`).

## 3. Spécifications

### 3.1. Détection

Port de `is_adult_category` : `adult`, `adults`, `adulte`, `adultes`, `xxx`, `+18`, `18+`, `porn`, `porno`, `erotic`, `erotique`, `nsfw`, `for adults`, `pour adultes`, avec les mêmes règles de limite de mot. L'indicateur `is_adult` de Xtream est pris en compte quand il existe. La détection repose sur les noms de catégories : elle est imparfaite, et l'app le dit.

### 3.2. Masquage

`adultContentVisible = false` par défaut. Les éléments détectés sont absents des listes, compteurs, recherche, guide, miroirs, recommandations et assistant. Le déblocage exige le PIN et dure jusqu'à la fermeture de l'app.

### 3.3. PIN

Sans Face ID ni Touch ID. PIN de 4 à 6 chiffres, saisi par une grille à l'écran, car certaines télécommandes n'ont pas de chiffres. Hachage PBKDF2-HMAC-SHA256 salé dans le coffre (`PRD-07`). Buntu utilise 600 000 itérations : à mesurer sur TV, et le nombre d'itérations est conservé avec le hachage. Attente croissante après 5 échecs. Récupération : réinitialisation complète de l'app (`PRD-41`).

### 3.4. Profils

Création, avatar, profil Enfants qui ne voit que les catégories autorisées, sans Sources, Réglages ni assistant distant. Sortie du profil par PIN. Profil actif mémorisé au relancement.

### 3.5. Bouquets

`hide_group`, `hide_item`, `set_custom_order`, `favorite_folders`. L'édition se fait par un mode dédié à la télécommande : pas de glisser-déposer.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Le PIN n'apparaît ni en base, ni dans les journaux.
- Le filtre s'applique dans le moteur.
- Les captures de boutique ne montrent jamais ce déblocage.

## 5. Critères d'Acceptation

1. Cas de test de `profile_repo.rs` en fixtures de parité.
2. Un jeu de catégories adultes n'apparaît dans aucun écran sans PIN.
3. Relancer l'app ne fait pas sortir d'un profil Enfants.

## 6. Sources

- [Age Rating](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/age-rating.html)
- [Application Registration Q&A](https://developer.samsung.com/tv-seller-office/faq/application-registration.html)
- [Remote Control](https://developer.samsung.com/smarttv/develop/guides/user-interaction/remote-control.html)
