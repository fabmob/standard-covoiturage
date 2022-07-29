# Introduction au standard covoiturage

Ce répertoire présente les travaux du [groupe de travail sur le « standard covoiturage »](https://wiki.lafabriquedesmobilites.fr/wiki/Standard_Covoiturage)
qui rassemble des acteurs français du covoiturage et du MaaS.

Le standard covoiturage a vocation à décrire des méthodes de référence pour
mettre à disposition ou échanger des données, réaliser des transactions et plus
généralement permettre des échanges de services entre opérateurs de covoiturage
et opérateurs de MaaS.

Le standard covoiturage couvre plusieurs blocs fonctionnels qui peuvent être
implémentés séparément ou conjointement.

Les blocs fonctionnels couverts par l'API sont les suivants (plus de détails
ci-dessous) :

* [La recherche d'un trajet d'un conducteur, en tant que 
  passager](#fonctionnalit%C3%A9s-li%C3%A9es-%C3%A0-une-recherche-de-trajets-simples)
* [La recherche d'un trajet d'un passager, en tant que 
  conducteur](#fonctionnalit%C3%A9s-li%C3%A9es-%C3%A0-une-recherche-de-trajets-simples)
* [La recherche d'un trajet régulier en tant que passager](#fonctionnalit%C3%A9s-li%C3%A9es-%C3%A0-une-recherche-de-trajets-r%C3%A9guliers)
* [La recherche d'un passager régulier en tant que conducteur](#fonctionnalit%C3%A9s-li%C3%A9es-%C3%A0-une-recherche-de-trajets-r%C3%A9guliers)
* [L'échange de messages entre deux usagers](#echange-de-messages-entre-usagers)

* [Le flux d'information des réservations vers le MaaS](#récupération-dinformations-dun-flux-unidirectionnel-de-lopérateur-de-covoiturage-vers-la-plateforme-de-maas-booking-feed)
  
* [La réservation via redirection vers l'opérateur de covoiturage (deep 
  link)](#réservation-via-deep-link)
* [La réservation intégrée par API](#réservation-intégrée-par-api)

# Fonctionnalités liées à une recherche de trajets simples

Entrées d'API implémentées par l'opérateur de covoiturage :

* `GET /driver_journeys` et/ou
* `GET /passenger_journeys`

Ces entrées permettent de rechercher des trajets de covoiturage en tant que 
passager ou conducteur, avec comme principaux critères de recherche les points 
de départ, d'arrivée, et le jour et l'heure de départ. La réponse donne une 
liste de trajets potentiels. Parmi les informations concernant ces trajets, on 
trouve :
 
* L'URL du deep link si la réservation par deep-link est supportée
* Le prix estimé pour le voyageur (cas deep link), ou le prix attendu par 
  l'opérateur de covoiturage (cas réservation par API)
* Les informations concernant le conducteur ou le passager 

# Fonctionnalités liées à une recherche de trajets réguliers

Entrées d'API implémentées par l'opérateur de covoiturage :

* `GET /driver_regular_trips` et/ou
* `GET /passenger_regular_trips`

Ces entrées permettent de rechercher des trajets de covoiturage réguliers.  
Contrairement aux trajets simples, la recherche ne précise pas une date 
précise, mais une récurrence hebdomadaire spécifiée via une heure de la 
journée, et les jours de la semaine. La recherche de trajets récurrents 
peut-être limitée à une période de temps donnée.

La réponse contient une liste de trajets, qui couvrent au moins partiellement 
les exigences de récurrence spécifiées lors de la requête. Ces trajets sont 
identiques à ceux obtenus via une recherche simple, et peuvent donc être 
réservés individuellement de la même manière.

Dans le cas de réservation via deep link, afin d'éviter des allers-retours 
entre les applicatifs du MaaS et de l'opérateur de covoiturage, l'attribut 
`webUrl` permet de rediriger vers une page qui agrège les différents trajets, 
par exemple la page du conducteur.

# Echange de messages entre usagers

Point d'API implémenté par l'opérateur de covoiturage et la plateforme de 
MaaS :

* `POST /messages` 
  
Permet d'envoyer un message d'un utilisateur à un autre. De manière 
factultative, le trajet ou la réservation auquel se réfère le message peut 
être précisé.

Les données (id etc.) de l'utilisateur destinataire peuvent être obtenues via 
une recherche préalable (par exemple, attribut `driver`).  

# Récupération d'informations d'un flux unidirectionnel de l'opérateur de covoiturage vers la plateforme de MaaS (booking feed)

Point d'API implémentée par la plateforme de MaaS :

* `POST /booking_events`

Ce cas d'usage nécessite une authentification de l'utilisateur via un 
dispositif de type "MaaS connect" (basé sur OpenIdConnect).

Une fois cette authentification faite, l'opérateur de covoiturage peut 
renvoyer un flux d'information à l'opérateur de MaaS sur les évènements 
concernant cet utilisateur (création d'une réservation, changement d'état 
d'une réservation).  

# Réservation via deep link

Pas de point d'API spécifique.

L'URL du deep link est fourni à la plateforme de MaaS au moment de la 
recherche. Le "booking feed" présenté ci-dessus est recommandé en conjonction 
avec la réservation par deep link pour permettre le retour d'informations sur 
les réservations faites par l'utilisateur.

# Réservation intégrée par API

Entrées d'API implémentées par l'opérateur de covoiturage :

* `POST /bookings`
* `PATCH /bookings/{bookingId}`
* `GET /bookings/{bookingId}`

Entrées d'API implémentées par la plateforme de MaaS :

* `PATCH /bookings/{bookingId}` 
  
La réservation par API ne nécessite pas d'authentification de l'usager.

L'entrée `POST` permet de demander la création d'une réservation, l'entrée 
`PATCH` de modifier le statut d'une demande existante (possibilité de modifier 
le statut de manière symétrique, notamment pour annuler une réservation).  
Enfin, l'entrée `GET` permet d'interroger l'opérateur de covoiturage sur les 
informations relatives à une réservation.

Concernant la formation du prix, le prix renvoyé lors de la recherche est le 
prix attendu par la plateforme de covoiturage. Le Maas est libre de fixer un 
prix différent à l'utilisateur final. 
