
# Curb Data Specification: General Information

This document contains specifications that are shared between the various CDS APIs.

# Table of Contents

- [Authorization](#authorization)
- [Beta Features](#beta-features)
- [Costs and Currencies](#costs-and-currencies)
- [Geographic Data](#geographic-data)
  - [Geographic Telemetry Data](geographic-telemetry-data)
  - [Polygon](#polygon)
  - [Intersection Operation](#intersection-operation)
- [Responses](#responses)
- [REST Endpoints](#rest-endpoints)
- [Pagination](#pagination)
- [UUID](#uuid)
- [Timestamp](#timestamp)
- [Versioning](#versioning)

# Authorization

CDS implementers **SHALL** provide authorization for API endpoints via a bearer token based auth system, when required.

For example, the `Authorization` header sent as part of an HTTP request:

```
GET /events/event HTTP/1.1
Host: api.operator.com
Authorization: Bearer <token>
```

More info on how to document [Bearer Auth in swagger](https://swagger.io/docs/specification/authentication/bearer-authentication/)

## JSON Web Tokens

JSON Web Token ([JWT](https://jwt.io/introduction/)) is **RECOMMENDED** as the token format.

JWTs provide a safe, secure way to verify the identity of an agency and provide access to MDS resources without providing access to other, potentially sensitive data.

> JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.

Implementers **MAY** include any metadata in the JWT they wish that helps to route, log, permission, or debug agency requests, leaving their internal implementation flexible.

JWT provides a helpful [debugger](https://jwt.io/#debugger) for testing your token and verifying security.

## OAuth 2.0

OAuth 2.0's `client_credentials` grant type (outlined in [RFC6749](https://tools.ietf.org/html/rfc6749#section-4.4)) is **RECOMMENDED** as the authentication and authorization scheme.

OAuth 2.0 is an industry standard authorization framework with a variety of existing tooling. The `client_credentials` grant type facilitates generation of tokens that can be used for access by agencies and distributed to data partners.

If implementers use this auth scheme, they **MAY** choose to specify token scopes that define access parameters like allowable time ranges. These guidelines **SHOULD** be encoded into the returned token in a parseable way.

## Endpoint Authentication Requirements  

All endpoints marked authenticated **SHALL** be authenticated, to protect potentially sensitive information.

# Beta Features

In some cases, features within CDS may be marked as "beta." These are typically recently added endpoints or fields. Because beta features are new, they may not yet be fully mature and proven in real-world operation. The design of beta features may have undiscovered gaps, ambiguities, or inconsistencies. Implementations of those features are typically also quite new and are more likely to contain bugs or other flaws. Beta features are likely to evolve more rapidly than other parts of the specification.

Despite this, CDS users are highly encouraged to use beta features. New features can only become proven and trusted through implementation, use, and the learning that comes with it. Users should be thoughtful about the role of beta features in their operations. Users of beta features are strongly encouraged to share their experiences, learnings, and challenges with the broader CDS community via GitHub issues or pull requests. This will inform the refinements that transform beta features into fully proven, stable parts of the specification. 

Beta features may be suitable for enabling some new tools and analysis, but may not be appropriate for mission-critical applications or regulatory decisions where certainty and reliability are essential. In subsequent releases existing beta features may include breaking changes, even in a minor release.

Working Groups and their Steering Committees are expected to review beta designated features and feedback with each release cycle and determine whether the feature has reached the level of stability and maturity needed to remove the beta designation. In a case where a beta feature fails to reach substantial adoption after an extended time, Working Group Steering Committees should discuss whether or not the feature should remain in the specification.

[Top][toc]

# Costs and currencies

Fields specifying a monetary cost use a currency as specified in [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217#Active_codes). All costs should be given as integers in the currency's smallest unit. As an example, to represent $1 USD, specify an amount of 100 (100 cents).

If the currency field is null, USD cents is implied.

[Top][toc]

# Definitions

Defining terminology and abbreviations used throughout CDS.

- API - Application Programming Interface - A function or set of functions/endpoints that allow one software application to access or communicate with features of a different software application or service.
- API Endpoint - A point at which an API connects with a software application or service.
- Curb Places - One of the defined Curb API areas: Areas, Zones, or Spaces.
- Curb Area - A geographically defined area around one or more Curb Zones to track releated activity before and after Curb Zone use.
- Curb Zone - A geographically and policy defined area where transactions may happen at curb space.
- Curb Space - A geographically defined area within a Curb Zone for a single vehicle.

# Event Times

Return data in the order of timestamps of the events on the ground, with most recent items returned first to construct as accurate a timeline as possible.

Because of unreliability of some device clocks and other factors, sensors and operators are unlikely to know with total confidence what time an event occurred at. However, endpoint producers are responsible for constructing as accurate a timeline as possible. Most importantly, the order of the timestamps for a particular vehicle's events must reflect the producer's best understanding of the order in which those events occurred.

# Geographic Data

References to geographic datatypes (Point, MultiPolygon, etc.) imply coordinates encoded in the [WGS 84 (EPSG:4326)][wgs84] standard GPS or GNSS projection expressed as [Decimal Degrees][decimal-degrees]. 

## Geographic Telemetry Data

Whenever a vehicle or device location coordinate measurement is presented, it must be represented as a GeoJSON [`Feature`][geojson-feature] object with a corresponding `properties` object with the following properties:


| Field          | Type            | Required/Optional     | Field Description                                            |
| -------------- | --------------- | --------------------- | ------------------------------------------------------------ |
| `timestamp`    | [timestamp][ts] | Required              | Date/time that event occurred. Based on GPS or GNSS clock |
| `altitude`     | Double          | Required if Available | Altitude above mean sea level in meters |
| `heading`      | Double          | Required if Available | Degrees - clockwise starting at 0 degrees at true North |
| `speed`        | Float           | Required if Available | Estimated speed in meters / sec as reported by the GPS chipset |
| `accuracy`     | Float           | Required if Available | Horizontal accuracy, in meters |
| `hdop`         | Float           | Required if Available | Horizontal GPS or GNSS accuracy value (see [hdop][hdop]) |
| `satellites`   | Integer         | Required if Available | Number of GPS or GNSS satellites |

Example of a vehicle location GeoJSON [`Feature`][geojson-feature] object:

```json
{
    "type": "Feature",
    "properties": {
        "timestamp": 1529968782421,
        "accuracy": 10,
        "speed": 1.21
    },
    "geometry": {
        "type": "Point",
        "coordinates": [
            -118.46710503101347,
            33.9909333514159
        ]
    }
}
```

## Polygon

A polygon is a GeoJSON geometry of type `"Polygon"` as defined in
[RFC 7946 3.1.6](https://www.ietf.org/rfc/rfc7946.txt). An example polygon is:

```
{
  "type": "Polygon",
  "coordinates": [[
    [-73.982105, 40.767932],
    [-73.973694, 40.764551],
    [-73.949318, 40.796918],
    [-73.958416, 40.800686],
    [-73.982105, 40.767932]
  ]]
}
```

## Intersection Operation

For the purposes of this specification, the intersection of two geographic datatypes is defined according to the [`ST_Intersects` PostGIS operation][st-intersects]

> If a geometry or geography shares any portion of space then they intersect. For geography -- tolerance is 0.00001 meters (so any points that are close are considered to intersect).
>
> Overlaps, Touches, Within all imply spatial intersection. If any of the aforementioned returns true, then the geometries also spatially intersect. Disjoint implies false for spatial intersection.

[Top][toc]

# Responses

List of acceptable endpoint responses.

* **200:** OK: operation successful.
* **201:** Created: `POST` operations, new object created
* **400:** Bad request.
* **401:** Unauthorized: Invalid, expired, or insufficient scope of token.
* **404:** Not Found: Object does not exist, returned on `GET` or `POST` operations if the object does not exist.
* **406:** Not Acceptable. Invalid response.
* **409:** Conflict: `POST` operations when an object already exists and an update is not possible.
* **500:** Internal server error: In this case, the answer may contain a `text/plain` body with an error message for troubleshooting.
* **501:** Not Implemented. 

## Error Messages

```json
{
    "error": "...",
    "error_description": "...",
    "error_details": [ "...", "..." ]
}
```

| Field               | Type     | Field Description      |
| ------------------- | -------- | ---------------------- |
| `error`             | String   | Error message string   |
| `error_description` | String   | Human readable error description (can be localized) |
| `error_details`     | String[] | Array of error details |

[Top][toc]

# REST Endpoints

All dynamic REST endpoints will return a JSON object containing the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `data` | _Endpoint-dependent_ | Required | The requested data objects. |
| `version` | String | Required | The specification version that the API conforms to (currently, `0.0`) |
| `time_zone` | String | Required | The time zone that applies to parking regulations in this dataset. MUST be a valid [TZ database](https://www.iana.org/time-zones) time zone name (e.g. `"US/Eastern"` or `"Europe/Paris"`). |
| `last_updated` | [Timestamp][#timestamp] | Required | The last time the data in this API was updated. |
| `currency` | String | Required | The ISO 4217 3-letter code for the currency in which rates for curb usage are denominated. All costs should be given as integers in the currency's smallest unit. As an example, to represent $1 USD, specify an amount of 100 (for 100 cents). |
| `author` | String | Optional | The name of the organization that produces and maintains this data. |
| `license_url` | URL | Optional | The licensing terms under which this data is provided. |

Servers MUST set the `Content-Type` header to `application/vnd.cds+json;version=1.0` to support
versioning in the future.  Clients SHOULD specify an `Accept` header containing 
`application/vnd.cds+json;version=1.0`. If the server receives a request that contains an `Accept`
header but does not include this value; it MUST respond with a status of `406 Not Acceptable`.

[Top][toc]

# Pagination

Endpoints may use pagination, which must comply with the [JSON API][json-api-pagination] specification. See [Event Times](/general-information.md#event-times) guideance about the order of data returned.

The following keys must be used for pagination links:

* `first`: url to the first page of data
* `last`: url to the last page of data
* `prev`: url to the previous page of data
* `next`: url to the next page of data

At a minimum, payloads that use pagination must include a `next` key, which must be set to `null` to indicate the last page of data.

```json
{
    "version": "x.y.z",
    "data": {
        "endpoint": [{
            "field_name": "..."
        }]
    },
    "links": {
        "first": "https://...",
        "last": "https://...",
        "prev": "https://...",
        "next": "https://..."
    }
}
```

[Top][toc]

# UUID

A UUID is a 128-bit, globally unique identifier represented as a string using the format defined in
[RFC 4122](https://www.ietf.org/rfc/rfc4122.txt). An example UUID is
`"98bd30e9-14cc-4e71-bee2-7bab0f28b2bd"`. UUIDs used in the Curbs API may be of any format described
in RFC 4122, including time-based (V1), random (V4), or name-based (V5).

[Top][toc]

# Timestamp

A timestamp is an integer representing a number of milliseconds since midnight, January 1st, 1970 UTC
(the UNIX epoch).

[Top][toc]

# Versioning

CDS APIs must handle requests for specific versions of the specification from clients.

Versioning must be implemented through the use of a custom media-type, `application/vnd.cds+json`, combined with a required `version` parameter.

The version parameter specifies the dot-separated combination of major and minor versions from a published version of the specification. For example, the media-type for version `1.0.1` would be specified as `application/vnd.cds+json;version=1.0`

Clients must specify the version they are targeting through the `Accept` header. For example:

```http
Accept: application/vnd.cds+json;version=1.2.0
```

If an unsupported or invalid version is requested, the API must respond with a status of `406 Not Acceptable`.

[Top][toc]

[decimal-degrees]: https://en.wikipedia.org/wiki/Decimal_degrees
[hdop]: https://en.wikipedia.org/wiki/Dilution_of_precision_(navigation)
[geo]: #geographic-data
[geojson-feature]: https://tools.ietf.org/html/rfc7946#section-3.2
[geojson-point]: https://tools.ietf.org/html/rfc7946#section-3.1.2
[st-intersects]: https://postgis.net/docs/ST_Intersects.html
[toc]: #table-of-contents
[ts]: /general-information.md#timestamps
[wgs84]: https://en.wikipedia.org/wiki/World_Geodetic_System
