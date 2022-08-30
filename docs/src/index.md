---
home: false
---

# Introduction

## Goals

The "standard covoiturage" ("carpoling standard") is an API specification that 
has been designed by a French working group bringing together French 
carpooling and MaaS players.
 
The "standard covoiturage" is intended to describe reference methods for 
making data available, carrying out transactions and, more generally, enabling 
the exchange of services between carpooling operators and MaaS platforms.

To date, the standard is based on an
[OpenAPI specification](./standard-carpooling_openapi.yaml).
The goal of the current document is to explain from a functional point of view 
how the technical parts should be interpreted.


## Drafts and final versions

The standard is published in a draft version (v1.0.0-alpha), to allow an 
experimentation before publication in a final version. As such, it might be 
subject to breaking changes.

The publication in final version is planned for the end of 2022.



## Functional perimeter

The standard is decomposed into functional blocks. To implement a functional 
block, the transport operator MUST implement all the specifications related to
this functional block (they have dedicated sections in the current document).



Functional blocks for searching:
- Searching for a driver journey as a passenger
- Searching for a passenger journey as a driver
- Searching for a regular journey as a passenger
- Searching for a regular passenger as a driver

Functional blocks for booking:
- Delegated booking with a deep link to the transport operator's application
- Booking information feed to the MaaS (recommended extension to the delegated 
  booking with a deep link)
- Integrated booking via API

Functional block for user information:
- Exchanging messages between two users

## Standard compliance

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", 
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this 
document are to be interpreted as described in [RFC 
2119](http://tools.ietf.org/html/rfc2119).

To comply with this standard, a transport operator MUST implement at least one 
functional block for performing a search, or one of the two booking options 
that comply with this specification.

In addition, any functionality offered by the transport operator's API that is 
covered by the standard MUST comply with the current specifications. The 
transport operator MAY agree with the MaaS platform to provide additional 
functionalities, not covered by the standard, in a custom manner.





