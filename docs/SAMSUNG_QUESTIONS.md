# Questions à poser à Samsung

Brouillon du 5 novembre 2026, **non envoyé**. Il exécute le `PRD-04` § 3.2. Les messages sont en anglais pour éviter un aller-retour de traduction chez Samsung.

## Avant d'envoyer

1. **Créer le compte Seller Office** sur [seller.samsungapps.com/tv](https://seller.samsungapps.com/tv). Seul l'utilisateur peut le faire : l'inscription fait accepter des conditions.
2. **Lire ces conditions** et chercher le « Samsung Apps Certification Guide ». Retirer du message A ce qu'ils règlent déjà.
3. **Remplacer les champs `[...]`** du message A. Ils dépendent de la décision `D1`.
4. **Ouvrir un ticket 1:1 Q&A par message.** A d'abord, B ensuite ou en parallèle. Limite de longueur du ticket inconnue : si le message est trop long, joindre le texte en fichier.

## Par où passer

Aucune adresse de contact publique ni formulaire avant inscription n'a été trouvé. Les pages [Becoming Seller Office Members](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-seller-office-member.html) et [Becoming Partners](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-partner.html) donnent ce parcours :

- Le 1:1 Q&A est offert aux membres du Seller Office. Elles ne disent pas qu'il faut être partenaire, sans l'exclure formellement. Les guides du 1:1 Q&A n'ont pas été lus.
- L'adresse du Content Manager se demande par le 1:1 Q&A. Un Content Manager peut aussi être attribué selon le pays saisi à l'inscription.
- La demande de partenariat se fait dans `Membership > Partnership`, menu réservé au manager du groupe. Elle demande l'adresse du Content Manager, et un contrat hors ligne avec Samsung est exigé par la page Membership.

## Message A : distribution et contenu

Bloquant. À envoyer en premier.

**Subject:** Pre-development question: TV media player with user-supplied sources

Hello,

I am planning a Samsung TV version of Buntu, a media player I am building for Apple platforms. No Tizen development has started. Before investing in it, I would like to know whether Samsung would accept this type of app, and under what conditions.

**The app**

- It is a player only. It does not provide, host or sell any content.
- It ships with no sources, links or source directory. On first launch it shows an empty screen asking the user to add their own source.
- Users add their own M3U playlists, servers using the Xtream Codes API, and XMLTV guides. This is commonly called an IPTV player. I use that term here so your answer applies to the real product, although the app name and store listing would not use it.
- Users must declare that they hold the right to access their sources.
- Because users choose their sources, a source may contain adult categories. These are hidden by default and need a PIN to display.
- Some servers only offer plain `http://`, so the app would accept them with a warning.
- There are no ads, no user accounts and no publisher server. The publisher collects no user data.

**Publisher:** [full name or company name, country of residence or registration]
**Target countries:** [list]

**Questions**

1. Does Samsung accept this type of app on the TV store? If it depends on conditions, which ones?
2. Which written rules apply? The Age Rating page refers to section 3 of the "Samsung Apps Certification Guide", which I could not find. Could you send it or tell me where to get it?
3. Does the answer differ by country? Are any of my target countries excluded for this type of app?
4. Would this app be accepted from a Public Seller in the United States, or does this type of app need a partnership in every country?
5. Can an individual become a Partner Seller, or is a registered company required? What are the criteria, the costs and the usual timeline for the offline contract?
6. Who is the Content Manager for [country or region]? Please share their email address so I can use the Membership > Partnership request.
7. What proof does Samsung expect from a player with no content of its own? For example: terms of use, a rights statement, or a process for handling complaints.
8. What happens after publication if a rights holder complains about what a user plays with the app?
9. Which age rating fits an app with no content of its own, given the PIN-protected adult categories described above?
10. Certification requires test accounts. Is a publisher-hosted demo source with royalty-free content, available as M3U and Xtream-type access with a TV guide, acceptable for this?
11. May the store listing name the supported formats, such as M3U and Xtream Codes API?
12. Is accepting plain `http://` sources a problem for certification?
13. For EU distribution, what is required from a publisher of a free app with no ads, for both the European Accessibility Act declaration and the DSA number?

If some of this belongs with a Content Manager, please tell me and I will take it up with them. A written answer would help me decide whether to start development.

Thank you,
[name]

## Message B : questions techniques

Les réponses de Samsung complètent les preuves du `PRD-03` sans les remplacer : seul un test sur TV réelle compte comme preuve. Les questions 1 à 5 importent le plus.

**Subject:** Technical questions before starting a Tizen web app (.wgt)

Hello,

I am planning a packaged Tizen web app that plays media from servers chosen by the user. I could not find answers to these points in the documentation. Please answer per TV model year when the behaviour differs. I would target 2021 models and later.

1. **CORS.** The CORS guide requires the header on the server side. Many media servers do not send it. In a packaged `.wgt` with `<access origin="*" subdomains="true"/>`, can `fetch` or XHR read a response from a server that sends no CORS headers? If not, what is the supported method? Is `tizen.download` suitable, and is it available on all TV models?
2. **Plain HTTP.** Can `fetch` and AVPlay use `http://` URLs from a packaged app? Are there restrictions by model year or at certification?
3. **Live MPEG-TS.** Does AVPlay officially support an endless MPEG-TS stream over HTTP, such as `http://host/stream.ts`? The documentation lists TS as a container and as an HLS segment format, but does not cover this case.
4. **Request headers.** The AVPlay API lists only `COOKIE` and `USER_AGENT` for `setStreamingProperty`. Is there a supported way to send `Referer` or other headers on media requests? If a local `tizen:service` is the only way, would a partner be allowed to use it for this purpose, given the 2026 ban on residential proxy SDKs?
5. **Secure storage.** The API index marks `keymanager` as mandatory on TV, but the Launch Checklist and Registering Applications say it is no longer available. Which is correct? Is `webapis.widgetdata` the recommended store for user credentials? Which algorithm and key scope does it use, and is the data removed on uninstall?
6. **Storage limits.** What is the IndexedDB quota on TV? What does the "partial" coverage of `unlimitedstorage` mean? Is there a recommended limit on writes to flash memory? The app may store about 100,000 catalogue items.
7. **Memory.** Is there a memory limit per web app, by model year, beyond which the app is terminated?
8. **Video overlay.** Is displaying HTML elements above the AVPlay video area supported, for example player controls and subtitles?
9. **Parallel playback.** Can two AVPlay instances be in the `PLAYING` state at once? What are the conditions for `IN_APP_MULTIVIEW`, and is it available to Public Sellers?
10. **Tools on Mac.** The TV Extension prerequisites mention Intel-based Macs. Are the CLI, `sdb` and the TV emulator supported on Apple Silicon?

Thank you,
[name]

## Ce que chaque réponse décide

| Question | Décide | Si la réponse est négative |
| :--- | :--- | :--- |
| A1, A3, A4 | Porte Distribution, `D1` | `PRD-04` § 3.7 : pas de boutique, ou États-Unis seulement |
| A5, A6 | `D1` : personne ou société, statut visé | Créer une société, ou rester Public Seller |
| A2, A7, A8 | Conditions d'utilisation, `PRD-04` § 3.3 | Ajouter les preuves ou le processus demandés |
| A9 | Classement, `PRD-04` § 3.4, `PRD-45` | Revoir le masquage adulte |
| A10 | `D5`, `PRD-52` § 3.4 | Changer de méthode de test |
| A11 | Texte de la fiche, `PRD-52` | Décrire l'app sans nommer les formats |
| A12, B2 | `D6`, `S2`, `PRD-09` | Sources HTTPS seules, couverture très réduite |
| A13 | Déclarations, `PRD-52` § 3.5 | Compléter le dossier UE |
| B1 | `S1`, `PRD-09` | Arrêt ou révision du projet si aucun repli ne marche |
| B3 | `S3`, `PRD-28`, `PRD-29` | HLS seul, ou arrêt si HLS échoue aussi |
| B4 | `PRD-28`, `PRD-29` | Les sources qui exigent `Referer` restent refusées |
| B5 | `S9`, `PRD-07` | Chiffrement JavaScript et clé dans `widgetdata` |
| B6, B7 | `S8`, `S10`, `PRD-06`, `PRD-12` | Budgets ou plafond de catalogue réduits |
| B8 | `S6`, `PRD-32` | Revoir l'interface du lecteur |
| B9 | `S11`, `PRD-49` | `PRD-49` abandonné |
| B10 | `S13`, `D4` | Machine x86-64 |

## Après l'envoi

- Archiver chaque réponse, ou l'absence de réponse, avec sa date dans `docs/store/` (`PRD-04` § 5, `PRD-52` § 5).
- Reporter les faits confirmés dans `docs/TIZEN_FACTS.md` avec la marque `[DOC]` seulement si Samsung les a écrits, en citant le ticket.
- Sans réponse après deux semaines, relancer une fois sur le même ticket. Ce délai est une suggestion : Samsung n'en annonce aucun.
- Une réponse positive n'est pas une garantie de certification. Faire relire la position par un juriste (`PRD-04` § 3.2).

## Sources

- [Becoming Seller Office Members](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-seller-office-member.html)
- [Becoming Partners](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-partner.html)
- [Application Registration Q&A](https://developer.samsung.com/tv-seller-office/faq/application-registration.html)
- [Age Rating](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/age-rating.html)
- [Launch Checklist](https://developer.samsung.com/tv-seller-office/checklists-for-distribution/launch-checklist.html)
- Faits techniques : [`TIZEN_FACTS.md`](TIZEN_FACTS.md)
