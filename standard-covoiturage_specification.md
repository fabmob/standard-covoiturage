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

Both flows share a common pattern: a `Booking` object with unique `booking_id` 
is shared between the MaaS platform and the carsharing operator, and its 
status is subsequently updated. However, these operations are performed 
differently depending on the selected booking flow.

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

* `POST /booking_events`

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
