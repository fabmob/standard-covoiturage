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

TODO explain driver/passenger journeys and link to OpenAPI spec

#### 3.1.2. Search for Regular trips and associated journeys

TODO explain driver/passenger regular trips and link to OpenAPI spec

### 3.2. Booking

This specifications provides 2 flows for implementing an initial booking transaction : 

- Integrated booking by API : the initial booking is handled by an API exchange between MaaS platforms and operators. Users stay on their original application, without having to switch to another.
- Delegated booking by deep link : booking is handled by switching 
  applications, with a deep link to access directly to the right page on the 
  carpooling operator application.

These 2 flows share a common syntax for operations after the initial booking 
occurs. Integrated booking by API can still leverage deep link information to 
access the operator's application while Delegated booking by deep link uses 
API endpoints to provide information of the evolution of the booking process 
to the MaaS platform.

MaaS platforms and carpooling operators MUST support at least one of these 2 flows if they implement booking.

They MAY implement both.

Both flows share a common pattern: a `Booking` object with unique `booking_id` 
is shared between the MaaS platform and the carsharing operator.  All 
information of a `Booking` object apart from its `status` attribute are 
definitely fixed at creation. For both flows, if any information apart from 
`status` needs to be changed, the carsharing operator or MaaS platform MUST 
create a new `Booking` with a new `booking_id`, and MUST update the status of 
the former booking to "CANCELLED".

#### 3.2.1. Integrated booking by API

Integrated booking by API is based on 3 routes described in the [OpenAPI 
specification](standard-covoiturage_openapi.yaml) : 

- POST /bookings : initialize the booking request
- PATCH /bookings : modify the state of the booking status to complete the booking flow 
- GET /bookings/{bookingId} : get the state of the booking

#### 3.2.2. Delegated booking by deep link

This section provides precise guidelines for a carpooling operator and a MaaS 
platform to implement a "booking" flow for passengers, with information to 
complete the booking shared via deep linking between the two and booking 
information sent back to the MaaS platform.

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

##### Information push from carsharing operator to MaaS platform (booking feed flow)

The booking feed is the channel by which the transport operator sends 
information to the MaaS platform, about the booking events happening on the 
transport operator website or app. The booking feed mechanism relies on an 
[Open ID Connect 1.0](https://openid.net/specs/openid-connect-core-1_0.html) 
identity layer where the Provider is the MaaS platform, subsequently referred 
to as "MaaS connect". See [the dedicated section](#33-authentication-across-platforms-maas-connect) for further information.

A transport operator offering booking by deep link MUST accept 
authentification via "MaaS connect".  

A MaaS platform SHOULD provide a "MaaS Connect" identity layer in order to get 
back information on booking events happening on the platform of the transport 
operator. In that case, it also MUST provide an API endpoint matching the 
`POST /booking_events` specification. Moreover, the passenger MUST be 
authenticated using MaaS Connect at the time of the redirection to allow the 
booking feed.

The booking feed flow starts as soon as a booking is created. It sends booking 
data back to the MaaS platform. It allows the MaaS platform to support 
use-cases for which booking data need to be instantly available (real-time 
reporting, incentive program, pricing bundles,...).

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

The passenger SHOULD have explicitely accepted that the operator shares the 
details of its bookings with the MaaS platform, in accordance with the 
applicable data-privacy regulations.


##### Information pull by th MaaS platform operator

The MaaS platform operator can also pull information on a given booking.

To do that, it uses the same routes as Integrated booking by API to query the 
carsharing operator for the status of the booking: `GET /bookings/{bookingId}`.

### 3.3. Authentication across platforms ("MaaS Connect")

TODO link to the connect spec
