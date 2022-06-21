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

### 1.4. Functional blocks and standard compliance

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

##### General Workflow

Endpoints implemented by the carpooling operator:
 
- `GET /driver_journeys` and/or
- `GET /passenger_journeys`

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

- `GET /driver_regular_trips` and/or
- `GET /passenger_regular_trips`

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

This specifications provides two flows for implementing an initial booking 
transaction : 

- Integrated booking by API: the initial booking is handled by an API exchange 
  between MaaS platforms and operators. Users stay on their original 
  application, without having to switch to another, and they don't need to be 
  authenticated with the carpooling operator.
- Delegated booking by deep link: booking is handled by switching 
  applications, with a deep link to access directly to the right page on the 
  carpooling operator application. The user needs to authenticate with the 
  carpooling operator.

MaaS platforms and carpooling operators MUST support at least one of these two 
flows if they implement booking. They MAY implement both.

#### 3.2.1. Flow independent specifications 

Both flows share a common pattern: a `*Booking` object with unique 
`booking_id` is shared between the MaaS platform and the carsharing operator, 
and its status is subsequently updated (some details of this object may differ 
depending on the flow).  However, these operations are performed differently 
depending on the selected booking flow.

All information of a `Booking` object apart from its `status` attribute are 
definitely fixed at creation. For both flows, if any information apart from 
`status` needs to be changed, the carsharing operator or MaaS platform MUST 
create a new `Booking` with a new `booking_id`, and MUST update the status of 
the former booking to "CANCELLED".

For both flows, a status change MUST update the `status` to a subsequent 
status respecting the following order: [ WAITING_CONFIRMATION, CONFIRMED,  
COMPLETED_PENDING_VALIDATION, VALIDATED, CANCELLED ]. `status` update requests 
that do not comply with this rule SHOULD be dismissed (they may be the result 
of an order of receipt different from that of the requests sending).

#### 3.2.2. Integrated booking by API

Integrated booking by API is based on 3 routes described in the [OpenAPI 
specification](standard-covoiturage_openapi.yaml) : 

- POST /bookings : initialize the booking request
- PATCH /bookings : modify the state of the booking status to complete the booking flow 
- GET /bookings/{bookingId} : get the state of the booking

#### 3.2.3. Delegated booking by deep link

This section provides precise guidelines for a carpooling operator and a MaaS 
platform to implement a "booking" flow for passengers with deep linking, in 
addition to the above flow independent specifications.

There is no specific API endpoint associated to this flow, but the MaaS 
platform that wishes to retrieve the booking information SHOULD implement the 
booking feed functional block. In this case, the carpooling operator MUST 
comply to the booking feed specification.

##### Redirection towards the carsharing operator

The transport operator supporting deep linking MUST provide for each `Journey` 
shared with the MaaS platform an attribute `webUrl`, with the full URL (with 
all needed parameters) for the user's device to redirect the passenger from 
the app of the MaaS platform to the operator's app or website.

The booking flow with a deep link is initiated by the MaaS platform with a 
`Journey` object provided by a transport operator (e.g. as a result of a 
previous search). To do that, it MUST redirect the user to the deep link 
contained in the 
[webUrl](https://github.com/fabmob/standard-covoiturage/pull/2/files#diff-c722233128f788ea06650bffef56e418732898441b4e2199997c40e9070e3345R269) 
parameter of the
[Journey](https://github.com/fabmob/standard-covoiturage/pull/2/files#diff-c722233128f788ea06650bffef56e418732898441b4e2199997c40e9070e3345R220) 
object.

If the operator is providing an app (and not a website), one obstacle to 
redirecting the passenger is whether they have the operator's app already 
installed in their mobile device or not. If not, the deep link URL SHOULD be 
capable of redirecting the passenger to the corresponding store instead. At 
the operator's discretion, this URL can also temporarily store the URL 
parameters for later retrieval. There are third-party services as well that 
can provide such a solution. This way, after the app is installed, the 
parameters can be recovered to automatically redirect the passenger to the 
booking flow as if the app had always been installed.

If the operator provides a website or if its app was already installed, then 
following the deep link SHOULD automatically and seamlessly redirect the 
passenger to the operator's booking flow. It is up to the operator to decide 
where to start this flow and what to do with the parameters sent to the MaaS 
app in the search to provide the passenger with the best booking experience.  

It is up to the passenger to decide whether to book the journey, any other 
journey or none.

If the passenger decides not to book the journey being displayed (or if is not 
available anymore), the operator MAY provide the means for the passenger to go 
back to the search results on the MaaS app. It is then up to the passenger to 
decide whether to select another result and repeat the above process. If they 
decide to book it, it is at the operator's discretion to decide how to 
continue guiding the passenger through the booking process.


~~~mermaid
flowchart LR
    A[Display trip<br>results<br>on MaaS] -->|Passenger clicks<br>on result| B{Is operator app <br/>installed?}
    B -->|Yes| D
    B -->|No| K["Redirect to store<br>(Search parameters<br>are preserved)"]
    K -->|App installed| D[Display<br>trip details]
    D --> E{Passenger<br>booked<br>trip?}
    E -.->|"No, back to results"| A
    E -.-> |Yes| F[Operator sends<br>booking details<br>to MaaS]
    F -.-> |Back to MaaS| G((Display trip<br>booked))
~~~

#### 3.2.4. Booking information push from carsharing operator to MaaS platform (booking feed flow)

The booking feed is the channel by which the transport operator sends 
information to the MaaS platform, about the booking events happening on the 
transport operator website or app. The booking feed mechanism relies on an 
[Open ID Connect 1.0](https://openid.net/specs/openid-connect-core-1_0.html) 
identity layer where the Provider is the MaaS platform, subsequently referred 
to as "MaaS connect". See [the dedicated 
section](#33-authentication-across-platforms-maas-connect) for further 
information. This section adds up to the above flow independent 
specifications.

The MaaS platform needs to implement the following endpoint:

- `POST /booking_events`

A transport operator offering booking by deep link MUST accept authentication 
via "MaaS connect".  

A MaaS platform SHOULD provide a "MaaS Connect" identity layer in order to get 
back information on booking events happening on the platform of the transport 
operator. In that case, it also MUST provide an API endpoint matching the 
`POST /booking_events` specification. Moreover, the passenger MUST be 
authenticated using MaaS Connect at the time of the redirection to allow the 
booking feed.

The booking feed flow starts as soon as a booking is created, regardless of 
whether the trip was first consulted on the MaaS app. It sends booking data to 
the MaaS platform. It allows the MaaS platform to support use-cases for which 
booking data need to be instantly available (real-time reporting, incentive 
program, pricing bundles,...).

If the MaaS platform is offering the `POST /booking_events` endpoint, and once 
a booking is created, the operator MUST send the booking details to the MaaS 
platform. The operator MUST also notify every `status` update of the booking 
to the MaaS plateform. As specified in [the introduction of the booking 
section](#32-booking), every other change MUST be operated by creating a new `Booking` 
object and udating the `status` of the former booking to "CANCELLED".  

A status update with the `POST /booking_events` endpoint MUST repeat the whole 
`Booking` object each time as redunduncy allows for a better resilience to 
communication failures (e.g. a status update event can be processed even if 
the booking creation event has been missed).


### 3.3. Authentication across platforms ("MaaS Connect")

The "MaaS Connect" authentication mechanism SHOULD ensure that the user 
explicitely accepts that the operator shares the details of its bookings with 
the MaaS platform, in accordance with the applicable data-privacy regulations.

TODO link to the connect spec
