# Outil de calcul des reports de congés

> Usage interne RH · régime applicable à compter du **1er juillet 2026** (décret n° 2025‑564 du 21 juin 2025)
> Fichier de l'outil : `interface/report-conges-interface-v3.html` — **un seul fichier**, aucun serveur, aucune installation.

## Sommaire

1. [Présentation](#1-présentation)
2. [Mode d'emploi](#2-mode-demploi-assistantes-rh)
3. [Les règles de calcul en résumé](#3-les-règles-de-calcul-en-résumé)
4. [Qualité : le jeu de tests](#4-qualité--le-jeu-de-tests)
5. [Mise en ligne (GitHub + Vercel)](#5-mise-en-ligne-github--vercel)
6. [Pour la maintenance](#6-pour-la-maintenance)
7. [Historique des actions](#7-historique-des-actions)
8. [Points en suspens](#8-points-en-suspens-à-faire-trancher)
9. [Organisation du dossier](#9-organisation-du-dossier)

---

## 1. Présentation

L'outil aide les gestionnaires RH à **calculer les droits à report des congés annuels non pris du fait d'un congé pour raison de santé ou d'un congé lié aux responsabilités parentales ou familiales**.

Il remplace le calcul manuel (tableau Excel) : l'assistante RH saisit la chronologie de l'agent, l'outil calcule période par période, applique les plafonds, détermine les fenêtres de report de 15 mois, puis fournit :

- une **fiche de calcul (RCR)** à imprimer ou à enregistrer en PDF, à classer au dossier individuel ;
- un **projet de courrier** pour informer l'agent, avec tous les chiffres du calcul.

**Principe de confidentialité.** Tout le calcul se fait **dans le navigateur** de la personne qui utilise l'outil. Aucune donnée saisie n'est envoyée, enregistrée ou conservée : fermer ou actualiser la page efface tout. Le fichier ne contient aucun appel réseau, et une règle de sécurité intégrée (Content‑Security‑Policy) interdit au navigateur toute connexion sortante. L'outil **n'envoie aucun mail** : il prépare le texte, l'utilisatrice le copie dans sa messagerie.

**Navigateurs :** Edge, Chrome, Firefox (JavaScript activé). L'outil fonctionne aussi hors connexion, en double‑cliquant sur le fichier.

---

## 2. Mode d'emploi (assistantes RH)

### 2.1 Avant de commencer

Rassemblez, pour l'agent concerné :

| Élément | Où le trouver |
|---|---|
| Les **périodes** : activité, congés pour raison de santé, congés familiaux, congé parental / disponibilité d'office — avec leurs **dates de début et de fin** | Gestion de carrière / paie |
| La **quotité** de travail de chaque période (100 %, 90 %, 80 %, 50 % en temps partiel thérapeutique…) | Arrêtés, gestion du temps de travail |
| La **date de reprise** (effective, ou prévue si l'agent n'a pas encore repris) | Gestion de carrière |
| Les **congés annuels déjà posés**, par année | Outil de gestion du temps |
| La **date d'information de l'agent** (date d'envoi du courrier) | Vous : c'est la date à laquelle vous l'informez |

### 2.2 Saisir un dossier

Ouvrez le fichier : l'outil s'affiche immédiatement (pas d'onglet à choisir).

| Champ | À saisir |
|---|---|
| **Identification** (facultatif) | Nom, matricule… Il apparaît sur la fiche et dans l'objet du courrier. |
| **Date de reprise (effective ou prévue)** | Obligatoire. C'est elle qui sépare les congés acquis « avant » et « pendant » l'absence. |
| **Date d'information de l'agent** (facultatif) | Date d'envoi du courrier. Le report « à la reprise » court à compter du **1er jour du mois suivant** cette date. **À défaut, le calcul est provisoire** (point de départ = date de reprise). |
| **Chronologie des périodes** | Une ligne par période : *Type*, *Début*, *Fin (incluse)*, *Quotité %*. Bouton **+ Ajouter une période**. Les périodes sont triées automatiquement par date de début. |
| **Congés annuels déjà posés** | Une ligne par année d'acquisition : *Année* et *CA posés (jours)*. Bouton **+ Ajouter une année**. |

### 2.3 Les quatre types de période

| Type | Quand l'utiliser | Effet |
|---|---|---|
| **Activité (période travaillée)** | Période travaillée, y compris une absence pour raison de santé de **moins de 30 jours consécutifs** jugée, au cas par cas, sans effet sur la prise des congés. | Génère des droits à la quotité saisie. |
| **Congé pour raison de santé** | CMO, CLM, CLD, congé de grave maladie, CITIS. | Génère des droits à la quotité saisie ; relève du **plafond « maladie » (20 j)**. |
| **Congé lié aux responsabilités parentales ou familiales** | Maternité, couches pathologiques, paternité, naissance, adoption, présence parentale, solidarité familiale, proche aidant. | Génère des droits **à 100 %** (la quotité est grisée et fixée à 100 % : le temps partiel est suspendu) ; relève du **plafond « parental / familial » (25 j)**. |
| **Sans acquisition** | Congé parental, disponibilité d'office. | Ne génère aucun droit et n'efface pas les droits acquis avant. |

### 2.4 Lancer le calcul et lire la fiche

Cliquez sur **Calculer les droits à report**. Si un champ obligatoire manque, un message s'affiche en rouge : *« Saisissez au moins une période. »*, *« Indiquez la date de reprise (effective ou prévue). »* ou *« Une période a une date de fin antérieure à sa date de début. »*

La **fiche de calcul des reliquats de congés reportés (RCR)** contient :

1. l'identification, la date de reprise, la date d'information et la date de génération ;
2. une note : soit le **point de départ du report** (si la date d'information est saisie), soit *« Calcul provisoire »* (sinon) ;
3. le **tableau récapitulatif** : *Droit à report · Jours · Fenêtre de report (15 mois) · État* ;
4. le **total** : *« Total des droits à report existant à la reprise »* ;
5. le **détail année par année** : frise chronologique, formule de chaque période (jours × 25 × quotité ÷ 360, arrondi), congés posés déduits, plafond et écrêtement, dates de report, et les encadrés *« choix à solliciter »* le cas échéant.

**Les trois états d'une fenêtre de report**

| État | Signification |
|---|---|
| **En cours** | La fenêtre de 15 mois n'est pas terminée. |
| **Délai écoulé à ce jour** | La fenêtre s'est terminée **après** la reprise mais avant aujourd'hui : le droit existait à la reprise, et le délai pour poser ces jours est maintenant passé. |
| **Perdus** | La fenêtre s'est terminée **avant** la reprise : l'agent n'a pas pu utiliser ces jours. Ils sont listés mais **exclus du total**. |

> **Important.** L'outil **ne suit pas les jours effectivement posés** après la reprise. Le total correspond aux droits existant **à la reprise**. Les états sont évalués à la date du jour.

**Cas particuliers affichés**

- **Régularisation (trop‑posés)** : si l'agent a posé plus de congés qu'il n'en avait acquis avant son absence, la ligne apparaît en négatif et vient en diminution du total.
- **Choix à solliciter** : quand l'agent reprend avant le 30/04 de l'année suivante, un encadré rappelle les deux options à lui proposer (report de 15 mois, ou période de référence jusqu'au 30/04 N+1 avec perte des jours non pris).

### 2.5 Imprimer la fiche ou l'enregistrer en PDF

1. Cliquez sur **Imprimer la fiche** (ou `Ctrl+P`).
2. Choisissez la destination **« Enregistrer au format PDF »**.
3. Enregistrez le PDF au dossier individuel de l'agent.

La fiche imprimée contient le logo, l'en‑tête et toute la fiche de calcul. **Ne sont pas imprimés** : le formulaire, les boutons, l'introduction, le courrier à l'agent, le bas de page de contrôle.

> ⚠ La note « Calcul provisoire » ne figure pas sur l'impression. **Renseignez la date d'information avant d'imprimer** : la date apparaît alors sur la fiche.

### 2.6 Envoyer l'information à l'agent (courrier)

Sous le détail du calcul, le bloc **Courrier d'information à l'agent** propose un **objet** et un **message** déjà rédigés, qui reprennent les éléments du calcul :

- la date de reprise (« Suite à votre reprise… » ou « Dans la perspective de votre reprise… prévue le… » si elle est à venir) ;
- la nature de l'absence (santé, familial, ou les deux) ;
- le total des jours reportables ;
- le récapitulatif de chaque droit avec ses dates de report ;
- le détail de chaque période (dates, nature, quotité, jours acquis, congés posés, plafond appliqué, solde reportable) ;
- les rappels utiles (plafonds, droit commun après la reprise) ;
- le paragraphe **« Choix à nous indiquer »** quand l'option CeA s'applique ;
- la phrase sur la **date d'information** et le point de départ du report.

Comment faire :

1. Renseignez la **date d'information** dans le formulaire (date d'envoi), puis relancez le calcul : le courrier en tient compte.
2. Relisez et modifiez si besoin le texte (zone modifiable).
3. Cliquez sur **Copier l'objet**, puis **Copier le message**, et collez‑les dans votre messagerie.

Le texte est **régénéré à chaque nouveau calcul** : une modification manuelle est perdue si vous recalculez. Un avertissement s'affiche au‑dessus si la date d'information est absente.

### 2.7 Exemples, effacement, aide

- **Charger un exemple : Exemple 1 à 5** — cinq dossiers fictifs prêts à l'emploi pour se familiariser ou vérifier un résultat (survolez le bouton pour voir la situation).
- **Tout effacer** — vide le formulaire et les résultats.
- **Règles de calcul** (bouton en haut à droite) — page qui décrit, en 14 sections, tout ce que fait l'outil. Un bouton **← Retour au calculateur** ramène à la saisie, **sans perdre ce qui a été saisi**.
- **Jeu de tests** (petit bouton en bas de page) — voir [§4](#4-qualité--le-jeu-de-tests).

### 2.8 Bonnes pratiques de saisie

- **Ne coupez pas une absence continue en plusieurs lignes sans nécessité** : le comptage des jours autour de février est sensible au découpage (voir [§8](#8-points-en-suspens-à-faire-trancher)). Une seule ligne par absence continue de même nature et de même quotité.
- **Évitez les chevauchements et les trous** entre périodes : l'outil ne les contrôle pas.
- **Saisissez l'activité après la reprise** si vous la connaissez (jusqu'à fin d'année, avec sa quotité) : elle entre dans le calcul du plafond proratisé (voir exemple : activité de novembre‑décembre à 100 % qui fait passer le plafond de 15 j à 16 j).
- **Date de reprise prévue** : vous pouvez calculer avant la reprise effective ; le courrier adopte alors la formulation « prévue le… ».
- Les **congés posés** se saisissent par **année d'acquisition**, et sont déduits des congés acquis *avant* l'absence de cette année.

### 2.9 Questions fréquentes

| Question | Réponse |
|---|---|
| *« Aucune période d'absence ouvrant droit à report n'a été identifiée »* | Aucune année saisie ne contient d'absence (santé/familial) ni d'activité commençant avant la date de reprise. Vérifiez les types et les dates. |
| Pourquoi le plafond est inférieur à 20 j ? | Il est proratisé selon les quotités de l'année (80 % → 16 j ; 50 % puis 100 % → moyenne pondérée par la durée). Voir la section 6 des règles. |
| Pourquoi une ligne en négatif ? | Régularisation : plus de congés posés que de congés acquis avant l'absence. |
| Pourquoi « Délai écoulé à ce jour » ? | Le dossier est ancien : la fenêtre de report est terminée à la date du jour. Le droit existait à la reprise et reste compté dans le total. |
| La date de départ du report est antérieure à la reprise | Cas où la date d'information est antérieure à la reprise, dans le même mois : la règle est appliquée telle quelle et un avertissement s'affiche. |
| Où sont enregistrées mes saisies ? | Nulle part : elles ne quittent pas le navigateur et disparaissent à la fermeture de la page. Enregistrez le PDF de la fiche. |

### 2.10 Ce que l'outil ne fait pas

- Il **ne décide pas** si l'absence a empêché la prise des congés (appréciation au cas par cas).
- Il ne calcule ni l'**alimentation du CET** (simple rappel : pas d'alimentation si moins de 20 jours posés dans l'année), ni la **prolongation exceptionnelle** de la période de report (non mobilisée à la CeA), ni le **choix de l'agent**.
- Il ne calcule pas les congés acquis à compter de la reprise, ni les jours effectivement posés après la reprise.
- Il ne conserve et n'envoie aucune donnée.

---

## 3. Les règles de calcul en résumé

La **référence détaillée est la page « Règles de calcul » de l'outil** (14 sections, exemples chiffrés recalculés automatiquement à chaque ouverture). En résumé :

| Sujet | Règle appliquée |
|---|---|
| **Années** | Chaque période est découpée au 31/12 ; chaque année civile est analysée séparément. |
| **Comptage des jours** | Convention 30 jours par mois, bornes incluses (le 31 est assimilé au 30 ; une année = 360 j). |
| **Droit d'une période** | jours × 25 × quotité ÷ 360 (2,08 CA par mois à temps plein). |
| **Arrondi** | **Au 0,5 le plus proche, à chaque période** ; une valeur exactement au milieu (x,25 ; x,75) est arrondie vers le haut. Le même arrondi s'applique au plafond. |
| **Congés « avant l'absence »** | Activité commençant avant la reprise. Solde = acquis − congés posés. Négatif = régularisation. |
| **Plafonds** | **20 j** (maladie) et **25 j** (parental / familial) à temps plein, **proratisés** par la moyenne des quotités pondérée par la durée de chaque période de l'année. Écrêtement distinct pour chacun. |
| **Écrêtement** | Surplus retiré d'abord des compteurs « pendant » (du dernier au premier), puis du solde « avant ». |
| **Point de départ des 15 mois** | Reprise, ou **1er jour du mois calendaire suivant l'information** si une date est saisie. Agent **absent au 31/12 N** : **01/01 N+1**. |
| **Fenêtre de report** | Départ + 15 mois − 1 jour. |
| **Congé familial** | Quotité imposée à 100 % (temps partiel suspendu). |
| **Total** | Somme des droits dont la fenêtre n'est pas terminée avant la reprise, régularisations comprises. |

---

## 4. Qualité : le jeu de tests

Le jeu de tests est **dans la page** : le bas de page indique en permanence le résultat (ex. *« ✓ 90 contrôles réussis sur 90 · 300 dossiers générés vérifiés · 3 constats »*), en rouge si un contrôle échoue. Le petit bouton **Jeu de tests** ouvre le détail. Les contrôles s'exécutent dans le navigateur, **sans utiliser les données saisies**, à chaque ouverture.

| Type | Contenu |
|---|---|
| **Contrôles unitaires** (63) | Arrondi, comptage des jours, plafonds, écrêtement, points de départ, fenêtres, états, courrier à l'agent, et **cohérence avec la page « Règles de calcul »** (chaque exemple chiffré de la page est recalculé par l'outil). |
| **Cas synthétiques** (19) | Dossiers construits à la main, avec résultat attendu et dates de report calculés indépendamment. Chacun peut être **chargé dans le calculateur** (bouton *Charger*). |
| **Dossiers générés au hasard** (300 par défaut) | Dossiers fictifs tirés à partir d'une **graine** (même graine = mêmes dossiers, modifiable dans la page), avec davantage de cas limites (fins de mois, 30/31 décembre, reprise le lendemain). Chaque dossier est **recoupé par un second calcul, écrit séparément** à partir des règles : lignes, jours, dates, états, total et détail période par période doivent être identiques. Huit propriétés sont vérifiées (multiples de 0,5, plafonds respectés, total = somme des lignes, calcul déterministe, jours indépendants de la date du jour et de la date d'information, courrier complet…). |
| **Constats** (3) | Comportements notés, **volontairement non corrigés** (voir [§8](#8-points-en-suspens-à-faire-trancher)) ; ils ne comptent pas comme des échecs mais sont signalés. |

**Solidité vérifiée lors de la mise au point :** 24 000 dossiers générés (8 graines), soit environ 90 000 lignes de report, ont été comparés au second calcul **sans aucun écart** ; 12 erreurs volontairement introduites dans le calcul (arrondi, ordre d'écrêtement, plafond, fenêtre, bornes de dates, congés posés…) ont **toutes été détectées**.

**En ligne de commande** (Node.js, optionnel) : `node interface/tests/run-tests.js` exécute les mêmes contrôles. Variables facultatives : `GRAINE=…`, `N=…` (nombre de dossiers), `OUTIL_HTML=…` (autre fichier à tester). Code retour non nul en cas d'échec.

---

## 5. Mise en ligne (GitHub + Vercel)

**Seul fichier à publier : `interface/report-conges-interface-v3.html`**. Il est autonome : le logo est intégré, il n'y a aucune ressource externe (aucun lien `http(s)`, aucun `<link>`, aucune police ou script externe).

1. **Renommer** le fichier en `index.html` dans le dépôt (Vercel l'affiche alors à l'adresse principale).
2. Créer un dépôt GitHub **privé** contenant `index.html` (et, si vous le souhaitez, ce `README.md`).
3. Dans Vercel, importer le dépôt, laisser les réglages par défaut (site statique, pas de commande de construction), déployer.
4. **Contrôler le site** : ouvrir l'adresse, charger l'Exemple 1, calculer, vérifier que le bas de page affiche des contrôles réussis ; dans les outils du navigateur (F12 → onglet Réseau), vérifier qu'**aucune requête** ne part lors d'une saisie ou d'un calcul.

À savoir :

- Par défaut, **toute personne qui connaît l'adresse peut ouvrir l'outil** (qui contient les règles internes). Regardez les options de protection de déploiement de Vercel (certaines, comme le mot de passe, peuvent être payantes selon l'offre).
- Vercel voit seulement qu'une page a été chargée (adresse IP, date), comme pour tout site ; **jamais les données saisies**.
- **Ne pas publier** : le dossier `corrections/` (des PDF contiennent un nom d'agent réel), `régles-exemples/` (documents de travail), les anciennes versions, ni les fichiers Word.

---

## 6. Pour la maintenance

Le fichier est découpé en **blocs de script indépendants** :

| Bloc | Rôle |
|---|---|
| `<script id="moteur">` | Le **calcul** (aucune dépendance à la page) : jours, droits, plafonds, écrêtement, dates, états, récapitulatif. |
| `<script id="courrier">` | Construction du **texte du courrier** à partir du résultat du calcul. |
| `<script id="exemples">` | Les 5 exemples (`PRESETS`). |
| `<script id="jeu-tests">` | Le **jeu de tests** (contrôles, cas synthétiques, générateur, calcul de référence). |
| `<script>` (dernier) | L'**interface** : vues, formulaire, affichage des résultats, courrier, pied de page. |

| Pour changer… | Où |
|---|---|
| La **règle d'arrondi** | une seule fonction : `arrondiDemi` (script `moteur`) — elle sert aux droits **et** au plafond. |
| Les **plafonds** 20 j / 25 j | constantes `PLAFOND_SANTE` / `PLAFOND_FAM` (script `moteur`). |
| Le **texte du courrier** | `construireCourrier` (script `courrier`). |
| Les **exemples** | `PRESETS` (script `exemples`). |
| Les **couleurs** | variables `--accent*` au début de la feuille de style (accent actuel : bleu du logo `#0D4094`). |
| Les **tests** | script `jeu-tests` (les tests sont dans la page, plus dans `run-tests.js`). |
| La **page des règles** | `<section id="vue-regles">`. |

**Procédure après toute modification du calcul :**

1. mettre à jour la page « Règles de calcul » ;
2. adapter si besoin les cas synthétiques et le calcul de référence du jeu de tests ;
3. lancer `node interface/tests/run-tests.js` (ou ouvrir la page et lire le bas de page) : tout doit être réussi ;
4. si une règle change vraiment, les tests de cohérence échouent tant que la page des règles n'est pas alignée — c'est voulu.

**Sécurité :** la règle `Content-Security-Policy` (balise `<meta>` en tête de fichier) vaut `default-src 'none'; img-src data:; style-src 'unsafe-inline'; script-src 'unsafe-inline'; form-action 'none'`. Ne jamais ajouter de ressource externe, d'analyse d'audience, de stockage ou d'envoi des données saisies.

---

## 7. Historique des actions

Tout le travail décrit ci‑dessous a été réalisé le **13/11/2026**, à partir de la version v2 de l'interface (24/08/2026) et des documents du dossier `régles-exemples/` (procédure du 01/07/2026, exercices et solutions, tableau Excel d'exemple).

### 7.1 Chronologie

| # | Action | Résultat / vérification |
|---|---|---|
| 1 | **Lecture et analyse de la v2** (fichier unique de 731 lignes, 2 onglets : « Comprendre la procédure » et « Calculateur »). | Structure documentée ; points d'attention relevés (calcul et affichage mélangés dans une fonction de ~200 lignes, formule répétée 3 fois, quotité du congé familial non imposée, pas d'export). |
| 2 | **Lecture des 4 corrections** (`corrections/`) et des 2 PDF montrant l'erreur de l'outil ; lecture de la procédure, des exercices et du tableau Excel. | Les 2 erreurs sont reproduites exactement : plafond « maladie » calculé avec une seule quotité (10 j au lieu de 13,5 j ; 20 j au lieu d'un plafond proratisé). La correction 4 est identifiée comme **remplaçant la 2**. |
| 3 | **Création de la v3** (copie de la v2, qui est conservée) et **application des corrections 1, 3 et 4**. Plafond maladie proratisé selon les quotités ; plafond familial (25 j) distinct avec son propre écrêtement ; point de départ du report au 1er jour du mois suivant l'information ; 01/01 N+1 si l'agent est absent au 31/12. | Cas du 2ᵉ PDF : plafond **13,5 j** ; les 5 exemples officiels donnent les mêmes résultats qu'avant (aucune régression). |
| 4 | **Refonte technique** : moteur de calcul séparé de l'affichage, formules et écrêtement factorisés, arrondi regroupé en un point unique. Ajout d'un **jeu de tests** en ligne de commande (33 tests au départ). | Résultats identiques sur 11 dossiers avant/après refonte ; 4 mutations volontaires détectées. |
| 5 | **Quotité 100 % imposée pour les congés familiaux** (champ grisé dans le formulaire) ; **règle de sécurité** interdisant toute connexion réseau (vérifiée dans un vrai navigateur). | Un congé familial saisi à 50 % est traité à 100 %. |
| 6 | **Suppression de fichiers** devenus inutiles (à la demande) : `report-conges-interface.html` (racine), les 3 fichiers `arbre-de-décision-…html`, `conges_copilot.html`. | 5 fichiers supprimés ; `outil-de-calcul-report-de-congés-rh.html` non touché. |
| 7 | **Interface** (demande) : suppression du champ « Quotité pour le plafond » (inutile : quotités saisies par période) ; **suppression des onglets** (l'outil s'affiche dès l'ouverture) ; **bouton « Règles de calcul »** ouvrant une page décrivant le calcul tel qu'appliqué (12 sections à l'époque) ; ancienne procédure conservée en bas, repliée. | Navigation vérifiée : la saisie est conservée en allant et revenant. |
| 8 | **Libellés** (suite à une question sur « Total … fenêtres non échues » et « Échéance dépassée ») : *« Total des droits à report existant à la reprise »*, avec la précision que les états sont évalués à la date du jour et que l'outil ne suit pas les jours posés ; état renommé **« Délai écoulé à ce jour »**. | Page des règles alignée. |
| 9 | **Charte graphique** : logo de la CeA (version couleur) intégré dans le fichier, design sobre, accent bleu du logo ; fiche imprimée avec logo, titre et introduction du calculateur masqués ; total encadré. | Captures d'écran et émulation de l'impression contrôlées. |
| 10 | **Changement de règle d'arrondi** (demande) : **au 0,5 le plus proche, à chaque période**, au lieu du 0,5 supérieur ; **suppression du choix** « Fraction exacte / Taux mensuel arrondi » ; suppression du bloc « procédure de référence » (parcours guidé, fiches) et de son code. | Tous les exemples recalculés et vérifiés à la main (exemple 3 : une régularisation de −0,5 j apparaît) ; la page des règles réécrite ; **tests de cohérence** page ↔ calcul ajoutés. |
| 11 | **Courrier d'information à l'agent** (demande) : projet de courrier sous le calcul, variables issues du calcul, modifiable, boutons de copie, **absent de la fiche imprimée** ; section 13 de la page des règles. | Copie testée avec un vrai clic ; protection contre le code HTML saisi dans le nom ; 60 tests. |
| 12 | **Jeu de tests intégré à la page** (demande) : résultat en bas de page, bouton vers le détail, 19 cas synthétiques (avec bouton *Charger*), dossiers générés au hasard recoupés par un second calcul, propriétés, constats ; section 14 de la page des règles. **Exemples renommés** « Exemple 1 » à « Exemple 5 », sans nom d'agent. | 90 contrôles ; 24 000 dossiers sans écart ; 12 erreurs volontaires détectées ; affichage des échecs vérifié sur une copie volontairement fautive. |
| 13 | **Conseils de mise en ligne** (GitHub + Vercel) : un seul fichier à publier. | Fichier vérifié autonome : aucune ressource externe. |
| 14 | **Rédaction de ce README** (mode d'emploi + historique). | — |

### 7.2 Incidents rencontrés et corrigés en cours de route

- Lors de la réécriture de la page des règles (étape 10), le bouton « ← Retour au calculateur » du bas de page a été supprimé par erreur ; **détecté par un test dans le navigateur**, rétabli, et un test vérifie désormais sa présence.
- Une espace manquante dans la ligne du plafond de la fiche (« =16 j ») a été **corrigée**.
- Un test trop faible (« solde nul ») a été **remplacé par un vrai contrôle**.
- Les erreurs volontaires ont révélé deux **trous dans le premier jeu de tests** (ordre de l'écrêtement ; reprise exactement le 31 décembre) : le calcul de référence compare maintenant aussi le détail période par période, et des cas limites ont été ajoutés.

### 7.3 Décisions prises

| Décision | Origine |
|---|---|
| La correction 4 remplace la correction 2. | Demande (« apparemment »), confirmée par le contenu. |
| Quotité du congé familial imposée à 100 %. | Demande (« il faut rajouter cette nuance »). |
| Aucune connexion de l'outil au web ; l'outil est publié mais les données restent dans le navigateur. | Demande. |
| Suppression des onglets, du champ de quotité du plafond, du choix de méthode et du bloc « procédure de référence ». | Demandes. |
| Arrondi au 0,5 le plus proche, à chaque période. | Demande. |
| Valeur exactement au milieu (x,25 / x,75) arrondie **vers le haut**. | Choix de ma part, **à confirmer**. |
| Plafond proratisé par la moyenne des quotités pondérée par la durée des périodes saisies. | Interprétation des corrections 1 et 3 (exemple « 13,5 j »), **à confirmer**. |
| Pas de bouton « ouvrir dans la messagerie » (texte long tronqué par certains logiciels) : copie dans le presse‑papiers. | Choix de ma part. |

---

## 8. Points en suspens (à faire trancher)

| # | Point | Situation |
|---|---|---|
| 1 | **Comptage de février** (3 constats du jeu de tests) | Une période couvrant exactement février compte **28 j** (et non 30) ; deux périodes coupées à fin février perdent **2 j** ; une coupure entre un 30 et un 31 compte **1 j de trop**. Non corrigé : décision RH à prendre (par exemple « un mois complet vaut toujours 30 jours, février compris »). |
| 2 | **Plafond du premier PDF d'erreur** | L'outil donne un plafond de 15 j (16 j si l'activité de fin d'année est saisie) ; la note de correction laissait un doute sur le résultat attendu. |
| 3 | **Congés posés et plafond** | Méthode de la procédure appliquée (cumul « brut » soumis au plafond, puis déduction des congés posés du solde « avant ») : le report effectif peut être inférieur au plafond. À confirmer. |
| 4 | **Information avant la reprise** | Si l'information précède la reprise dans le même mois, le report démarre avant la reprise (avertissement affiché). Faut‑il plutôt ne jamais commencer avant la reprise ? |
| 5 | **Congés « avant l'absence »** | Point de départ = reprise (ou mois suivant l'information), même si l'agent est absent au 31/12 ; l'arbitrage ne le précise pas. |
| 6 | **Plafond familial proratisé** | Même proratisation que pour la maladie ; elle n'existait pas avant. |
| 7 | **Quotité 100 % pour tous les congés familiaux** | Appliquée aussi à la présence parentale et au proche aidant : à confirmer pour un agent à temps partiel dans ces congés. |
| 8 | **Période « jusqu'au 30/04 N+1 »** | La procédure l'indique « en attente d'arbitrage » ; l'outil la propose comme option à solliciter. |
| 9 | **Texte du courrier** | Rédigé pour l'outil : à faire valider par les RH (introduction, « Choix à nous indiquer », signature). |
| 10 | **Fiche imprimée** | La mention « Calcul provisoire » n'y figure pas (voir [§2.5](#25-imprimer-la-fiche-ou-lenregistrer-en-pdf)). |
| 11 | **Mode d'emploi Word** (`Mode_emploi_outil_report_conges_V2.docx`) | Périmé (anciennes règles, ancien arrondi, onglets) : ce README le remplace. |
| 12 | **Nom d'agent dans des PDF de correction** | À ne pas publier ; à anonymiser si ces fichiers doivent circuler. |

---

## 9. Organisation du dossier

```
conges_rh/
├── README.md                              ← ce document
├── interface/
│   ├── report-conges-interface-v3.html    ← L'OUTIL (version actuelle, seul fichier à publier)
│   ├── report-conges-interface-v2.html    ← ancienne version (conservée)
│   ├── logo-color.png / logo.png          ← logos sources (déjà intégrés dans le HTML)
│   └── tests/run-tests.js                 ← lanceur en ligne de commande du jeu de tests
├── corrections/                           ← corrections et PDF d'exemple reçus (ne pas publier)
├── régles-exemples/                       ← procédure, exercices et tableau Excel d'exemple (ne pas publier)
├── Mode_emploi_outil_report_conges_V2.docx   ← ancien mode d'emploi (périmé)
└── outil-de-calcul-report-de-congés-rh.html  ← ancienne version, non utilisée (à confirmer avant suppression)
```
