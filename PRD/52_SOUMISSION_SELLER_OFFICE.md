# PRD 52 — Soumission au Seller Office

- **ID :** `PRD-52`
- **Prérequis :** `PRD-04`, `PRD-13`, `PRD-17`, `PRD-45`, `PRD-50`, `PRD-51`
- **Origine Buntu :** `docs/BRANDING_BUNTU.md` § 7.3, § 8
- **Couches impactées :** dossier de publication, compte Seller Office
- **Statut :** `[ ]`
- **Objectif :** un dossier complet dès le premier dépôt. Une information de test manquante est une cause majeure de refus `[DOC]`.

---

## 1. Contexte & Objectif

Ce PRD exécute le dépôt, une fois la position de Samsung connue (`PRD-04`). Toute action dans le compte Seller Office reste **une décision de l'utilisateur** : soumettre publie.

## 2. Responsabilité

**Fait :** fichiers de la fiche, App UI Description, comptes de test, dépôt, défauts, mises à jour.

**Ne fait pas :** politique de contenu (`PRD-04`), paquet (`PRD-50`), recette (`PRD-51`).

## 3. Spécifications

### 3.1. Parcours

Créer l'application et son App ID, envoyer le paquet (pré-test automatique), renseigner images, titre, description, service, pays, facturation, fonctions et informations de test, demander la sortie depuis Applications > Distribute en choisissant les groupes de modèles (nouveau pré-test), attendre la vérification de Samsung `[DOC]`.

### 3.2. Fichiers

| Élément | Exigence `[DOC]` |
| :--- | :--- |
| Logo | 1920×1080, PNG 32 bits transparent, 300 Ko au plus |
| Fond de l'icône | 1920×1080, PNG 24 bits ou JPG, 300 Ko au plus |
| Icône | 512×423, PNG, 300 Ko au plus |
| Fond de la fiche | 1920×1080, JPG, 2 Mo au plus |
| Captures | 4 obligatoires, JPEG, moins de 500 Ko |
| Description | 4 000 caractères au plus, au moins 3 mots-clés par langue |

Les pages Samsung diffèrent pour les captures (1280×720 ou 1920×1080 selon le Launch Checklist, 1920×1080 seulement pour Entering Application Information). **Toujours fournir 1920×1080.**

Captures prises sur la source de démonstration. Aucune chaîne commerciale, aucune affiche protégée.

### 3.3. App UI Description

Modèle Samsung non modifiable : page de titre, historique, structure, cas d'usage, menus et fonctions, touches de la télécommande, langues `[DOC]`. Il explique que l'app est un lecteur, comment ajouter la source de démonstration, et où se trouvent le PIN et le masquage adulte.

### 3.4. Informations de test

Au moins un compte de test par groupe de modèles, adresse de contact, pays concernés `[DOC]`. Les accès restent valides pendant toute la revue.

### 3.5. Déclarations

Catégorie Vidéo, classement (`PRD-04`), URL de politique de confidentialité en anglais, informations de vendeur, déclaration EAA pour l'UE, numéro DSA si demandé. Pas de publicité dans l'app : le déclarer ainsi `[DOC]`.

### 3.6. Après dépôt

- Délai non garanti `[DOC]`. Un ticket 1:1 Q&A permet de demander une revue plus rapide `[DOC]`.
- Défauts `A`, `B`, `C` : `A` bloque, `B` est à corriger de préférence `[DOC]`.
- Sortie possible pour les seuls groupes réussis `[DOC]`.
- Toute modification de code demande une nouvelle version `[DOC]`.
- Mise à jour progressive : 3 %, puis 10 % après deux semaines, puis 100 % une semaine plus tard, hors groupes 2020 et antérieurs `[DOC, résumé]`.

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucun agent ne soumet, ne publie ni ne signe de conditions sans accord explicite de l'utilisateur.
- Aucun texte de fiche ne promet des chaînes ou un accès gratuit.
- Le dossier décrit la version réellement envoyée.

## 5. Critères d'Acceptation

1. Le dossier passe le pré-test.
2. La liste du Launch Checklist est cochée.
3. Le résultat de Samsung et chaque défaut sont archivés dans `docs/store/`.

## 6. Sources

- [Application Publication Process](https://developer.samsung.com/tv-seller-office/application-publication-process.html)
- [Launch Checklist](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/launch-checklist.html)
- [Entering Application Information](https://developer.samsung.com/tv-seller-office/guides/applications/entering-application-information.html)
- [Application UI Description](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/application-ui-description.html)
- [Certification Process Q&A](https://developer.samsung.com/tv-seller-office/faq/certification-process.html)
- [Certification Defects Q&A](https://developer.samsung.com/tv-seller-office/faq/certification-defects.html)
- [Distributing Applications](https://developer.samsung.com/tv-seller-office/guides/applications/distributing-application.html)
