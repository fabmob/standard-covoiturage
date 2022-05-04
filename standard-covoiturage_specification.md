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

There are two flavours of searches: a driver can search for passengers (so, he 
searches `passenger_journey`), or a passenger can search for a driver (looking 
for a `driver_journey`). The difference between these two searches are in the 
endpoints used and the return values, so the following sequence diagram works 
for both.

~~~mermaid
sequenceDiagram
    participant U as User
    participant M as MaaS
    participant T as TO
    U->>M: Search for trips
    M->>T: GET /driver_journeys (or /passenger_journeys)
    T-->>M: DriverJourneys[] (or PassengerJourneys[])
    M-->>U: Show results
~~~

The search results are queried by the MaaS platform to the transport operator 
in a single API call, providing all relevant query informations. The minimum 
required information is the starting and arrival places.  

In particular, no user information is required to perform a search.

###### The case of a round trip

If the case of a round trip, it is up to the MaaS platform to perform the 
searches for the outward and return journey separately.

##### The case of regular journeys

Searching for regular journeys has its own specific workflow presented in 
{TODO anchor link}.

##### Simple search technical specification

The transport operator MUST return at most `count` matching results, or all 
matching results if `count` parameter is not specified. All returned results 
MUST match the query parameters.

If the parameter `count` is specified, the transport operator SHOULD return in 
priority the most relevant results. The measure of relevance is left to the 
discretion of the transport operator.

If no `departureDate` attribute is provided by the MaaS platform in the call 
to `GET \driver_journeys` or `GET \passenger_journeys`, the current datetime 
MUST be assumed.

The `id` attribute of the response MUST be unique for a given Transport 
Operator, among all journeys (`passengerJourneys` and `driverJourneys`).

The `price` attribute of the response SHOULD be a price estimation, if no 
estimation is possible, a price with type "UNKNOWN" SHOULD be returned.

#### Search for Regular trips and associated journeys

##### General workflow 

The counterpart of `\passenger_journeys` and `\driver_journeys` endpoints for 
searching regular journeys are two GET endpoints: `\driver_regular_trips` and 
`\passenger_regular_trips`. The query parameters specify an additionnal 
required `departureTimeOfDay`, and an optional `departureWeekdays`.

The return values are similar, they differ mainly in that they offer a 
`schedules` attribute that details several trips.  More differences will occur 
at the booking phase.

##### Workflow for more sophisticated regular journeys

It is up to the MaaS platform to assemble the results of several calls into a 
more sophisticated regular journey. For instance, a varying 
`departureTimeOfDay` (in the same week, or alterning weeks) can be queried 
with a large `timeDelta` and post-filtering, or with several calls with 
`departureTimeOfDay` and concatenation.

##### Technical specification for searching for regular journeys

The transport operator MUST return at most `count` matching results, or all 
matching results if `count` parameter is not specified. All returned results 
MUST match the query parameters.

The returned results MAY not reach the `maxDepartureDate` depending on the 
forecasting horizon of the transport operator.

A regular trip matches the `departureWeekdays` and `departureTimeOfDay` if 
*any* journey of the regular trip matches the time and day constraint.

The `webUrl` attribute in the response SHOULD link to a page giving an 
overview of the regular trips (e.g. a "driver" page) and allowing to continue 
the process of booking regular trips by the user (booking by deeplink).

The transport operator MUST return at most `count` matching results, or all 
matching results if `count` parameter is not specified. All returned results 
MUST match the query parameters.

The returned results MAY not reach the `maxDepartureDate` depending on the 
forecasting horizon of the transport operator.

A regular trip matches the `departureWeekdays` and `departureTimeOfDay` if 
*any* journey of the regular trip matches the time and day constraint.

The `webUrl` attribute in the response SHOULD link to a page giving an 
overview of the regular trips (e.g. a "driver" page) and allowing to continue 
the process of booking regular trips by the user (booking by deeplink).

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
