# "Standard covoiturage" specification

## 1. Introduction

### 1.1. Goals

TODO description of the goals of this standard

### 1.2. Terminology

This specification includes a "Normative" and an "Informative" part.

TODO explain Normative / Informative concepts

This specification also defines the following terms : 

- Journey : TODO define journey 
- Regular trip : TODO define regular trip
- ...

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](http://tools.ietf.org/html/rfc2119).

### 1.3. References to other parts of the standard

This document is the entry point for the "Standard covoiturage" specification. Its goal is to explain from a functional point of view how the technical parts should be interpreted.

It makes reference to other parts of the standard : 

- An OpenAPI schema describing the entire API
- Other documents describing specific technical parts like process flows ...

### 1.4. Functional blocks and standard compliance

The standard is decomposed into functional blocks. To implement a functional 
block, the transport operator MUST implement all the specifications related to
this functional block (they have dedicated sections in the current document).

Functional blocks for searching:
* Searching for a driver journey as a passenger
* Searching for a passenger journey as a driver
* Searching for a regular journey as a passenger
* Searching for a regular passenger as a driver

Functional blocks for booking:
* Delegated booking with a deep link to the transport operator's application
* Booking information feed to the MaaS (recommended extension to the delegated 
  booking with a deep link)
* Integrated booking via API

To comply with this standard, a transport operator MUST implement at least one 
functional block for performing a search, or one of the two booking options 
that comply with this specification.

In addition, any functionality offered by the transport operator's API that is 
covered by the standard MUST comply with the current specifications. The 
transport operator MAY agree with the MaaS platform to provide additional 
functionalities, not covered by the standard, in a custom manner.

## 2. Normative 

The first draft versions of this specification do not include normative parts.

## 3. Informative

### 3.1. Search / Carpool information

#### 3.1.1. Search for Journeys

TODO explain driver/passenger journeys and link to OpenAPI spec

#### 3.1.2. Search for Regular trips and associated journeys

TODO explain driver/passenger regular trips and link to OpenAPI spec

### 3.2. Booking

This specifications provides 2 flows for implementing an initial booking transaction : 

- Integrated booking by API : the initial booking is handled by an API exchange between MaaS platforms and operators. Users stay on their original application, without having to switch to another.
- Delegated booking by Deeplink : booking is handled by switching applications, with a deeplink to access directly to the right page on the carpooling operator application.

These 2 flows share a common syntax for operations after the initial booking occurs. Integrated booking by API can still leverage deeplink information to access the operator's application while Delegated booking by deeplink uses API endpoints to provide information of the evolution of the booking process to the MaaS platform.

MaaS platforms and carpooling operators MUST support at least one of these 2 flows if they implement booking.

They MAY implement both.

#### 3.2.1. Integrated booking by API

Integrated booking by API is based on 3 routes described in the [OpenAPI specification](standard-covoiturage_openapi.yaml) : 

- POST /bookings : initialize the booking request
- PATCH /bookings : modify the state of the booking status to complete the booking flow 
- GET /bookings/{bookingId} : get the state of the booking

#### 3.2.2. Delegated booking by Deeplink

Delegated booking by Deeplink is described in [booking-deeplink-specification.md](booking-deeplink-specification.md)

It uses the same routes as Integrated booking by API to share the status of the booking with the MaaS platform : 

- PATCH /bookings : modify the state of the booking status to complete the booking flow 
- GET /bookings/{bookingId} : get the state of the booking

### 3.3. Authentication across platforms ("MaaS Connect")

TODO link to the connect spec
