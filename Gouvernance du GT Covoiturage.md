# Gouvernance GT Covoiturage (version 0.3.2 - 16/03/2022)


## I. Préambule

### a. Historique et contexte

Ces dix dernières années différentes méthodes ont été utilisées par les 
opérateurs pour mettre à disposition de l’information sur l’offre de 
covoiturage disponible : standard [RDEX+](https://rdex.fabmob.io/), [API 
ViaNavigo Carpool](https://doc.vianavigo.com/api-carpool/#!/status/get_status) 
voire format GTFS adapté.

Dans le contexte du développement de systèmes de MaaS publics et privés qui 
agrègent des services de mobilité pour créer des offres de services 
multimodales ou des offres de service complexes, à l’échelle de plusieurs 
territoires et systèmes, le besoin d’un nouveau standard a émergé ces 
dernières années. Un premier travail exploratoire a été réalisé par des AOM et 
des opérateurs de covoiturage, basé sur [RDEX+](https://rdex.fabmob.io/), et 
diffusé sous le nom de RDEX+. D'autres travaux ont été financés par l'Etat et 
l'ADEME  comme le projet Carpool as a Service, et des API propriétares sont 
mises à disposition par certains opérateurs

Le nouveau standard est promu par des contributeurs fondateurs qui 
représentent largement l’écosystème : opérateurs de covoiturage, Gart, 
Autorités Organisatrices des Transports.


### b. Le standard covoiturage : définition et objectifs

Le « standard covoiturage » décrit des méthodes de référence pour mettre à 
disposition ou échanger des données, réaliser des transactions et plus 
généralement permettre des échanges de services entre un opérateur de 
covoiturage et un opérateur de MaaS. L’objectif d'intégration du covoiturage 
dans le MaaS est prioritaire sur l’intégration entre plateformes de 
covoiturage. 

Trois finalités complémentaires ont été définies pour le “standard 
covoiturage” :
* Rendre l’intégration de l’offre de covoiturage la plus fluide possible dans 
  d’autres plateformes et générer ainsi plus d’usage. 
* Permettre l’expression des différentes formes de covoiturage. 
* Améliorer l’expérience utilisateur et l’accessibilité des services de 
  covoiturage.

Le présent document décrit la structure du standard, le processus de 
production et d’évolution de celui-ci, ainsi que les règles de gouvernance et 
de prise de décision. Il est inclus au standard lui-même, et évolue selon les 
mêmes règles que le reste du standard. Il est constitué de trois parties : la 
description des rôles, la structure du standard et son processus 
d’élaboration.



### c. Un standard ouvert

#### 1. Les principes

Le « standard covoiturage » et sa gouvernance préservent les intérêts des 
parties prenantes de la communauté de manière non-discriminante. A cet effet 
ils respectent des principes fondamentaux des « standards ouverts » tels que 
[définis par OpenStand](https://open-stand.org/about-us/principles/) et 
pratiqués par des organisations de référence de standardisation pour le 
numérique (Internet Society, W3C, IETF, IEE, Fondation OASIS), et en 
particulier:
* Un processus transparent basé sur le consensus. Ce processus garantit que 
  toutes les parties prenantes concernées peuvent contribuer et sont 
  sollicitées, et qu’aucune organisation ou aucun groupe particulier ne domine 
  le processus, au détriment des autres
* Un standard qui serve les intérêts collectifs de l’écosystème du covoiturage 
  : qualité technique, stabilité, compétition équitable, possibilité de 
  développer des innovations sur la fondation que constitue le standard
* Accessibilité et gratuité. Le standard est disponible pour toutes les 
  parties prenantes, sans frais, sur un dépôt commun, et la communauté 
  travaille à toute documentation, outil ou implémentation de référence qui 
  rend facilite l’accès et l’implémentation effective du standard. Cet accès 
  répond à la définition de « standard ouvert » au sens de [l’article 4 de la 
  loi pour la confiance dans l’économie 
  numérique](https://www.legifrance.gouv.fr/loda/article_lc/LEGIARTI000006421544)
* L’adoption et la mise en œuvre de ce standard par les parties prenantes de 
  l’écosystème est volontaire

#### 2. La licence

Le standard covoiturage est publié sous licence ouverte [Apache 
2.0](https://www.apache.org/licenses/LICENSE-2.0), licence couramment utilisée 
(ex : format GTFS).

La licence répond au besoin de libre accès au standard, de protection des 
réutilisateurs en matière de propriété intellectuelle et ouvre la possibilité 
d’innover par enrichissement du standard à titre expérimental (dans ce cas de 
figure il n’est pas permis de faire usage de la marque « standard covoiturage 
»).

## II. Les rôles

### a. Contributeur de la communauté covoiturage

#### 1. Participant

Toute personne physique ou morale peut participer et contribuer aux groupes de 
travail sur une base volontaire et bénévole. Les outils de travail 
collaboratif mis en place par la communauté sont accessibles aux contributeurs 
de manière libre ou sur simple demande.

#### 2. Membre délibérant

Toute personne morale qui **met en œuvre le standard dans le cadre de ses 
activités** (opérateur, intégrateur ou éditeur de solution, AOM) **peut 
participer à la gouvernance** avec le statut de membre délibérant. Un membre 
délibérant peut participer à toutes les décisions et dispose du droit de veto 
chaque fois que cela est prévu par les règles de gouvernance.  Le statut de 
membre délibérant et l’accès aux droits attachés est gratuit, il ne repose sur 
aucune contribution financière. Il nécessite une inscription avec une 
description de l’organisation et la désignation d’au moins un représentant. Le 
membre délibérant doit pouvoir justifier de sa qualité de partie prenante qui 
met en oeuvre le standard dans le cadre de ses activités, ou bien sera amenée 
à le faire.
Tout membre délibérant apparaît dans **une liste officielle des acteurs 
soutenant et promouvant le standard.** Tout membre délibérant peut demander à 
ne plus figurer sur cette liste, et renonce alors aux droits attachés au 
statut de membre délibérant.

### b. Groupe de travail

Lorsque des contributeurs souhaitent développer et expérimenter de nouvelles 
fonctionnalités, ils peuvent constituer un groupe de travail sur une base 
volontaire. Le groupe de travail est constitué librement par ceux qui 
souhaitent coopérer ensemble sur une extension du standard qui sera publiée 
dans la partie informative du standard.

Des groupes de travail peuvent aussi être créés par la communauté pour traiter 
tout sujet qui a trait l’amélioration ou la promotion du standard.

Pour être reconnu officiellement par la communauté, un groupe de travail est 
validé par la réunion d’orientation, sur la base d’une présentation des 
objectifs poursuivis par celui-ci. Le groupe de travail est ouvert à tout 
contributeur de la communauté.

### c. Facilitateur

Le facilitateur est une personne ou une organisation qui accompagne la 
communauté dans le maintien, la promotion et le développement du standard. Ses 
missions évoluent en accord avec la communauté, et recouvrent a minima :
 
* La gestion des outils de la communauté (Github, outils de communication et 
  collaboration, documentation)
* La coordination et la facilitation des groupes de travail (qui peut être 
  partagée ou déléguée à d’autres contributeurs de la communauté)
* La représentation du standard et de la communauté qui le soutient auprès de 
  toute partie prenante externe, en coordination avec les contributeurs de la 
  communauté
* Organiser la prise de décision et faire respecter les principes de 
  gouvernance. Elaborer et soumettre à la communauté des décisions relatives à 
  la gouvernance et au développement du standard (en particulier tous les 
  sujets listés ci-dessus).

Le facilitateur ne prend pas part aux votes de la communauté, pendant toute la 
durée de son mandat.

Le facilitateur de la communauté est choisi chaque année par la communauté par 
un vote majoritaire. Le mandat est annuel.

Il est proposé que le facilitateur pour l’année 2022 soit La Fabrique des 
Mobilités, qui peut soutenir cette activité dans le cadre du programme CEE « 
Mon compte mobilité » qui prévoit un soutien à l’écosystème MaaS dans le 
développement et la promotion de standards.


## II. Le standard

### a. Publication

Un dépôt de référence (Gitlab ou Github) hébergera la version de référence 
validée par la communauté, les versions élaborées par les groupes de travail, 
et toute évolution proposée proposée par une partie prenante à la communauté, 
en vue d’une validation. Ce dépôt accueillera aussi toute la documentation 
disponible relative au standard et aux travaux des groupes de travail. Les 
décisions, les débats qui l'entourent, et la justication des votes (en 
particulier des vétos) font partie de la documentation librement accessibles.

Le dépôt est géré et administré par le facilitateur.

### b. Langue

La version initiale du standard est publiée en anglais sauf, sauf la 
documentation de la présentation du standard qui elle est initialement en 
français. Au démarrage, les comptes-rendus de réunion des groupes de travail 
sont en français.
Des langues additionnelles, notamment l'anglais, peuvent être développpées par 
la communauté, selon les besoins identifiées.

### c. Structures du standard

#### 1. Les versions

La communauté approuve le standard et ses évolutions périodiquement. La 
version du standard approuvée par la communauté selon les règles de 
gouvernance de ce document est la version canonique, qui fait référence. Toute 
utilisation de la marque « standard covoiturage » comporte l’obligation de se 
conformer à l’une des versions canoniques publiées sur le Git de référence de 
la communauté. Une version du standard qui ne correspond à l’une des versions 
canoniques dûment approuvée par la communauté ne peut pas faire usage de la 
marque « standard covoiturage ».

Une gestion sémantique des versions est mise en place selon le standard 
[SemVer](https://semver.org/lang/fr/).

#### 2. La partie normative

La partie normative est le tronc commun du standard covoiturage dont la mise 
en œuvre a été approuvée selon les procédures prévues à la gouvernance.
Des outils peuvent être approuvés par la communauté pour valider la conformité 
d’une implémentation du standard covoiturage.  Chaque implémentation doit 
faire l'objet d’une phase de consultation (voir III.b) et de vote pour 
intégrer la partie normative. Les critères objectifs permettant de 
caractériser si une implémentation relève de la partie normative du standard 
sont : 
  
* **Le portage** : porté par tous les opérateurs, et plus généralement 
  l’ensemble des “membres délibérants”
* **L’engagement** : engagement de tous les opérateurs à se conformer aux 
  spécifications s’ils mettent en oeuvre toute fonctionnalité décrite par le 
  standard dans la partie normative
* **Le fonctionnement** : fonctionnement validé en production avec des 
  implémentations
* **La garantie de pérennité, de performance et de respect des règlementations 
  en vigueur**

#### 3. La partie informative

La partie informative contient des extensions et des évolutions structurantes 
du standard, publiées à part sur le dépôt de référence. Chaque extension est 
publiée sous la responsabilité d’un groupe de travail approuvé par la 
communauté selon les règles définies dans la gouvernance.

Les extensions concernent des évolutions fonctionnelles du standard que des 
parties prenantes souhaitent expérimenter dans un cadre de concertation 
commun. La communauté ne s'engage pas sur le respect des 4 critères formulés 
pour la partie normative.

L’objectif de la partie informative est de permettre l’expérimentation et 
approuver des évolutions de la partie normative sur la base d’expériences 
concrètes et d’un processus d’élaboration ouvert. Elle décrit des évolutions 
de fonctionnalités qui sont rétrocompatibles avec la partie normative. Cela 
signifie qu’une implémentation d’extension contenue dans la partie informative 
doit pouvoir se faire dans le respect de l’ensemble de la partie normative. 
C’est le principal critère de publication d’une extension dans le cadre de la 
partie informative.

Le passage de la partie informative à la partie normative peut entraîner des 
évolutions du standard rendues nécessaires par la prise en compte des retours 
d’expérience.


#### 4. Les versions non officielles du standard

La licence Apache 2.0 autorise toute partie prenante à faire évoluer et 
adapter le standard librement. Ces évolutions se font alors sans approbation 
ni concertation particulière de la communauté. Par conséquent ces versions du 
standard ne doivent pas utiliser la marque « standard covoiturage ».

Le facilitateur et le comité d’orientation du standard sont mandatés par la 
communauté pour faire respecter le bon usage de la marque « standard 
covoiturage ».

## III. Processus d'élaboration du standard

### a. Elaboration de la version 1.0.0

> Julie : à revoir
:::warning
Les contributeurs de la communauté adoptent à l’unanimité une version 0.3.0 du 
standard composée uniquement du présent document de gouvernance. A partir de 
ce moment, les signataires ayant apporté leur soutien seront ajoutés dans la 
partie 4 et deviennent membres délibérants de droit. Les règes de la 
gouvernance sont dès lors appliquées pour faire évoluer le document de 
gouvernance et publier des premières versions des travaux dans les volets 
informatifs puis normatifs. Ce standard publié sur le dépôt de référence 
contient les éléments suivants :
* Validation de la version 0.3.0 au 31/01/2022
* Adoption des thèmes des premiers groupes de travail : d'ici le 07/02/2022
* Publication des premiers résultats au volet informatif : 31/03/2022
* Publication d'une version 1.0.0 du standard avec une partie normative et 
  d'une partie informative 31/05/2022
:::

### b. Evolution de la partie normative

L’évolution de la partie normative est un processus continu qui vise à 
répondre aux exigences de performance et stabilité de standard d’une part, aux 
besoins d’évolution d’autre part.
Tous les deux mois la communauté passe en revue des évolutions proposées pour 
la partie normative. Ces évolutions peuvent être proposées directement par la 
communauté (évolutions mineures, correctifs); être le résultats d'un groupes 
de travail dédié ou bien être une extension informative candidate à la partie 
normative. Le facilitateur organise le partage, le tri et le vote sur ces 
évolutions du standard, si nécessaire. de bugs, qui sont entièrement 
rétrocompatibles avec les versions précédentes. 

Les évolutions proposées font l'objet de discussions sur les outils de la 
communauté afin d’en comprendre les objectifs et les modalités.
Les votes se déroulent de la manière suivante :

* Les évolutions font l'objet d'une revue et d'une discussion lors d'une 
  réunion d'orientation du standard à laquellle sont conviés les membres 
  délibérants
* Chaque évolution discutée en réunion d'orientation fait l'objet d'une 
  publication 5 jours ouvrées au préalable
* Des échanges en ligne de la publication de l'évolution sont organisés 
  jusqu'à 5 jours ouvrés au-delà de la réunion d'orientation
* à l'issue de ce délai un vote en ligne est organisé. Les votes possibles 
  sont POUR, NEUTRE, CONTRE
* Le quorum est fixé à 10 membres délibérants, y compris les votes NEUTRES
* Chaque évolution qui receuille 2/3 des votes positifs est validée ( le 
  calcul se fait sur les votes exprimés POUR ou CONTRE uniquement, les votes 
  NEUTRE sont uniquement comptabilisés pour l'atteinte du quorum)
* Tout membre délibérant dispose d'un délai de 5 jours ouvrés pour émettre, 
  seul ou à plusieurs, un veto motivé et publié sur les outils de la 
  communauté. En cas de veto de la proposition est soumise de nouveau à 
  consultation ou à de nouveau travaux.

### c. Revue des extentions informatives candidates au volet normatif

Lorsqu’une extension du volet informatif a été mise en œuvre dans des projets, 
le groupe de travail qui soutient cette extension peut la soumettre à la 
communauté en vue d’une adoption dans la partie normative du standard. Le 
processus est le suivant :

* Le groupe de travail fournit une version révisée de l’extension ;
* Le groupe de travail fournit un rapport évaluant les conditions de mise en 
  œuvre de l’extension dans des projets et en dresse un bilan. Le rapport doit 
  montrer que l'extension respecte les 4 critères de la partie normative 
  (insérer lien)
* La phase de concertation et vote décrite au (insérer lien) est mise en 
  oeubre en vue d'adopter l'évolution de la partie normative


### d. Evolution de la partie informative

Chaque groupe de travail informatif a la responsabilité de faire évoluer 
l’extension dont il a la charge, par consensus au sein du groupe de travail 
(absence de veto de l’un des membres du groupe). Le groupe de travail doit 
assurer la transparence de ses travaux (publication au sein de la communauté) 
et consulte la communauté autant que nécessaire pour définir son extension.

Chaque nouvelle version publiée doit faire l’objet d’une période de 
concertation et information de la communauté d’une durée de 10 jours ouvrés, à 
l'issue de laquelle le groupe de travail dédide par consensus de publier 
l'extension informative révisée.

Chaque groupe de travaila la responsabilité de maintenir la rétrocompatibilité 
de son extension avec la dernière version canonique de la partie normative du 
standard.

Chaque année le groupe de travail doit soumettre un rapport à la communauté 
décrivant les évolutions apportées à l’extension, les implémentations 
réalisées et leur bilan, ainsi que les perspectives pour l’année suivante 
(évolutions fonctionnelles envisagées, processus éventuel d’adoption dans la 
partie normative). A cette occasion la communauté peut prendre une décision de 
désinscription de l’extension de la partie informative. Cette décision à la 
majorité qualifiée des 2/3, sera motivée par des critères d’intérêt de la 
communauté pour l’extension ou de difficultés techniques constatées avec la 
partie normative du standard.

Lorsqu’une extension n’est plus soutenue par la communauté, cette extension et 
la documentation ne sont plus inscrites dans la partie informative du 
standard, et sont archivées dans une section dédiée des outils de la 
communauté, pour référence.

### d. Réunion d'orientation du standard

Tous les deux mois, les membres délibérants organisent une réunion 
d'orientation du standard. Ces réunions permettent de traiter de différents 
missions : 
* Revue et discussion des évolutions normatives proposées
* Le lancement d'un nouveau groupe de travail, à la demande d'un groupe de 
  contributeurs
* Proposer à la communauté une facilitateur et des modalités de son 
  intervention
* Adopter des motions ou lancer des actions d'intérêt pour la promotion et 
  l'évolution du standard


## 4. Listes de membres délibérants ayant approuvé le standard dans sa version actuelle


| Organisation                     | Représentant       | Suppléant           | Type                                  |
| --------                         | --------           | --------            | -----------                           |
| Blablacar                        | Ricardo Lage       |                     | Opérateur de covoiturage              |
| Citygo                           | Eric Feuerstein    |                     | Opérateur de covoiturage              |
| Cityway                          | Eric Fière         | Jean-Luc Gauducheau | Opérateur de MaaS                     |
| Coopgo                           | Arnaud Delcasse    |                     | Opérateur de covoiturage              |
| DGITM                            | Mélanie Veissier   |                     | Ministère de la transition Ecologique |
| Ecov                             | Romain Fayoux      | Thomas Matague      | Opérateur de covoiturage              |
| Karos                            | Tristan Croiset    | Camille Noël        | Opérateur de covoiturage              |
| Klaxit                           | Cyrille Courtière  | Noé Jubert          | Opérateur de covoiturage              |
| Mobicoop                         | Olivier Sarrat     |                     | Opérateur de covoiturage              |
| Métropole Aix-Marseille Provence | Christophe Brusset |                     | AOM                                   |
| OuestGo                          | Félix Moal         |                     | Opérateur de covoiturage              |
| Région Occitanie                 | Ariane Lissarrague | Nicolas Chazot      | AOM                                   |
