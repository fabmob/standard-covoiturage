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

##### General workflow: Providing the link

The booking flow with a deep link starts when a passenger clicks on a journey 
search result in the MaaS app. This search result MUST open the deep link 
contained in the 
[webUrl](https://github.com/fabmob/standard-covoiturage/pull/2/files#diff-c722233128f788ea06650bffef56e418732898441b4e2199997c40e9070e3345R269) 
parameter returned by a 
[Journey](https://github.com/fabmob/standard-covoiturage/pull/2/files#diff-c722233128f788ea06650bffef56e418732898441b4e2199997c40e9070e3345R220) 
object. This parameter MUST be provided by the carpool operator responsible 
for their implementation of the search API public specification. It contains 
the full URL and its corresponding parameters needed for the mobile device to 
redirect the passenger from the app displaying the search results to the 
operator's app or website.

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
clicking on the deep link SHOULD automatically and seamlessly redirect the 
passenger to the operator's booking flow. It is up to the operator to decide 
where to start this flow and what to do with the parameters sent to the MaaS 
app in the search to provide the passenger with the best booking experience.  
It is up to the passenger to decide whether to book the clicked journey or 
not.

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

##### General workflow: transport operator pushes information to MaaS platform (booking feed flow)

The booking feed flow starts as soon as a booking is created. It sends booking 
data back to the MaaS platform. It allows the MaaS platform to support 
uses-cases for which booking data need to be instantly available (real-time 
reporting, incentive program, pricing bundles,...).

Once a booking is created, the operator SHOULD send the booking details to the 
MaaS platform. The operator SHOULD also notify any update of the booking to 
the MaaS plateform.

The passenger SHOULD have explicitely accepted that the operator shares the 
details of its bookings with the MaaS platform, in accordance with the 
applicable data-privacy regulations.

The passenger MUST be authenticated in the operator plateform using an [Open 
ID Connect 1.0](https://openid.net/specs/openid-connect-core-1_0.html) 
identity layer where the Provider is the MaaS platform.

The MaaS platform MUST provide an API endpoint matching the 
[UPSERT_ENDPOINT_LINK_PLACEHOLDER] specification.

The operator SHOULD call the [UPSERT_ENDPOINT_LINK_PLACEHOLDER] endpoint each 
time a booking involving the passenger is created or the booking data have 
changed.

##### MaaS platform pulls information from transport operator

It uses the same routes as Integrated booking by API to share the status of 
the booking with the MaaS platform : 

- GET /bookings/{bookingId} : get the state of the booking

### 3.3. Authentication across platforms ("MaaS Connect")

TODO link to the connect spec
