# PRD 04 — Conformité d'un lecteur de type IPTV

- **ID :** `PRD-04`
- **Prérequis :** `PRD-01`
- **Origine Buntu :** `docs/BRANDING_BUNTU.md` § 1.2, § 7.2, § 7.3
- **Couches impactées :** règles produit, textes, données de certification
- **Statut :** `[ ]`
- **Objectif :** connaître la position de Samsung **avant** d'investir dans le développement complet.

---

## 1. Contexte & Objectif

Ce PRD ne vaut pas avis juridique. Aucune page publique lue ne dit si Samsung accepte un lecteur de playlists fournies par l'utilisateur `[NON DOC]`. En revanche, le statut de vendeur limite déjà la distribution : un Public Seller publie **seulement aux États-Unis**, et publier ailleurs demande un contrat avec Samsung `[DOC]`. Pour un produit écrit d'abord en français, c'est le premier risque commercial.

## 2. Responsabilité

**Fait :** règles de contenu, position de produit, questions à poser à Samsung, conditions de publication.

**Ne fait pas :** mécanique de dépôt (`PRD-52`), filtrage dans l'app (`PRD-45`), signature (`PRD-50`).

## 3. Spécifications

### 3.1. Ce qui est établi

| Point | État |
| :--- | :--- |
| Violence, pornographie, langage injurieux | Interdits `[DOC]` |
| Classement | Auto-évaluation soumise à l'accord de Samsung : Tous, 4+, 12+, 16+, 18+ `[DOC]` |
| 18+ | Non publiable en Corée, Arabie saoudite, Émirats `[DOC]` |
| Corée, Brésil | Certificat de classement demandé `[DOC]` |
| Politique de confidentialité | URL requise si l'app collecte des données, en anglais `[DOC]` |
| UE | Déclaration de conformité EAA, et numéro DSA dans certains cas `[DOC]` |
| Proxy résidentiel | Interdit `[DOC]` |
| Règle sur les lecteurs IPTV | `[NON DOC]` |
| « Samsung Apps Certification Guide », section 3 (« Banned Content ») | Cité, non trouvé |
| Conditions d'utilisation du Seller Office | Non lues : elles apparaissent à l'inscription |

La boutique mobile Galaxy Store demande pour certains contenus les accords des titulaires de droits, mais cette règle ne vise pas expressément la TV. Un vendeur d'abonnements affirme, sans source, que Samsung a retiré des applications IPTV au Royaume-Uni et en Italie à partir de 2023 `[COMM]`. L'éditeur d'un lecteur concurrent écrit que les lecteurs IPTV dédiés sont presque absents de la boutique Tizen `[COMM]`. Ces deux indices ne prouvent rien, mais ils justifient de poser la question à Samsung avant de construire.

### 3.2. Actions avant le développement complet

1. Créer un compte Seller Office et **lire les conditions d'utilisation** à l'inscription.
2. Obtenir le « Samsung Apps Certification Guide » et sa section 3.
3. Écrire à Samsung (1:1 Q&A, ou Content Manager s'il y en a un). Les messages prêts à envoyer sont dans [`docs/SAMSUNG_QUESTIONS.md`](../docs/SAMSUNG_QUESTIONS.md). Ils couvrent au moins ces questions :
   - Un lecteur sans contenu, dont les sources sont saisies par l'utilisateur, est-il accepté ?
   - Dans quels pays, et sous quel statut de vendeur ?
   - Une personne physique peut-elle devenir Partner Seller, ou faut-il une société ?
   - Quelles preuves attend-on ? Quel classement pour une app sans contenu propre ?
4. Faire relire la position par un juriste, avec la vérification de la marque déjà prévue (`docs/BRANDING_BUNTU.md` § 8 de Buntu).

### 3.3. Position du produit

- Buntu ne fournit, n'héberge et ne vend aucun contenu.
- Aucune source, aucun lien, aucun répertoire de sources dans l'app, le paquet ou la fiche.
- L'app s'ouvre sur un écran vide qui invite à ajouter une source (`PRD-35`).
- Termes à préférer : « vos sources », « lecteur ». Termes interdits : « IPTV illimitée », « gratuit », « sans abonnement », « toutes les chaînes ».
- Le mot « IPTV » n'apparaît ni dans le nom ni dans les visuels. Le nom technique `cnd-iptv` reste interne.
- Conditions d'utilisation dans l'app : l'utilisateur déclare détenir le droit d'accéder à ses sources.
- Pas d'option d'import de sources par un tiers sans action de l'utilisateur.

### 3.4. Contenu pour adultes

Une source peut contenir des catégories pour adultes. Ces catégories sont **masquées par défaut**, et leur affichage exige un code PIN (`PRD-45`). Le classement proposé à Samsung se décide après sa réponse.

### 3.5. Compte de test

La certification exige un accès à toutes les fonctions et au moins un compte de test par groupe de modèles `[DOC]`. Il faut donc une **source de démonstration hébergée par l'éditeur** : contenus libres de droits, M3U et compte de type Xtream, avec guide TV. Elle figure dans les notes de test, jamais dans l'app.

### 3.6. Confidentialité

Buntu ne collecte rien. Une politique de confidentialité reste publiée, car les URL et les clés saisies peuvent être transmises aux services choisis par l'utilisateur. Le DUID n'est pas lu ; si un identifiant est un jour nécessaire, `getUuid` passe avant le DUID.

### 3.7. Si Samsung refuse

| Situation | Conséquence |
| :--- | :--- |
| Refus de principe | Pas de boutique. Installation en mode développeur pour un usage personnel, sans diffusion au public |
| Public Seller seul | Publication aux États-Unis seulement |
| Partenariat accordé | Pays choisis avec le Content Manager |

## 4. Contraintes Non-Négociables (Guardrails IA)

- Aucune capture ni donnée de test n'utilise une chaîne commerciale ou une affiche protégée.
- Pas de procédé de contournement : pas de portail qui pousse la playlist à la TV par son adresse MAC, pas de mode caché pour la revue.
- La version soumise à Samsung est la version distribuée.

## 5. Critères d'Acceptation

1. La réponse de Samsung, ou l'absence de réponse, est archivée avec sa date.
2. Le statut de vendeur visé et les pays sont écrits dans l'index.
3. La source de démonstration est en ligne et ses droits sont documentés.
4. La politique de confidentialité est publiée en anglais et en français.

## 6. Sources

- [Becoming a Seller Office member](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-seller-office-member.html)
- [Becoming a partner](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-partner.html)
- [Age Rating](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/age-rating.html)
- [Application Registration Q&A](https://developer.samsung.com/tv-seller-office/faq/application-registration.html)
- [Entering Application Information](https://developer.samsung.com/tv-seller-office/guides/applications/entering-application-information.html)
- [Launch Checklist](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/launch-checklist.html)
- [Galaxy Store : IP Infringement Checklist](https://developer.samsung.com/galaxy-store/ip-infringement.html) (mobile, pas TV)
- `[COMM]` [OTTV, vendeur d'abonnements, sans source pour son affirmation](https://www.ottv.org/docs/setup-iptv-on-samsung-tv), [IPTV One, éditeur d'un lecteur concurrent](https://www.iptv-one.app/en/blog/best-iptv-player-samsung-tv-v2)
- [TechCrunch, 3 août 2026](https://techcrunch.com/2026/08/03/samsung-bans-smart-tv-apps-that-share-users-internet-connections-with-strangers/)
