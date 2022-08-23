# Introduction au standard covoiturage

Ce répertoire présente les travaux du [groupe de travail sur le « standard 
covoiturage 
»](https://wiki.lafabriquedesmobilites.fr/wiki/Standard_Covoiturage)
qui rassemble des acteurs français du covoiturage et du MaaS.

Le standard covoiturage a vocation à décrire des méthodes de référence pour
mettre à disposition ou échanger des données, réaliser des transactions et plus
généralement permettre des échanges de services entre opérateurs de covoiturage
et opérateurs de MaaS.


À date, le standard s'appuye sur une
[spécification OpenAPI (en)](./standard-covoiturage_openapi.yaml).
Une documentation additionnelle est en cours de rédaction (cf. le
[README du répertoire "doc"](./doc/README.fr.md)).

Le standard est publié en version informative (v1.0.0-alpha), pour permettre 
une expérimentation avant publication en version normative. À ce titre, il est 
possible que le standard soit sujet à des changements non rétro-compatibles.  
Se référer au
[document de gouvernance](./Gouvernance du GT Covoiturage.md) pour plus de 
détails.

La publication en version normative est prévue pour fin 2022.

# Périmètre fonctionnel

Le standard covoiturage couvre plusieurs blocs fonctionnels qui peuvent être 
implémentés séparément ou conjointement.

Les blocs fonctionnels couverts par l'API sont les suivants :

* La recherche d'un trajet d'un conducteur, en tant que passager
* La recherche d'un trajet d'un passager, en tant que conducteur
* La recherche d'un trajet régulier d'un conducteur en tant que passager
* La recherche d'un passager régulier en tant que conducteur
* L'échange de messages entre deux usagers
* La réservation intégrée par API
* La réservation déléguée via redirection vers l'opérateur de covoiturage 
  (deep link)
* Le flux d'information des réservations vers le MaaS

Il y a donc deux parcours de réservation distincts, la réservation intégrée 
**par API** et la réservation déléguée **par deep link**. L'opérateur de 
covoiturage peut choisir d'implémenter l'un, l'autre, ou les deux parcours.

# Détails par bloc fonctionnel

## Recherche de trajets simples

Le standard couvre la rercherche de trajets covoiturés en tant que passager ou 
conducteur, d'une origine à une destination et à une date et heure données. La 
réponse présente une liste de trajets potentiels, qui précisent notamment :

* Les informations concernant le conducteur ou le passager, et le trajet
* Le prix estimé pour le voyageur (cas deep link), ou le prix attendu par 
  l'opérateur de covoiturage (cas réservation par API)
* L'URL du deep link de réservation si la fonctionnalité est supportée

Entrées d'API associées à cette fonctionnalité implémentées par l'opérateur de 
covoiturage :

* `GET /driver_journeys` et/ou
* `GET /passenger_journeys`
 
## Recherche de trajets réguliers

Le standard permet également la recherche de trajets de covoiturage réguliers.  
Contrairement aux trajets simples, la recherche ne précise pas une date 
précise, mais une récurrence hebdomadaire spécifiée via une heure de la 
journée, et les jours de la semaine souhaités.

La réponse présente une liste de propositions, où chacune contient une liste 
de trajets qui couvrent au moins partiellement les exigences de récurrence 
spécifiées lors de la requête. Chacun de ces trajets peut être réservé 
individuellement de la même manière qu'à la suite d'une recherche simple.

Dans le cas de la réservation via deep link, afin d'éviter plusieurs 
allers-retours entre les applicatifs du MaaS et de l'opérateur de covoiturage, 
l'attribut `webUrl` permet de rediriger vers une page qui agrège les 
différents trajets, par exemple la page du conducteur.

Entrées d'API associées à cette fonctionnalité implémentées par l'opérateur de 
covoiturage :

* `GET /driver_regular_trips` et/ou
* `GET /passenger_regular_trips`
 
## Échange de messages entre usagers

Le standard permet l'envoi de messages d'un utilisateur à un autre. De manière 
factultative, le trajet ou la réservation auquel se réfère le message peut 
être précisé.

Entrée d'API associée à cette fonctionnalité implémentée par l'opérateur de 
covoiturage et la plateforme de MaaS :

* `POST /messages` 
  

## Réservation intégrée par API

La réservation intégrée par API est l'un des deux parcours pour la réservation 
d'un trajet. Elle ne nécessite pas d'authentification de l'usager auprès de 
l'opérateur de covoiturage.  L'opérateur de MaaS peut demander la création 
d'une réservation (entrée `POST` ci-dessous), de modifier le statut d'une 
réservation (entrée `PATCH`) ou interroger l'opérateur de covoiturage sur les 
informations relatives à une réservation (entrée `GET`).

L'opérateur de covoiturage peut quant à lui symétriquement modifier le statut 
d'une réservation, par exemple en cas d'annulation.

Le prix renvoyé lors de la recherche est le prix attendu par la plateforme de 
covoiturage. Le Maas est libre de fixer un prix différent à l'utilisateur 
final.


Entrées d'API implémentées par l'opérateur de covoiturage :

* `POST /bookings`
* `PATCH /bookings/{bookingId}`
* `GET /bookings/{bookingId}`

Entrée d'API implémentée par la plateforme de MaaS :

* `PATCH /bookings/{bookingId}` 
  
## La réservation déléguée via redirection vers l'opérateur de covoiturage (deep link)

La réservation déléguée par deep link est l'un des deux parcours pour la 
réservation d'un trajet, dans laquelle l'utilisateur est redirigé vers 
l'application de l'opérateur de covoiturage afin de réserver ses trajets.  
L'opérateur de MaaS peut bénéficier d'un flux d'information concernant les 
réservations qui sont effectuées. Pour cela, une authentification de 
l'utilisateur via un dispositif basé sur OpenID Connect est nécessaire.

L'URL du deep link de réservation est fourni à la plateforme de MaaS au moment 
de la recherche.

Entrée d'API implémentée par la plateforme de MaaS pour le flux d'informations 
concernant les réservations via deep link :

* `POST /booking_events`

# Contact

Pour toute question, contactez 
[ghislain@fabmob.io](mailto:ghislain@fabmob.io).
