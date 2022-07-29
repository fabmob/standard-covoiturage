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

Le standard est publié en version informative, pour permettre une 
expérimentation avant publication en version normative. À ce titre, il est 
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

# Contact

Pour toute question, contactez 
[ghislain@fabmob.io](mailto:ghislain@fabmob.io).
