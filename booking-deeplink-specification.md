# Indroduction

This specification provides precise guidelines for a carpooling operator and a MaaS platform to implement a "booking" flow for passengers, with information to complete the booking shared via deeplinking between the two.

## Requirements Notation and Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "PROVIDERTIONAL" in this document are to be interpreted as described in [RFC 2119](http://tools.ietf.org/html/rfc2119).

## Functional overview

The booking flow with a deep link starts when a passenger clicks on a journey search result in the MaaS app. This search result MUST open the deep link contained in the [DeepLink](https://github.com/fabmob/standard-covoiturage/pull/2/files#diff-c722233128f788ea06650bffef56e418732898441b4e2199997c40e9070e3345R321-R322) parameter returned by a [Journey](https://github.com/fabmob/standard-covoiturage/pull/2/files#diff-c722233128f788ea06650bffef56e418732898441b4e2199997c40e9070e3345R220) object. This parameter MUST be provided by the carpool operator responsible for their implementation of the search API public specification. It contains the full URL and its corresponding parameters needed for the mobile device to redirect the passenger from the app displaying the search results to the operator's app.

One obstacle to redirecting the passenger is whether they have the operator's app already installed in their mobile device or not. If not, the deep link URL SHOULD be capable of redirecting the passenger to the corresponding store instead. At the operator's discretion, this URL can also temporarily store the URL parameters for later retrieval. There are third-party services as well that can provide such a solution. This way, after the app is installed, the parameters can be recovered to automatically redirect the passenger from the onboarding to the booking flow as if the app had always been installed.

If the app was already installed and the passenger was already registered with the operator service, then clicking on the deep link will automatically and seamlessly redirect the passenger to the operator's booking flow. It is up to the operator to decide where to start this flow and what to do with the parameters sent to the MaaS app in the search to provide the passenger with the best booking experience. It is up to the passenger to decide whether to book the clicked journey or not.

If the passenger decides to not to book the journey being displayed (or it is not anymore available), the operator SHOULD provide the means for the passenger to go back to the search results. It is then up to the passenger to decide whether to select another result and repeat the above process. If they decide to book it, it is at the operator's discretion to decide how to continue guiding the passenger through the booking process. Once the booking is confirmed, the operator MUST send the booking details back to the MaaS if the following conditions are met. One, the passenger's account on the operator platform is linked to the MaaS using the [Connect specification](https://github.com/klaxit/open-carpooling-service-specification/blob/master/connect-specification.md). Two, the MaaS must provide an implementation of the booking API specification (tbd) for the operator to consume.

## Flow summary

<img src="booking-flow.png"
     alt="Booking Flow"
     style="width: 800px" />
[Source](https://mermaid-js.github.io/mermaid-live-editor/edit#pako:eNpdkUtvwjAQhP_KyieQQL1HFRUoUFX0JThVmMMSL2Dh2JbttEIJ_712HhVqTlY8-83MumaFEcQydlTmpzijC_C64RriN9_l0luFVwhO2seDmznylQo-HY2GN8TtHqbTWfOJ3pM-kYNCyeIyCDp5A4v6xYOx5DAYB2gtRMHDTGofUCkST7fOcNHCvsg3kN__eTcNrHecbUhIR0WAYMBHFCWf0ZbQFWew6LCkQK51R0dgoz-5bxJjzvYdb93y5jHBn3n0GnqmwVQVBAWUyvdDeRqCZf3XMukOxlxIDBNDg2WfdwIHLC4pZ7-yBuZ3CuhKrnYfw1IiWPiBK_VpiNAamG7VHWDVARY9P9008Dwa_X-rLuB4zDWbsJJciVLEZ64ThbNwppI4y-JR0BFjRM64vkVpZQUGWgoZY7HsiMrThGEVzPaqC5YFV9EgyiWe4tJ71e0XgNzGkA)