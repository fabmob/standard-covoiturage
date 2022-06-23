# Introduction to the "standard-covoiturage"

Version française [disponible ici](./README.fr.md).

This directory presents the work to create a standard for making data 
available, carrying out transactions and, more generally, enabling the 
exchange of services between carpooling operators and MaaS platforms. The 
working group working on it brings together French carpooling and MaaS 
players.

To date, the standard is based on a [OpenAPI specification](./standard-carpooling_openapi.yaml).
Additional documentation is being written (see the
[README of the "doc" directory](./doc/README.md)).

The standard is published in a draft version, to allow an experimentation 
before publication in a normative version. As such, it may be subject to 
breaking changes.

Publication in normative version is planned for the end of 2022.

# Functional scope

The "standard-covoiturage" (carpooling-standard)  covers several functional 
blocks that can be implemented separately or jointly.

The functional blocks covered by the API are the following (more details below):

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
