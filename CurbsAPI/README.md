# Curbs API

_Version 0.0 (DRAFT)_

The Curbs API is a REST API allowing cities to specify areas of interest along the curb along with
the rules for using them: who is allowed to park, load, unload, pick up, drop off, etc. at the curb,
for how long, for what price (if any), at what times, and on what days. Locations defined in the
Curbs API can be connected to event and metric data and shared with companies and the public, for
purposes such as routing, finding legal parking, loading, and pick-up/drop-off spots, or analyzing
curb utilization over time.

There are three different levels of geography that can be specified in the Curbs API:

  - A [Curb Zone](#curb-zone) is a contiguous region of curb space on a single block face that is
    regulated in a particular way. Defining curb zones is *required* as part of the Curbs API.
  - A [Curb Space](#curb-space) is a defined vehicle parking spot along the curb for use by one
    vehicle at a time. Curb spaces are *optional*.
  - A [Curb Area](#curb-area) is a larger area of interest, such as a neighborhood or corridor, that
    could be used to show proximity, approaches, conflicts, circling, or other activity. Curb areas
    are *optional*.

## Table of Contents

- [REST Endpoints](#rest-endpoints)
  * [Query Curb Zones](#query-curb-zones)
  * [Query Curb Areas](#query-curb-areas)
  * [Query Curb Spaces](#query-curb-spaces)
  * [Query Curb Policies](#query-curb-policies)
  * [Fetch a Curb Zone](#fetch-a-curb-zone)
  * [Fetch a Curb Area](#fetch-a-curb-area)
  * [Fetch a Curb Space](#fetch-a-curb-space)
  * [Fetch a Curb Policy](#fetch-a-curb-policy)
- [Common Types](#common-types)
  * [UUID](#uuid)
  * [Timestamp](#timestamp)
  * [Polygon](#polygon)
- [Data Objects](#data-objects)
  * [Curb Zone](#curb-zone)
  * [Curb Area](#curb-area)
  * [Curb Space](#curb-space)
  * [Policy](#policy)

## REST Endpoints

All endpoints return a JSON object containing the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `data` | _Endpoint-dependent_ | Required | The requested data objects. |
| `version` | String | Required | The specification version that the API conforms to (currently, `0.0`) |
| `time_zone` | String | Required | The time zone that applies to parking regulations in this dataset. MUST be a valid [TZ database](https://www.iana.org/time-zones) time zone name (e.g. `"US/Eastern"` or `"Europe/Paris"`). |
| `last_updated` | Timestamp | Required | The last time the data in this API was updated. |
| `currency` | String | Required | The ISO 4217 3-letter code for the currency in which rates for curb usage are denominated. |
| `author` | String | Optional | The name of the organization that produces and maintains this data. |
| `license_url` | URL | Optional | The licensing terms under which this data is provided. |

Servers MUST set the `Content-Type` header to `application/vnd.cds+json;version=0.0` to support
versioning in the future.  Clients SHOULD specify an `Accept` header containing 
`application/vnd.cds+json;version=0.0`. If the server receives a request that contains an `Accept`
header but does not include this value; it MUST respond with a status of `406 Not Acceptable`.

[Top][toc]

###  Query Curb Zones

Endpoint: `/curbs/zone`
Method: `GET`
`data` Payload: a JSON object with the following fields:
  - `zones`: an array of [Curb Zone](#curb-zone) objects
  - `policies`: an array of [Policy](#policy) objects

_This endpoint must be implemented by every Curbs API server._

#### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `area`       | [UUID][uuid]      | The ID of a [Curb Area](#curb-area). If specified, only return Curb Zones contained within this area. |
| `min_lat`<br/>`min_lng`<br/>`max_lat`<br/>`max_lng` | Numeric | Specifies a bounding box; if one of these parameters is specified, all four MUST be. If specified only return Curb Zones that intersect the supplied bounding box. |
| `lat`<br/>`lng`<br/>`radius` | Numeric | If one of these parameters is specified, all three MUST be. Returns only Curb Zones that are within `radius` centimeters of the point identified by `lat`/`lng`. Curb Zones in the response MUST be ordered ascending by distance from the center point. |
| `include_geometry` | `true`/`false` |  If the value is `false`, do not include the `geometry` field within the [Curb Zone](#curb-zone) feature object. |
| `include_policies` | `true`/`false` | If the value is `false`, do not include the `policies` field within the response. |
| `time` | [Timestamp][ts] | Only Curb Zone objects whose validity period includes this time will be returned; availability data (if supplied) will be returned as of this time. |

[Top][toc]

###  Query Curb Areas

Endpoint: `/curbs/area`
Method: `GET`
`data` Payload: a JSON object with an `areas` field containing an array of [Curb Area](#curb-area) objects.

_Optional endpoint; if not implemented, the server should reply with `501 Not Implemented`._

#### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `min_lat`<br/>`min_lng`<br/>`max_lat`<br/>`max_lng` | Numeric | Specifies a bounding box; if one of these parameters is specified, all four MUST be. If specified only return Curb Areas that intersect the supplied bounding box. |
| `lat`<br/>`lng`<br/>`radius` | Numeric | If one of these parameters is specified, all three MUST be. Returns only Curb Areas that are within `radius` centimeters of the point identified by `lat`/`lng`. Curb Areas in the response MUST be ordered ascending by distance from the center point. |

[Top][toc]

### Query Curb Spaces

Endpoint: `/curbs/space`
Method: `GET`
`data` Payload: a JSON object with a `spaces` field containing an array of [Curb Space](#curb-space) objects.

_Optional endpoint; if not implemented, the server should reply with `501 Not Implemented`._

#### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `zone`       | [UUID][uuid]      | The ID of a [Curb Zone](#curb-zone). If specified, only return Curb Spaces contained within this zone. |
| `min_lat`<br/>`min_lng`<br/>`max_lat`<br/>`max_lng` | Numeric | Specifies a bounding box; if one of these parameters is specified, all four MUST be. If specified only return Curb Zones that intersect the supplied bounding box. |
| `lat`<br/>`lng`<br/>`radius` | Numeric | If one of these parameters is specified, all three MUST be. Returns only Curb Spaces that are within `radius` centimeters of the point identified by `lat`/`lng`. Curb Spaces in the response MUST be ordered ascending by distance from the center point. |
| `time` | [Timestamp][ts] | Availability data (if supplied) will be returned as of this time. |

[Top][toc]

### Query Curb Policies

Endpoint: `/curb/policy`
Method: `get`
`data` Payload: a JSON object with a `policies` field containing an array of [Curb Policy](#curb-policy) objects.

_Optional endpoint; if not implemented, the server should reply with `501 Not Implemented`._

#### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `ids`        | Comma-separated list of [UUIDs][uuid] | If present, return only policies with the supplied UUIDs. Otherwise, return all policies. |

[Top][toc]

###  Fetch a Curb Zone

Endpoint: `/curbs/zone/<id>`
Method: `GET`
`data` Payload: the [Curb Zone](#curb-zone) object with the ID provided in the path.

_Optional endpoint; if not implemented, the server should reply with `501 Not Implemented`._

#### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `time` | [Timestamp][ts] | The Curb Zone object will only be returned if its validity period includes this time; otherwise, the server should reply with `404 Not Found`. Availability data (if supplied) will be returned as of this time. |

[Top][toc]

###  Fetch a Curb Area

Endpoint: `/curbs/area/<id>`
Method: `GET`
`data` Payload: the [Curb Area](#curb-area) object with the ID provided in the path.

_Optional endpoint; if not implemented, the server should reply with `501 Not Implemented`._

#### Query Parameters

This endpoint takes no query parameters.

[Top][toc]

### Fetch a Curb Space

Endpoint: `/curbs/space/<id>`
Method: `GET`
`data` Payload: the [Curb Space](#curb-space) object with the ID provided in the path.

#### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `time` | [Timestamp][ts] | Availability data (if supplied) will be returned as of this time. |

[Top][toc]

### Fetch a Curb Policy

Endpoint: `/curb/policy/<id>`
Method: `get`
`data` Payload: the [Curb Policy](#curb-policy) object with the ID provided in the path. 

#### Query Parameters

This endpoint takes no query parameters.

[Top][toc]

 
## Common Types

The following types are used throughout the Curbs API.

### UUID

A UUID is a 128-bit, globally unique identifier represented as a string using the format defined in
[RFC 4122](https://www.ietf.org/rfc/rfc4122.txt). An example UUID is
`"98bd30e9-14cc-4e71-bee2-7bab0f28b2bd"`. UUIDs used in the Curbs API may be of any format described
in RFC 4122, including time-based (V1), random (V4), or name-based (V5).

### Timestamp

A timestamp is an integer representing a number of milliseconds since midnight, January 1st, 1970 UTC
(the UNIX epoch).

### Polygon

A polygon is a GeoJSON geometry of type `"Polygon"`  as defined in
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

## Data Objects 

### Curb Zone

A Curb Zone is a geographical entity representing a single region along the curb, along with
metadata about that region and the policies that apply to its use by vehicles.. What constitutes
an individual Curb Zone is determined by the city, but all Curb Zones MUST meet the following
criteria:

  1. Always have a common regulation along their entire extent (i.e., if at certain times of day,
     half of a given stretch of curb is a loading zone and the other half is metered parking, that
     stretch of curb must be divided into at least two Curb Zones);
  1. Never span multiple blocks -- an entire Curb Zone must be on the same street and be between
     the same two cross streets, alleys, or service roads. 
  1. Never overlap other Curb Zones in the same dataset with overlapping validity times. This
     applies to both the zone's polygon geometries and linear references (if used).
  1. Be assigned a unique ID, in the form of a [UUID][uuid]. This ID SHOULD remain consistent as long as
     the Curb Zone's extent and policies remain substantially the same.
  1. It SHOULD NOT be possible to legally park a single vehicle in two different Curb Zone at the 
     same time (i.e., a given non-demarcated parking area or loading zone should be represented as
     a single curb location), unless this conflicts with the requirements above.

A Curb Zone is represented as a JSON object, whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_zone_id` | [UUID][uuid] | Required | TheID of this Curb Zone. |
| `geometry` | [Polygon][polygon] | Required | The spatial extent of this curb zone. |
| `curb_policy_ids` | Array of [UUIDs][uuid] | Required | An array of IDs of [Policy objects](#policy). Together, these define the regulations of this Curb Zone. |
| `start_date` | [Timestamp][ts] | Required | The earliest time that the data for this curb location is known to be valid. This could be the date on which the data was collected, for instance. This MUST never change for a given id. |
| `end_date` | [Timestamp][ts] | Optional | The time at which the data for this curb location ceases to be valid. If not present, the data will be presumed to be valid indefinitely. |
| `location_references` | Array of [Location Reference objects](#location-reference) objects | Optional | One or more linear references for this Curb Zone. |
| `name` | String | Optional | A human-readable name for this Curb Zone that identifies it to end users. |
| `user_zone_id` | String | Optional | An identifier that can be used to refer to this Curb Zone on physical signage as well as within mobile applications, typically for payment purposes. |
| `street_name` | String | Optional | The name of the street that this Curb Zone is on. |
| `cross_street_start_name` | String | Optional | The name of the cross street at or before the start of this Curb Zone (which cross street is at the start and end of the location is defined in the same direction as the linear reference for this curb; if no linear reference is provided, start and end SHOULD be oriented such that start comes before end when moving in the direction of travel for the roadway immediately adjacent to the curb.) |
| `cross_street_end_name` | String | Optional | The name of the cross street at the end of this Curb Zone. |
| `length` | Integer | Optional | The length, in centimeters, of the Curb Zone when projected along the street centerline. Note that this is the definitive length of the curb area, and not the edge length of the geographic polygon. |
| `available_space_lengths`| Array of Integers | Optional | If availability information is present, an array of numbers containing the lengths (in centimeters) of all known non-overlapping available spaces within this Curb Zone. In cases where availability is known less precisely, this data MAY be inferred from a model. |
| `availability_time` | [Timestamp][ts] | Optional | If availability information is present, the timestamp corresponding to the most recent time that availability was computed for this zone. |
| `width` | Integer | Optional | The width, in centimeters, that the Curb Zone occupies from the curb to the roadway lane. |
| `parking_angle` | String | Optional | The angle in which passenger vehicles in this Curb Zone are meant to park. May take one of the following values: <ul><li>`parallel`</li><li>`perpendicular`</li><li>`angled`</li></ul> |
| `num_spaces` | Integer | Optional | The number of demarcated spaces within this Curb Zone. Demarcated spaces may also be specified using [Curb Spaces](#curb-spaces). |
| `street_side` | String | Optional | The cardinal or subcardinal direction representing the side of the roadway that this curb is on. May be `N`, `NE`, `E`, `SE`, `S`, `SW`, `W`, or `NW`. For cities with "grid directions", the side MAY be based on the grid direction rather than the closest true-north compass direction, but MUST NOT be more than 90 degrees away from the true compass direction. |
| `median`| Boolean | Optional | If "true", this curb location is on the median of a street, rather than its edge. A median is a strip of land separating two roadways within the same street. Note that, for medians, `street_side` is interpreted relative to the roadway that the particular curb is on, so the curb along the median of the southern roadway of a divided street would have `street_side` of `N`. |
| `entire_roadway`| Boolean | Optional | If "true", this curb location takes up the entire width of the roadway (which may be impassible for through traffic when the Curb Zone is being used for parking or loading). This is a common condition for alleyways. If `entire_roadway` is `true`, `street_side` MUST NOT be present. |
| `curb_area_id`| [UUID][uuid] | Optional | The ID of the [Curb Area](#curb-area) that this Curb Zone is a part of. If specified, the area identified MUST be retrievable through the Curb API and its geographical area MUST contain that of the Curb Zone. |

#### Curb Zone ID Stability

The `curb_zone_id` field uniquely identifies a particular zone with one particular set of policies.

  - *New ID*: When a Curb Zone ceases to be valid, or it needs substantive changes, its ID
    MUST NOT not be reused by a Curb Zone with different data, even if it occupies the same
    location. If data updates reflect changes in the physical world or agency regulations,
    a new ID MUST be assigned. 
  - *No new ID*: data updates that reflect changes in how the same physical locations or
    regulations are modeled digitally -- e.g., additional metadata, or regulations being modeled
    more accurately -- SHOULD NOT be implemented by ending a Curb Zone's validity and creating a
    new one with a new ID. The existing Curb Zone can be updated silently with the new data;
    callers MAY NOT rely on a Curb Zone with the same ID remaining identical over time.

[Top][toc]

### Curb Area

Defines curb areas in a city and their properties. A Curb Area is a particular neighborhood or area
of interest that includes one or more [Curb Zones](#curb-zone). Important notes about Curb Areas:

  * Curb Areas MAY overlap with other Curb Areas
  * Curb Areas are not meant to be city-wide, and instead should be an area of interest around one
    or more Curb Zones that is no bigger than a neighborhood.

A Curb Area is represented as a JSON object, whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_area_id` | [UUID][uuid] | Required | The ID for the curb area. |
| `geometry` | [Polygon][polygon] | Required | The spatial extent of this curb location. |
| `name` | String | Required | The name of this curb area. |
| `curb_zone_ids` | Array of [UUIDs][uuid] | Required | The IDs of all the Curb Zones included within this Curb Area at the requested time.	|

[Top][toc]

### Curb Space

Defines individual demarcated spaces within a Curb Zone. Important notes about Curb Spaces:
  - Curb Spaces may NOT overlap with other Curb Spaces
  - Curb Spaces must be wholly contained within a single Curb Zone

A Curb Space is represented as a JSON object whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_space_id` | [UUID][uuid] | Required | The ID of the curb space. |
| `geometry` | [Polygon][polygon] | Required |The spatial extent of this curb location. |
| `curb_zone_id` | [UUID][uuid] | Required | The ID of the Curb Zone this space is within. The geometry of the specified Curb Zone MUST contain the geometry of this space. |
| `space_number` | Integer | Optional | The sequence number of this space within its Zone. If specified, two spaces within the same Curb Zone MUST NOT share a space number, and space numbers SHOULD be consecutive positive integers starting at 1. |
| `length` | Integer | Required | Length in centimeters of this Space. If comparing the length of a vehicle to that of a space, note that vehicles may have to account for a buffer for doors, mirrors, bumpers, ramps, etc. |
| `width` | Integer | Optional | Width in centimeters of this Space. | If comparing the length of a vehicle to that of a space, note that vehicles may have to account for a buffer for doors, mirrors, bumpers, ramps, etc. |
| `available` | Boolean | Optional | Whether this space is available for vehicles to park in at the specified time  (‘True’ means the Space is available). |
| `availability_time` | [Timestamp][ts] | Optional | If availability information is present, the most recent time that availability was computed for this space. |

[Top][toc]

### Policy

A Policy object is a rule that allows or prohibits a particular set of users from using a particular curb at a particular time or times. Multiple Policy objects together define the full extent of regulations within a [Curb Zone](#curb-zone). Much of this is similar to curbLR.

The `policy` field within the FeatureCollection returned by [Query Curb Zones](#query-curb-zones) contains a list of the Policy objects referenced by the returned zones. In addition, the [Query Curb Policies](#query-curb-policies) endpoint return the complete list of policies.

A Policy is represented as a JSON object whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_policy_id` | UUD | Required | An ID that uniquely identifies this exact regulation across Curb Zones. Two Policy objects containing the same `curb_policy_id` MUST be completely identical. A `curb_policy_id` MUST NOT be reused -- once created, it must continue to refer to the identical policy forever. |
| `priority` | Integer | Required | Specifies which other policies this one takes precedence over. If two Policies on the same Curb Zone have overlapping [Time Spans](#time-span) and apply to the same user class, the one that applies at a given time is the one with the **lowest** priority. Two Policies that apply to the same Curb Zone with overlapping Time Spans and user classes MUST NOT have the same priority. |
| `rules` | Array of [Rules](#rule) | Required | The rule(s) that this policy applies. If a Policy specifies multiple rules, each rule MUST specify disjoint lists of user classes. |
| `time_spans` | Array of [Time Spans](#time-span) | Optional | If specified, this regulation only applies at the times defined within. |

#### Rule

A rule defines who is allowed to do what, and for how long, on a curb, per the policy.
It is a JSON object with the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `activity` | String | | Required | The activity that is forbidden or permitted by this regulation. Value MUST be one of the [activities below](#activities)
| `max_stay` | Integer | Optional | The length of time (in minutes) for which the curb may be used under this regulation. |
| `no_return` | Integer | Optional | The length of time (in minutes) that a user must vacate a Curb Zone before allowed to return for another stay. |
| `user_classes` | Array of Strings | If specified, this regulation only applies to users matching the [user classes](#user-classes) contained within. If not specified, this regulation applies to everyone. 
| `rate` | Array of [Rates](#rate) | Optional | The cost of using this Curb Zone when this regulation applies. Rates are repeated to allow for prices that change over time. For instance, a regulation may have a price of $1 for the first hour but $2 for every subsequent hour. |

##### Activities

The following activities may be specified for rules within a policy. The reason we have
"positive" and "negative" versions of the same activities (like `loading` and `no parking`)
is due to priorities: a `loading` rule that is higher priority than a `no loading` rule,
for instance, implies that the Curb Zone does allow loading at the time in question, while a
`no parking` rule would not.

- `parking` (implies that loading and stopping are also permitted)
- `no parking`
- `loading` (of goods; implies that stopping is also permitted)
- `no loading` (implies that parking is also prohibited)
- `stopping`  (stopping briefly to pick up or drop off passengers)
- `no stopping` (stopping, loading, and parking are all prohibited)
- `travel`: represents curbside lanes intended for moving vehicles, like bus lanes, bike lanes,
  and rush-hour-only travel lanes; implies that parking, loading, and stopping are prohibited).

##### User Classes

A user class represents any class of vehicles that is regulated by a city with respect to curb
space. They can be defined by the vehicle's physical characteristics (e.g., trucks, vans, EVs),
the presence/absence of a particular permit (e.g., residential parking permits) or even by the
intent or destination of the driver, for things like hotel or school unloading zones.

New user classes MAY be generated by data providers to reflect their regulations, but when possible,
the following known values should be used:

- `bicycle`
- `bus`
- `handicap`
- `motorcycle`
- `resident`
- `rideshare`
- `taxi`
- `commercial` (applies to "truck"; if "commercial" and "truck" user classes have separate rules,
  trucks use the "truck" rule)
- `truck`

#### Time Span 

A Time Span defines a period of time (that may occur once or repeatedly) during which a given regulation applies. When multiple fields are combined, all criteria must be met in order for a given Time Span to apply. For instance, the following Time Span represents 10AM to 1PM on Mondays and Tuesdays:

```
{
  "days_of_week": ["mon", "tue"],
  "time_of_day_start": "10:00",
  "time_of_day_end": "13:00"
}
```

A Time Span is represented as a JSON object whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `from` | [Timestamp][ts] | Optional |  The earliest point in time that this Time Span could apply. If unspecified, the Time Span applies to all matching periods arbitrarily far in the past. |
| `to` | [Timestamp][ts] | Optional | The latest point in time that this Time Span could apply. If unspecified, the Time Span applies to all matching periods arbitrarily far in the future. |
| `days_of_week` | Array of strings | Optional | An array of days of the week when this Time Span applies, specified as 3-character strings (`"sun"`, `"mon"`, `"tue"`, `"wed"`, `"thu"`, `"fri"`, `"sat"`). |
| `days_of_month` | Array of integers | Optional | An array of days of the month when this Time Span applies, specified as integers (1-31). Note that, in order to specify, e.g., the "2nd Monday of the month", you can use `days_of_month` combined with `days_of_week` (in this example, `days_of_week = ["mon"]` and `days_of_month = [8,9,10,11,12,13,14]`). |
| `months` | Array of integers | Optional | If specified, this Time Span applies only during these months (1=January, 12=December). |
| `time_of_day_start` | "HH:MM" string | Optional | The 24-hour local time that this Time Span starts to apply. If unspecified, this Time Span starts at midnight. |
| `time_of_day_end` | "HH:MM" string | Optional | The 24-hour local time that this Time Span stops applying. This is not inclusive, so for instance if `time_of_day_end` is `"17:00"`, this Time Span goes up to 5PM but does not include it.  If unspecified, this Time Span ends at midnight. |
| `designated_period` | String | Optional | A string representing an arbitrarily-named, externally-defined period of time. Any values MAY be specified but the following known values SHOULD be used when possible: <ul><li>`snow emergency`</li><li>`holidays`</li><li>`school days`</li><li>`game days`</li></ul> |
| `designated_period_except` | `Boolean` | `Optional` | If specified and `true`, this Time Span applies at all times not matching the named designated period. (e.g., if `designated_period` is `snow emergency` and `designated_period_except` is `true`, this Time Span does not apply on snow days). |

#### Rate

A Rate defines the amount a user of the curb needs to pay when a given rule applies. It is a JSON object with the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `per_hour` | Integer | Required | The rate per hour for this space in cents (or the smallest denomination of local currency). |
| `increment_minutes` | Integer | Optional | If specified, this is the smallest amount of time a user can pay for (e.g., if `increment_minutes` is `15`, a user can pay for 15, 30, 45, etc. minutes). |
| `increment_amount` | Integer | Optional | If specified, the rate for this space is rounded up to the nearest increment of this amount, specified in the same units as `per_hour`. |
| `start_minutes` | Integer | Optional | The amount of time the vehicle must have already been present in the Curb Zone before this rate starts applying. If not specified, this rate starts when the vehicle arrives. |
| `end_minutes` | Integer | Optional | The amount of time after which the rate stops applying. If not specified, this rate ends when the vehicle departs. |

[Top][toc]

### Location Reference

A Location Reference defines a linear reference for a given Curb Zone. A linear reference defines a Curb Zone's position by reference to a linear feature, like a street centerline or edge-of-pavement line. Linear referencing systems can be global, like [SharedStreets linear references](https://sharedstreets.io/) or [OpenLR](http://www.openlr.org/index.html), or local to a particular city. 

A Location Reference is a JSON object with the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `source` | URL | Required | An identifier for the source of the linear reference. This MUST be URL pointing to more information about the underlying map or reference system. Values include (but other can be used): <ul><li>`https://sharedstreets.io`: SharedStreets</li><li>`http://openlr.org`: OpenLR</li><li>`https://coord.com`: Coord</li><li>`https://yourcityname.gov`: custom city LR, direct link if possible</li> |
| `ref_id` | String | Required | The linear feature being referenced (usually a street or curb segment). For OpenLR, this is the Base64-encoded OpenLR line location for the street segment of which this Curb Zone is part, and the start and end offsets below are relative to this segment. |
| `start` | Integer | Required | The distance (in centimeters) from the start of the referenced linear feature to the start of the Curb Zone. |
| `end` | Integer | Required | The distance (in centimeters) from the start of the referenced linear feature to the end of the Curb Zone. end MAY be smaller than start, implying that the direction of the Curb Zone is opposite to the direction of the referenced linear feature. |
| `side` | String | Optional | If the referenced linear feature is a roadway, the side of the roadway on which the Curb Zone may be found, when heading from the start to the end of the feature in its native orientation. Values are `left` and `right`. MUST be absent for features where `entire_roadway` is true. |

[Top][toc]

[toc]: #table-of-contents
[uuid]: #uuid
[ts]: #timestamp
[polygon]: #polygon
