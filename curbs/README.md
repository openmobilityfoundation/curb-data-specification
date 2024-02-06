# Curb Data Specification: Curbs API

<a href="/curbs/"><img src="https://i.imgur.com/aBqI6yr.png" width="100" align="right" alt="CDS Curbs Icon" border="0"></a>

The Curbs API is a REST API allowing cities to specify areas of interest along the curb along with
the rules for using them: who is allowed to park, load, unload, pick up, drop off, etc.,
for how long, for what price (if any), at what times, and on which days. Locations defined in the
Curbs API can be connected to event and metrics data, and can be shared with companies and the public, for
purposes such as routing, finding legal parking, loading, and pick-up/drop-off spots, or analyzing
curb utilization over time.

**See [other CDS APIs](/README.md#curb-data-specification-apis) on the homepage.**

# Endpoints

There are four different endpoints that are part of the Curbs API:

  - A [Curb Zone](#curb-zone) is a contiguous region of curb space on a single block face that is
    regulated in a particular way. Defining curb zones is *required* as part of the Curbs API.
  - A [Curb Space](#curb-space) is a defined vehicle parking spot along the curb for use by one
    vehicle at a time. Curb spaces are *optional*.
  - A [Curb Area](#curb-area) is a larger area of interest, such as a neighborhood or corridor, that
    could be used to show proximity, approaches, conflicts, circling, or other activity. Curb areas
    are *optional*.
  - A [Curb Object](#curb-object) is a physical item or asset located adjacent to or within a curb space
    that is grouped under a certain type and contains a unique set of attributes. Curb objects are *optional*.
  - A [Curb Policy](#policy) A Policy object is a rule that allows or prohibits a particular set of 
    users from using a particular curb at a particular time or times.  Curb policies are *optional* 
    but recommended with Curb Zones.

**See [examples](examples.md) for these endpoints.**

![Curb Places](https://i.imgur.com/YQ7P243.jpg)

# Table of Contents

- [REST Endpoints](#rest-endpoints)
  - [Authorization](#authorization)
  - [Query Curb Zones](#query-curb-zones)
  - [Query Curb Areas](#query-curb-areas)
  - [Query Curb Spaces](#query-curb-spaces)
  - [Query Curb Policies](#query-curb-policies)
  - [Fetch a Curb Zone](#fetch-a-curb-zone)
  - [Fetch a Curb Area](#fetch-a-curb-area)
  - [Fetch a Curb Space](#fetch-a-curb-space)
  - [Fetch a Curb Policy](#fetch-a-curb-policy)
- [Data Objects](#data-objects)
  - [Curb Zone](#curb-zone)
  - [Curb Area](#curb-area)
  - [Curb Space](#curb-space)
  - [Curb Object](#curb-object)
    - [Object Types](#object-types) 
  - [Policy](#policy)
    - [Rule](#rule)
      - [Activities](#activities)
      - [User Classes](#user-classes)
    - [Time Span](#time-span)
    - [Rate](#rate) 
  - [Location Reference](#location-reference)
- [Examples](#examples)

# REST Endpoints

All endpoints return a JSON object containing the fields as specified in the [REST Endpoint](/general-information.md#rest-endpoints) details.

[Top][toc]

## Authorization

[Authorization](/general-information.md#authorization) is not required for any of the Curbs endpoints, as this information should be made public and easily accessible.

[Top][toc]

##  Query Curb Zones

Endpoint: `/curbs/zones`  
Method: `GET`  
`data` Payload: a JSON object with a `zones` field containing an array of [Curb Zone](#curb-zone) objects.

_This required endpoint must be implemented by every Curbs API server. If attaching policies to curb zones, the [Query Curb Policies](#query-curb-policies) endpoint is also required._

### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `area`       | [UUID][uuid]      | The ID of a [Curb Area](#curb-area). If specified, only return Curb Zones contained within this area. |
| `min_lat`<br/>`min_lng`<br/>`max_lat`<br/>`max_lng` | Numeric | Specifies a bounding box; if one of these parameters is specified, all four MUST be. If specified only return Curb Zones that intersect the supplied bounding box. |
| `lat`<br/>`lng`<br/>`radius` | Numeric | If one of these parameters is specified, all three MUST be. Returns only Curb Zones that are within `radius` centimeters of the point identified by `lat`/`lng`. Curb Zones in the response MUST be ordered ascending by distance from the center point. |
| `include_geometry` | `true`/`false` |  If the value is `false`, do not include the `geometry` field within the [Curb Zone](#curb-zone) feature object. |
| `time` | [Timestamp][ts] | Only Curb Zone objects whose validity period includes this time will be returned; availability data (if supplied) will be returned as of this time. |

[Top][toc]

##  Query Curb Areas

Endpoint: `/curbs/areas`  
Method: `GET`  
`data` Payload: a JSON object with an `areas` field containing an array of [Curb Area](#curb-area) objects.

_Optional endpoint. If not implemented, the server should reply with `501 Not Implemented`._

### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `min_lat`<br/>`min_lng`<br/>`max_lat`<br/>`max_lng` | Numeric | Specifies a bounding box; if one of these parameters is specified, all four MUST be. If specified only return Curb Areas that intersect the supplied bounding box. |
| `lat`<br/>`lng`<br/>`radius` | Numeric | If one of these parameters is specified, all three MUST be. Returns only Curb Areas that are within `radius` centimeters of the point identified by `lat`/`lng`. Curb Areas in the response MUST be ordered ascending by distance from the center point. |

[Top][toc]

## Query Curb Spaces

Endpoint: `/curbs/spaces`  
Method: `GET`  
`data` Payload: a JSON object with a `spaces` field containing an array of [Curb Space](#curb-space) objects.

_Optional endpoint. If not implemented, the server should reply with `501 Not Implemented`._

### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `zone`       | [UUID][uuid]      | The ID of a [Curb Zone](#curb-zone). If specified, only return Curb Spaces contained within this zone. |
| `min_lat`<br/>`min_lng`<br/>`max_lat`<br/>`max_lng` | Numeric | Specifies a bounding box; if one of these parameters is specified, all four MUST be. If specified only return Curb Zones that intersect the supplied bounding box. |
| `lat`<br/>`lng`<br/>`radius` | Numeric | If one of these parameters is specified, all three MUST be. Returns only Curb Spaces that are within `radius` centimeters of the point identified by `lat`/`lng`. Curb Spaces in the response MUST be ordered ascending by distance from the center point. |
| `time` | [Timestamp][ts] | Availability data (if supplied) will be returned as of this time. |

[Top][toc]

## Query Curb Policies

Endpoint: `/curbs/policies`  
Method: `GET`  
`data` Payload: a JSON object with a `policies` field containing an array of [Curb Policy](#policy) objects.

_Optional endpoint, but required if Curb Zones contain policy_id references. If not implemented, the server should reply with `501 Not Implemented`._

### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `ids`        | Comma-separated list of [UUIDs][uuid] | If present, return only policies with the supplied UUIDs. Otherwise, return all policies. |

[Top][toc]

##  Fetch a Curb Zone

Endpoint: `/curbs/zones/<id>`  
Method: `GET`  
`data` Payload: the [Curb Zone](#curb-zone) object with the ID provided in the path.

_Optional endpoint. If not implemented, the server should reply with `501 Not Implemented`._

### Query Parameters

All query parameters are optional.

| Name            | Type            | Description                                    |
| --------------- | --------------- | ---------------------------------------------- |
| `time`          | [Timestamp][ts] | The Curb Zone object will only be returned if its validity period includes this time; otherwise, the server should reply with `404 Not Found`. Availability data (if supplied) will be returned as of this time. |
| `show_historic` | Boolean         | Whether to return historic, retired curb zone data. Default is "false" to reduce payload size and complexity. |

[Top][toc]

##  Fetch a Curb Area

Endpoint: `/curbs/areas/<id>`  
Method: `GET`  
`data` Payload: the [Curb Area](#curb-area) object with the ID provided in the path.

_Optional endpoint. If not implemented, the server should reply with `501 Not Implemented`._

### Query Parameters

This endpoint takes no query parameters.

[Top][toc]

## Fetch a Curb Space

Endpoint: `/curbs/spaces/<id>`  
Method: `GET`  
`data` Payload: the [Curb Space](#curb-space) object with the ID provided in the path.

_Optional endpoint. If not implemented, the server should reply with `501 Not Implemented`._

### Query Parameters

All query parameters are optional.

| Name         | Type            | Description                                    |
| ------------ | --------------- | ---------------------------------------------- |
| `time`       | [Timestamp][ts] | Availability data (if supplied) will be returned as of this time. |

[Top][toc]

## Fetch a Curb Policy

Endpoint: `/curbs/policies/<id>`  
Method: `GET`  
`data` Payload: the [Curb Policy](#policy) object with the ID provided in the path. 

### Query Parameters

This endpoint takes no query parameters.

[Top][toc]

# Data Objects 

## Curb Zone

A Curb Zone is a geographical entity representing a single region along the curb, along with
metadata about that region and the policies that apply to its use by vehicles. What constitutes
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
     the Curb Zone's geography remains substantially the same. Policies may be updated without changing
     the ID.
  1. It SHOULD NOT be possible to legally park a single vehicle in two different Curb Zone at the 
     same time (i.e., a given non-demarcated parking area or loading zone should be represented as
     a single curb location), unless this conflicts with the requirements above.

A Curb Zone is represented as a JSON object, whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_zone_id` | [UUID][uuid] | Required | The ID of this Curb Zone. |
| `geometry` | [Polygon][polygon] | Required | The spatial extent of this curb zone. A new `curb_zone_id` is required if this geometry changes. |
| `curb_policy_ids` | Array of [UUIDs][uuid] | Required | An array of IDs of [Policy objects](#policy). Together, these define the regulations of this Curb Zone. |
| `prev_policies` | Array of [Previous Policy](#previous-policy) objects | Optional | An array of information about previous policies that have applied to this curb zone. They are listed in order with the most recent ones first. |
| `published_date` | [Timestamp][ts] | Required | The date/time that this curb zone was first published in this data feed. |
| `last_updated_date` | [Timestamp][ts] | Required | The date/time that the properties of ths curb zone were last updated. This helps consumers know that some curb objects fields may have changed. |
| `prev_curb_zone_ids` | Array of [UUIDs][uuid] | Optional | An array of IDs of previous curb zone objects. They are listed in order with the most recent ones first. |
| `start_date` | [Timestamp][ts] | Required | The earliest time that the data for this curb location is known to be valid (_inclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). This could be the date on which the data was collected, for instance. This MUST never change for a given id. |
| `end_date` | [Timestamp][ts] | Optional | The time at which the data for this curb location ceases to be valid (_exclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). If not present, the data will be presumed to be valid indefinitely. |
| `location_references` | Array of [Location Reference](#location-reference) objects | Optional | One or more linear references for this Curb Zone. |
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
| `curb_area_ids`| Array of [UUID][uuid] | Optional | The ID(s) of the [Curb Areas](#curb-area) that this Curb Zone is a part of. If specified, the areas identified MUST be retrievable through the Curb API and its geographical area MUST contain that of the Curb Zone. |
| `curb_space_ids`| Array of [UUID][uuid] | Optional | The ID(s) of the [Curb Spaces](#curb-space) that this Curb Zone contains. If specified, the spaces identified MUST be retrievable through the Curb API and its geographical area MUST be contained in this Curb Zone. |
| `curb_object_ids` | Array of [UUID][uuid] | Optional | The ID(s) of the [Curb Objects](#curb-object) that this Curb Zone is related to. If specified, the objects identified MUST be retrievable through the Curb API. Curb Objects can be related to a Curb Space or a Curb Zone. |

[Top][toc]

### Curb Zone ID Stability

The `curb_zone_id` field uniquely identifies a particular zone with one particular set of policies in a specific geographic area.

- *New ID*: When a Curb Zone ceases to be valid, or it needs substantive geometric changes, it will be retired and returned only when the `show_historic` flag is "true" in the [Query Curb Zones](#query-curb-zones) endpoint parameters. Its ID _MUST NOT_ not be reused by a Curb Zone with different data. If geometric data updates reflect changes in the physical world, a new ID MUST be assigned and the previous one stored for historic lookups. 
- *No new ID*: data updates that reflect changes in how the same physical locations or regulations are modeled digitally -- e.g., additional metadata, or regulations being modeled more accurately -- SHOULD NOT be implemented by ending a Curb Zone's validity and creating a new one with a new ID. The existing Curb Zone can be updated silently with the new data and its `last_updated_date` changed. Callers MAY NOT rely on a Curb Zone with the same ID remaining identical over time. If policies in a zone change, you may update the policy IDs, track previous policies, and reset the `last_updated_date`, but keep the zone ID the same.

[Top][toc]

## Curb Area

Defines curb areas in a city and their properties. A Curb Area is a particular neighborhood or area
of interest that includes one or more [Curb Zones](#curb-zone). Important notes about Curb Areas:

  - Curb Areas MAY overlap with other Curb Areas
  - Multiple Curb Areas MAY include the same Curb Zones
  - Curb Areas are not meant to be city-wide, and instead should be an area of interest around one
    or more Curb Zones that is no bigger than a neighborhood.
  - Unlike Zones, Areas may be updated as needed, with a new `curb_area_id` being optionally assigned by the city

A Curb Area is represented as a JSON object, whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_area_id` | [UUID][uuid] | Required | The ID for the curb area. |
| `geometry` | [Polygon][polygon] | Required | The spatial extent of this curb location. |
| `name` | String | Optional | The name of this curb area for reference. |
| `published_date` | [Timestamp][ts] | Required | The date/time that this curb area was first published in this data feed. |
| `last_updated_date` | [Timestamp][ts] | Required | The date/time that the properties of ths curb area were last updated. This helps consumers know that some fields may have changed. |
| `curb_zone_ids` | Array of [UUIDs][uuid] | Required | The IDs of all the Curb Zones included within this Curb Area at the requested time.	|

[Top][toc]

## Curb Space

Defines individual demarcated spaces within a Curb Zone. Important notes about Curb Spaces:

  - Curb Spaces may NOT overlap with other Curb Spaces
  - Curb Spaces must be wholly contained within a single Curb Zone
  - Unlike Zones, Spaces may be updated as needed, with a new `curb_space_id` being optionally assigned by the city

A Curb Space is represented as a JSON object whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_space_id` | [UUID][uuid] | Required | The ID of the curb space. |
| `geometry` | [Polygon][polygon] | Required |The spatial extent of this curb location. |
| `name` | String | Optional | The name of this curb space for reference. |
| `published_date` | [Timestamp][ts] | Required | The date/time that this curb area was first published in this data feed. |
| `last_updated_date` | [Timestamp][ts] | Required | The date/time that the properties of ths curb area were last updated. This helps consumers know that some fields may have changed. |
| `curb_zone_id` | [UUID][uuid] | Required | The ID of the Curb Zone this space is within. The geometry of the specified Curb Zone MUST contain the geometry of this space. |
| `curb_object_ids` | Array of [UUID][uuid] | Conditionally Required | The ID(s) of the [Curb Objects](#curb-object) that this Curb Space is related to. If specified, the objects identified MUST be retrievable through the Curb API. Curb Objects can be related to a Curb Space or a Curb Zone.|
| `space_number` | Integer | Optional | The sequence number of this space within its Zone. If specified, two spaces within the same Curb Zone MUST NOT share a space number, and space numbers SHOULD be consecutive positive integers starting at 1. |
| `length` | Integer | Required | Length in centimeters of this Space. If comparing the length of a vehicle to that of a space, note that vehicles may have to account for a buffer for doors, mirrors, bumpers, ramps, etc. |
| `width` | Integer | Optional | Width in centimeters of this Space. | If comparing the length of a vehicle to that of a space, note that vehicles may have to account for a buffer for doors, mirrors, bumpers, ramps, etc. |
| `available` | Boolean | Optional | Whether this space is available for vehicles to park in at the specified time  (‘True’ means the Space is available). |
| `availability_time` | [Timestamp][ts] | Optional | If availability information is present, the most recent time that availability was computed for this space. |

[Top][toc]

## Curb Object

Defines individual assets located adjacent to a Curb Space or Curb Zone. Important notes about Curb Objects:

  - Curb Objects can be located anywhere: within, beside, or overlapping with Curb Zones, Spaces, or other Curb Objects
  - Curb Objects must be related to either a Curb Space or Curb Zone
  - ...

A Curb Object is represented as a JSON object whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_object_id` | [UUID][uuid] | Required | The ID of the curb object. |
| `geometry` | [Point][point] | Required |The spatial location of this curb object location. This can represent the approximate center of the object, or the centroid location of the object, depending on its size and shape. |
| `name` | String | Optional | A short name of this curb object for reference. |
| `description` | String | Optional | A more detailed description of the object if needed. |
| `owner` | String | Optional | The name of the agency, department, etc responsibile for maintaining this object. |
| `object_type` | [Object Types](#object-types) String | Required | The categrory of the curb object. Value is one of the [Object Types](#object-types). |
| `linear_distance` | Integer | Optional | Parallel distance from the side of the object to the linear referencing start point of the curb, in centimeters. |
| `perpendicular_distance` | Integer | Optional | Perpendicular distance from the front of the object to the curb edge start/end, in centimeters. This distance can be negative or positive, with the positive direction being from the curb towards the sidewalk. |
| `max_length` | Integer | Optional | Maximum, bounding box length of the object parallel to the curb, in centimeters. |
| `max_depth` | Integer | Optional | Maximum, bounding box depth of the object perpendicular to the curb, in centimeters. |
| `max_height` | Integer | Optional | Maximum, bounding box height of the object from the sidewalk/street surface, in centimeters. |
| `published_date` | [Timestamp][ts] | Required | The date/time that this curb object was first published in this data feed. |
| `last_updated_date` | [Timestamp][ts] | Required | The date/time that the properties of ths curb object were last updated. This helps consumers know that some fields may have changed. |
| `curb_zone_id` | [UUID][uuid] | Conditionally Required | The ID of the Curb Zone this object is related to. The geometry of the specified Curb Zone does not need to directly relate to the geometry of this object. |
| `curb_space_id` | [UUID][uuid] | Conditionally Required | The ID of the Curb Space this object is related to. The geometry of the specified Curb ZSpace does not need to directly relate to the geometry of this object. |
| `external_object_id` | String | Optional | A unique identifier for this object from an external source, like `external_object_url`. |
| `external_object_url` | URL | Optional | A URL link to a data feed that contains the `external_object_id`. |

[Top][toc]

### Object Types

The following object types may be specified for Curb Objects. This list is meant to be exhaustive an made to represent all types of objects at the curb. Descriptions have been provided with each object type. New objects MAY be generated to reflect local curb uses, but when possible, the following well-known recommended values should be used. If multiple similar values apply, then use the more descriptive/specific value when possible.

**Well-known values:**
Mobility Related
- `signage` -
- `bus_stop` -
- `bike_rack` -
- `scooter_parking` -
- `ev_charging` -
- `ramp` -
- `meter` -
- `pay_station` - 

Curbside Infrastructure 
- `lighting` -
- `signal_cabinet` -
- `utility_box` -
- `fire_hydrant` -
- `surveillance_camera` -

Curbside Furniture
- `barrier` -
- `bollard` -
- `street_trees` -
- `planter` - 
- `drinking_fountain` -
- `toilet` -
- `bench` -
- `sculpture` -
- `art` -
- `fountain` -
- `solid_waste_bins` -
- `post_box` -
- `locker` -

Other
- `food_vendor` -
- ...

[Top][toc]

### Type Specific Attributes

Some object types will have unique attributes that can be optionally included with the given object type. A list of these attributes are outlined below for some of these object types. 

#### Signage

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `purpose` | List - String | Optional | The reason the sign is there or what type of information it is providing. See the list of purposes in XXX. |
| `sign_text` | String | Optional |All relevant text located on the sign. |
| `owner` | String | Optional | The entity that owns and maintains the sign. |

#### Object Type X

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |

[Top][toc]

## Policy

A Policy object is a rule that allows or prohibits a particular set of users from using a particular curb at a particular time or times. Multiple Policy objects together define the full extent of regulations within a [Curb Zone](#curb-zone). The design of the Policy object borrows heavily from the work of the [CurbLR](https://github.com/curblr/curblr-spec) project, with additions for the larger scope of CDS.

The `policy` field within the FeatureCollection returned by [Query Curb Zones](#query-curb-zones) contains a list of the Policy objects referenced by the returned zones. In addition, the [Query Curb Policies](#query-curb-policies) endpoint return the complete list of policies.

A Policy is represented as a JSON object whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_policy_id` | UUID | Required | An ID that uniquely identifies this exact regulation across Curb Zones. Two Policy objects containing the same `curb_policy_id` MUST be completely identical. A `curb_policy_id` MUST NOT be reused -- once created, it must continue to refer to the identical policy forever. |
| `published_date` | [Timestamp][ts] | Required | The date/time that this policy was first published in this data feed. |
| `priority` | Integer | Required | Specifies which other policies this one takes precedence over. If two Policies on the same Curb Zone have overlapping [Time Spans](#time-span) and apply to the same user class, the one that applies at a given time is the one with the **lowest** priority. E.g., a priority of `1` takes precedence over a priority of `3`. Two Policies that apply to the same Curb Zone with overlapping Time Spans and equivalent User Class enumerations MUST NOT have the same priority. |
| `rules` | Array of [Rules](#rule) | Required | The rule(s) that this policy applies. If a Policy specifies multiple rules, each rule MUST specify disjoint lists of user classes. |
| `time_spans` | Array of [Time Spans](#time-span) | Optional | If specified, this regulation only applies at the times defined within. |
| `data_source_operator_id` | Array of [UUIDs][uuid] | Optional | An array of Data Source Operator IDs that this policy only applies to. IDs come from [data_source_operators.csv](/data_source_operators.csv) file here in the CDS repo. Read our [How to Get a Data Source Operator ID](https://github.com/openmobilityfoundation/curb-data-specification/wiki/Adding-a-CDS-Data-Source-Operator-ID) guide. |

[Top][toc]

### Rule

A rule defines who is allowed to do what, and for how long, on a curb, per the policy.

It is a JSON object with the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `activity` | [Activity](#activities) String | Required | The activity that is forbidden or permitted by this regulation. Value MUST be one of the [activities](#activities). |
| `max_stay` | Integer | Optional | The length of time (in units of `max_stay_unit`) for which the curb may be used under this regulation. If not specified, the curb may be used under this regulation indefinitely. May not be applicable for all [activities](#activities). |
| `max_stay_unit` | Enum | Optional | The [Unit of Time](/general-information.md#unit-of-time-enum) associated with the `max_stay` value. Defaults to "minute". |
| `no_return` | Integer | Optional | The length of time (in units of `no_return_unit`) that a user must vacate a Curb Zone before being allowed to return for another stay. Defaults to 0. May not be applicable for all [activities](#activities). |
| `no_return_unit` | Enum | Optional | The [Unit of Time](/general-information.md#unit-of-time-enum) associated with the `no_return` value. Defaults to "minute". |
| `user_classes` | Array of [user class](#user-classes) Strings | Optional | If specified, this regulation only applies to users matching the [user classes](#user-classes) contained within. If not specified, this regulation applies to everyone. The order of `user_classes` is not relevant, but a vehicle using a curb with this rule must match all `user_classes` contained in the array. |
| `rate` | Array of [Rates](#rate) | Optional | The cost of using this Curb Zone when this regulation applies. Rates are repeated to allow for prices that change over time. For instance, a regulation may have a price of $1 for the first hour but $2 for every subsequent hour. The complete set of the [Rates](#rate) array must span **from** `start_minutes` = `0` or `null` **to** `end_minutes` = `max_stay` without overlap of effective minutes (i.e. the range created by rate `start_minutes` and `end_minutes`).  If a "negative" [activity](#activities) is used, this array should be empty. May not be applicable for all [activity](#activities). |

[Top][toc]

#### Activities

The following activities may be specified for rules within a policy. The reason we have
"positive" and "negative" versions of the same activities (like `loading` and `no parking`)
is due to priorities: a `loading` rule that is higher priority than a `no loading` rule,
for instance, implies that the Curb Zone does allow loading at the time in question, while a
`no parking` rule would not. If "negative", `rate` array should be empty.

- `parking` - implies that loading and stopping are also permitted
- `no parking` - may not stop and leave vehicle unattended
- `loading` - loading of goods; implies that stopping is also permitted
- `no loading` - no loading allowed; implies that parking is also prohibited
- `unloading` - unloading of goods; implies that stopping is also permitted
- `no unloading` - no unloading allowed; implies that parking is also prohibited
- `stopping` - stopping briefly to pick up or drop off passengers
- `no stopping` - stopping, loading, unloading, and parking are all prohibited; not a typical travel lane
- `travel` - represents curbside lanes typically intended for moving vehicles, like bus lanes, bike lanes,
  and rush-hour-only travel lanes; implies that parking, loading, unloading, and stopping are prohibited.
- `no travel` - no travel allowed; implies it could be used for other activities

[Top][toc]

#### User Classes

A user class represents any class of vehicles that is regulated by a city with respect to curb
space. They can be defined by the vehicle's physical characteristics (e.g., trucks, vans, EVs),
the presence/absence of a particular permit (e.g., residential parking permits), or even by the
intent or destination of the driver, for things like hotel or school unloading zones.

These are not meant to be a mirror to similarly named items in the Events API, but instead serve a 
unique purpose of describing locally defined regulations at a curb.

This array of user classes serves as an 'AND' function. A vehicle must have all the properties listed
in the array to use the curb. For example, an accessible EV bus will use `handicap-accessible` AND `electric` 
AND `bus`. To create 'OR' values at the same curb, you must create a new rule 
with the new array of values.

New user classes MAY be generated to reflect local regulations, but when possible,
the following well-known recommended values should be used. If multiple similar values apply, then use the more 
descriptive/specific value when possible.

**Well-known values:**

Vehicle types
- `bicycle`
- `bus`
- `cargo_bicycle`
- `car`
- `moped`
- `motorcycle`
- `scooter`
- `truck`
- `van`

Vehicle properties
- `handicap-accessible`
- `human`
- `electric_assist`
- `electric`
- `combustion`
- `autonomous`

Purpose
- `construction`
- `delivery`
- `emergency_use`
- `freight`
- `parking`
- `permit`
- `rideshare`
- `school`
- `service_vehicles`
- `special_events`
- `taxi`
- `utilities`
- `vending`
- `waste_management`

[Top][toc]

### Time Span 

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
| `start_date` | [Timestamp][ts] | Optional | The earliest point in time that this Time Span could apply (_inclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). If unspecified, the Time Span applies to all matching periods arbitrarily far in the past. See note below for more details. |
| `end_date` | [Timestamp][ts] | Optional | The latest point in time that this Time Span could apply (_exclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). If unspecified, the Time Span applies to all matching periods arbitrarily far in the future. See note below for more details. |
| `days_of_week` | Array of strings | Optional | An array of days of the week when this Time Span applies, specified as 3-character strings (`"sun"`, `"mon"`, `"tue"`, `"wed"`, `"thu"`, `"fri"`, `"sat"`). |
| `days_of_month` | Array of integers | Optional | An array of days of the month when this Time Span applies, specified as integers (1-31). Note that, in order to specify, e.g., the "2nd Monday of the month", you can use `days_of_month` combined with `days_of_week` (in this example, `days_of_week = ["mon"]` and `days_of_month = [8,9,10,11,12,13,14]`). |
| `months` | Array of integers | Optional | If specified, this Time Span applies only during these months (1=January, 12=December). |
| `time_of_day_start` | "HH:MM" string | Optional | The 24-hour local time that this Time Span starts to apply (_inclusive_, see [Range Boundaries](/general-information.md#range-boundaries)), in the local timezone. If unspecified, this Time Span starts at midnight. |
| `time_of_day_end` | "HH:MM" string | Optional | The 24-hour local time that this Time Span stops applying (_exclusive_, see [Range Boundaries](/general-information.md#range-boundaries)), in the local timezone. This is not inclusive, so for instance if `time_of_day_end` is `"17:00"`, this Time Span goes up to 5PM but does not include it.  If unspecified, this Time Span ends at midnight. |
| `designated_period` | String | Optional | A string representing an arbitrarily-named, externally-defined period of time. Any values MAY be specified but the following known values SHOULD be used when possible: <ul><li>`snow emergency`</li><li>`holidays`</li><li>`school days`</li><li>`game days`</li></ul> |
| `designated_period_except` | `Boolean` | `Optional` | If specified and `true`, this Time Span applies at all times not matching the named designated period. (e.g., if `designated_period` is `snow emergency` and `designated_period_except` is `true`, this Time Span does not apply on snow days). |

**Note about `start_date` and `end_date` in _Time Span_:** these fields are optional but useful for defining policies that will be used once and won't be reused later, like around a specific, temporary event. If used, they are only applicable in any connected Curb Zone during their overlapping time frames.

[Top][toc]

### Rate

A Rate defines the amount a user of the curb needs to pay when a given rule applies. It is a JSON object with the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `rate` | Integer | Required | The rate for this space in cents (or the smallest denomination of local currency) per `rate_unit`. |
| `rate_unit` | Enum | Required | The unit of time associated with the rate. One of "second", "minute", "hour", "day", "week", "month", "quarter", "year". |
| `rate_unit_period` | Enum | Optional | The period of time that the `rate_unit` covers. One of "rolling" or "calendar". If not specified, **defaults** to "rolling". When **rolling**, the `rate_unit` begins (inclusive) at the timestamp of the event and ends (exclusive) when one full `rate_unit` has elapsed. For example, with `{"rate_unit": "month", "rate_unit_period": "rolling"}` the `rate_unit` for an event starting at `2022-02-25 19:25:52` would be interpreted as ranging from the start of the event - `2022-02-25 19:25:52` (inclusive) - to the timestamp when one "month" has elapsed: `2022-03-25 19:25:52` (exclusive). E.g. from the 25th of one month to the 25th of the next month. When **calendar**, the `rate_unit` begins (inclusive) at the start of the `rate_unit` in question and ends (exclusive) when one full `rate_unit` has elapsed. For example, with `{"rate_unit": "month", "rate_unit_period": "calendar"}` the `rate_unit` for an event starting at `2022-02-25 19:25:52` would be interpreted as ranging from the start of the "month" - `2022-02-01 00:00:00` (inclusive) - to the end of the "month": `2022-03-01 00:00:00` (exclusive). E.g. from the 25th of the month to the end of the current calendar month only. The "week" `rate_unit` is defined as starting on Monday, per the ISO 8601 standard. See [example](/curbs/examples.md#curb-policy-rate-units) for other scenarios in action. | 
| `increment_duration` | Integer | Optional | If specified, this is the smallest number of `rate_unit`s a user can pay for (e.g., if `increment_duration` is `15` and `rate_unit` is `minute`, a user can pay for 15, 30, 45, etc. minutes). |
| `increment_amount` | Integer | Optional | If specified, the rate for this space is rounded up to the nearest increment of this amount, specified in the same currency units as `rate`. |
| `start_duration` | Integer | Optional | The number of `rate_unit`s the vehicle must have already been present in the Curb Zone before this rate starts applying (_inclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). If not specified, this rate starts when the vehicle arrives. |
| `end_duration` | Integer | Optional | The number of `rate_unit`s after which the rate stops applying (_exclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). If not specified, this rate ends when the vehicle departs. |

[Top][toc]

## Location Reference

A Location Reference defines a linear reference for a given Curb Zone. A linear reference defines a Curb Zone's position by reference to a linear feature, like a street centerline or edge-of-pavement line. Linear referencing systems can be global, like [SharedStreets linear references](https://sharedstreets.io/) or [OpenLR](http://www.openlr.org/index.html), or local to a particular city. 

A Location Reference is a JSON object with the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `source` | URL | Required | An identifier for the source of the linear reference. This MUST be a URL pointing to more information about the underlying map or reference system. Values include (but other can be used): <ul><li>`https://sharedstreets.io`: SharedStreets</li><li>`http://openlr.org`: OpenLR</li><li>`https://yourcityname.gov/LR`: custom city LR, direct link if possible</li></ul> |
| `ref_id` | String | Required | The linear feature being referenced (usually a street or curb segment). For OpenLR, this is the Base64-encoded OpenLR line location for the street segment of which this Curb Zone is part, and the start and end offsets below are relative to this segment. |
| `start` | Integer | Required | The distance (in centimeters) from the start of the referenced linear feature to the start of the Curb Zone (_inclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). |
| `end` | Integer | Required | The distance (in centimeters) from the start of the referenced linear feature to the end of the Curb Zone (_exclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). 'end' MAY be smaller than start, implying that the direction of the Curb Zone is opposite to the direction of the referenced linear feature - in this case the [Range Boundaries](/general-information.md#range-boundaries)) are reversed. |
| `side` | String | Optional | If the referenced linear feature is a roadway, the side of the roadway on which the Curb Zone may be found, when heading from the start to the end of the feature in its native orientation. Values are `left` and `right`. MUST be absent for features where `entire_roadway` is true. |

[Top][toc]
  
## Previous Policy

An array of information about what previous policies applied to a [curb zone](#curb-zone) and when. This allows cities to historically track what policies applied to a curb zone.

A Previous Policy is a JSON object with the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_policy_ids` | Array of [UUIDs][uuid] | Required | An array of IDs of [Policy objects](#policy). Together, these define the previous regulations of this Curb Zone. |
| `start_date` | [Timestamp][ts] | Required | The date/time that this policy started being active for this curb location (_inclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). |
| `end_date` | [Timestamp][ts] | Required | The date/time that this policy ended being active for this curb location (_exclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). |

[Top][toc]
 
# Examples

See a series of [CDS Curbs endpoint examples](examples.md) to use as templates. 

[Top][toc]

[toc]: #table-of-contents
[uuid]: /general-information.md#uuid
[ts]: /general-information.md#timestamp
[polygon]: /general-information.md#polygon
[point]: /general-information.md#point
