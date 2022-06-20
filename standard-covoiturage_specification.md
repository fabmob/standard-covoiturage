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

## 2. Normative 

The first draft versions of this specification do not include normative parts.

## 3. Informative

### 3.1. Search / Carpool information

#### 3.1.1. Search for Journeys

##### General Workflow

Endpoints implemented by the carpooling operator:
 
* `GET /driver_journeys` and/or
* `GET /passenger_journeys`

There are two flavours of searches: a driver can search for passengers (so, he 
searches a `passenger_journey`), or a passenger can search for a driver 
(looking for a `driver_journey`). The difference between these two searches 
are in the endpoints used and the return values, the following sequence 
diagram works for both. The carpooling operator can implement one or the other 
or both API endpoints.

~~~mermaid
sequenceDiagram
    participant U as User
    participant M as MaaS
    participant T as TO
    U->>M: Search for journeys
    rect rgb(255, 255, 236)
    M->>T: GET /driver_journeys (or /passenger_journeys)
    T-->>M: DriverJourneys[] (or PassengerJourneys[])
    end
    M-->>U: Show results
~~~

The search results are queried by the MaaS platform to the carpooling operator 
in a single API call, providing all relevant query informations. The minimum 
required information is the starting and arrival places.  

In particular, no user information is required to perform a search.

The search provides the necessary information to procede to the booking phase 
(described in other sections). If booking by deep link is supported, the link 
is shared along the journey information in the response's attribute `webUrl`.  
If integrated booking by API is supported, then the journey information plus 
the `price` attribute enable to make a booking request.

##### The case of a round trip

If the case of a round trip, it is up to the MaaS platform to perform the 
searches for the outward and return journeys separately.

##### The case of regular trips

Searching for regular trips has its own specific workflow presented in
[a dedicated section](https://github.com/fabmob/standard-covoiturage/blob/spec_search/standard-covoiturage_specification.md#3112-search-for-regular-trips-and-associated-journeys).

##### Technical specification

The carpooling operator MUST return at most `count` matching results, or all 
matching results if `count` parameter is not specified. All returned results 
MUST match the query parameters. If the parameter `count` is specified, the 
carpooling operator SHOULD return in priority the most relevant results. The 
measure of relevance is left to the discretion of the carpooling operator.

The `id` attribute of the response MUST be unique for a given carpooling 
operator (same `operator` attribute) among all journeys (`passengerJourneys` 
and `driverJourneys`).

The meaning of `price` attribute of the response SHOULD be a price estimation, 
if no estimation is possible, a price with `type` "UNKNOWN" SHOULD be 
returned. If the `type` is "PAYING", then the attributes `amount` and 
`currency` are required.

In the case of integrated booking by API, this estimation is to be taken as 
the actual amount expected by the carpooling operator. Therefore, the 
carpooling operator SHOULD return a `price` of type "FREE" or "PAYING" to 
allow the MaaS platform to book by API.

#### 3.1.2 Search for regular trips and associated journeys

##### General workflow 

Endpoints implemented by the carpooling operator:

* `GET /driver_regular_trips` and/or
* `GET /passenger_regular_trips`

These are the counterparts of the `GET \passenger_journeys` and `GET 
\driver_journeys` endpoints, but for searching regular trips. The carpooling 
operator can implement one or the other or both API endpoints.

A regular trip is a recurring trip that is repeated from one week to another.  
The API allows for searching regular trips that start around the same time of 
the day at given week days. The query parameters therefore specify an 
additionnal required `departureTimeOfDay`, and an optional `departureWeekdays` 
compared to the single journey search.

The return values offer a `schedules` attribute that details the decomposition 
of the regular trip into single journeys. All attributes combined give the 
same level of information for the single journeys than a simple search would.

Again, the response gives all the necessary elements to procede with the 
booking phase as with single journey objects, but with an additionnal optional 
`webUrl` attribute, that allows for a better user experience than having to go 
back and forth between the MaaS the carpooling apps as many times as there are 
journeys to book.

##### Workflow for more sophisticated regular journeys

It is up to the MaaS platform to assemble the results of several calls into a 
more sophisticated regular journey. For instance, a varying 
`departureTimeOfDay` depending on week day, or different schedules on 
alternating weeks, can be queried with a large `timeDelta` and post-filtering, 
or with several calls with different `departureTimeOfDay` and concatenation.

##### Technical specification for searching for regular journeys

The carpooling operator MUST return at most `count` matching results, or all 
matching results if `count` parameter is not specified. All returned results 
MUST match the query parameters.

A regular trip matches the `departureWeekdays` and `departureTimeOfDay` if 
*any* journey of the regular trip matches the time and day constraint and 
starts between `minDepartureDate` and `maxDepartureDate` (included).

The returned results may not reach the `maxDepartureDate` because of the 
limited forecasting horizon of the carpooling operator.

If the carpooling operator supports booking by deep link, the `webUrl` 
attribute in the response SHOULD link to a page giving an overview of the 
regular trips (e.g. a "driver" page) and allowing to continue the process of 
booking regular trips by the user.

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
