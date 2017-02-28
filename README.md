Mobility Arrival and Departure API
==================================

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This document describes the **Mobility Arrival and Departure API**. This API
can be implemented by a sending HEI if it wants to **allow** receiving HEIs to
send arrival and departure dates of particular students via an API (as opposed
to do doing so by phone or by email).

The new arrival and/or departure dates are NOT REQUIRED to be saved
immediately. This means that the results of calling this API don't have to be
immediately reflected in mobility's `actual-arrival-date` and
`actual-departure-date` values in the Outgoing Mobilities API response. Some
sending HEIs MAY simply choose to notify their IRO members instead, so that the
request gets reviewed by a human first. The chosen strategy SHOULD be described
in the `<user-notes>` element in the manifest entry.


Request method
--------------

 * Requests MUST be made with HTTP POST method. Servers SHOULD reject all other
   request methods.


Request parameters
------------------

Parameters MUST be provided in the regular `application/x-www-form-urlencoded`
format.


### `sending_hei_id` (required)

An identifier of the institution which is the sending partner of the mobility.
This parameter MUST be required by the server even if it covers only a single
institution.


### `mobility_id` (required)

Identifier of the mobility which is to be modified. The sending partner of this
mobility MUST match the HEI referenced in the `sending_hei_id` parameter.


### `actual_arrival_date` (optional)

The new value for the `<actual-arrival-date>` of this mobility. If given, then
it MUST contain a date in the `YYYY-mm-dd` format.


### `actual_departure_date` (optional)

The new value for the `<actual-departure-date>` of this mobility. If given,
then it MUST contain a date in the `YYYY-mm-dd` format.


Permissions
-----------

All requests from the EWP Network MUST be allowed access to this API. Consult
the [Echo API][echo] specs for details on handling unprivileged requests.


Handling of invalid parameters
------------------------------

 * General [error handling rules][error-handling] apply.

 * If the requester does not cover the receiving HEI of `mobility_id`, then the
   server MUST respond with HTTP 400 error response.

 * If the request doesn't contain either `actual_arrival_date` nor
   `actual_departure_date` parameter, then the server MAY respond with HTTP
   400 error response, or with HTTP 200 response.

 * If the dates are provided in a wrong format, then the server MUST respond
   with HTTP 400 error response. It is RECOMMENDED to include user-messages in
   this response.

 * If the dates provided in the request don't meet some other "sanity" checks
   required by the server (and they cannot be saved), then the server MAY
   respond with HTTP 400 error response. It is REQUIRED to include
   user-messages in such a response, so that the end user gets notified about
   the reason for this rejection.


Response
--------

Servers MUST respond with a valid XML document described by the [response.xsd]
(response.xsd) schema. See the schema annotations for further information.


[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery
[echo]: https://github.com/erasmus-without-paper/ewp-specs-api-echo
[error-handling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#error-handling
