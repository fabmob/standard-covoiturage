# Introduction to the "standard covoiturage"

Version française [disponible ici](./README.fr.md).

This repository presents the work of the "standard covoiturage" ("carpoling 
standard") working group. It brings together French carpooling and MaaS 
players.
 
The "standard covoiturage" is intended to describe reference methods for 
making data available, carrying out transactions and, more generally, enabling 
the exchange of services between carpooling operators and MaaS platforms.

To date, the standard is based on an
[OpenAPI specification](./standard-covoiturage_openapi.yaml).
Additional documentation is currently being written (see the
[README of the "doc" directory](./doc/README.md)).

The standard is published in a draft version (v1.0.0-alpha), to allow an 
experimentation before publication in a final version. As such, it might be 
subject to breaking changes.

The publication in final version is planned for the end of 2022.

# Functional scope

The "standard covoiturage" (carpooling standard)  covers several functional 
blocks that can be implemented separately or jointly.

The functional blocks covered by the API are the following :

* The search for a driver's journey, as a passenger
* The search for a passenger's journey, as a driver
* The search for a regular driver's trip as a passenger
* The search for a regular passenger as a driver

* The exchange of messages between two users
* Integrated reservation by API
* Delegated reservation via redirection to the carpooling operator (deep link)
* Booking information flow to the MaaS

# Contact

For any question, please contact
[ghislain@fabmob.io](mailto:ghislain@fabmob.io).
