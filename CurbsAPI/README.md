# Curbs API

The Curbs API is a way for cities to specify areas of interest along the curb along with the rules
for using them: who is allowed to park, load, unload, pick up, drop off, etc. at the curb, for how
long, for what price (if any), at what times, and on what days. Curb zones specified in the Curbs
API can be connected to event and metric data and shared with companies and the public. 

There are three different levels of geography that can be specified in the Curbs API:

  - A [Curb Zone](#curb-zone) is a contiguous region of curb space on a single block face that is
    regulated in a particular way. Defining curb zones is *required* as part of the Curbs API.
  - A [Curb Space](#curb-space) is a defined vehicle parking spot along the curb for use by one
    vehicle at a time. Curb spaces are *optional*.
  - A [Curb Area](#curb-area) is a larger area of interest, such as a neighborhood or corridor, that
    could be used to show proximity, approaches, conflicts, circling, or other activity. Curb areas
    are *optional*.

## Objects 

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
  1. Be assigned a unique ID, in the form of a UUID. This ID SHOULD remain consistent as long as
     the Curb Zone's extent and policies remain substantially the same.
  1. It SHOULD NOT be possible to legally park a single vehicle in two different Curb Zone at the 
     same time (i.e., a given non-demarcated parking area or loading zone should be represented as
     a single curb location), unless this conflicts with the requirements above.

A Curb Zone is represented as a JSON object, whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_zone_id` | UUID | Required | TheID of this Curb Zone. |
| `geometry` | GeoJSON polygon geometry | Required | The spatial extent of this curb zone. |
| `curb_policy_ids` | Array of strings | Required | An array of UUIDs, each one of which is the ID of a [Policy object](#policy). Together, these define the regulations of this Curb Zone. |
| `start_date` | RFC 3339 Timestamp | Required | The earliest time that the data for this curb location is known to be valid. This could be the date on which the data was collected, for instance. This MUST never change for a given id. |
| `end_date` | RFC 3339 Timestamp | Optional | The time at which the data for this curb location ceases to be valid. If not present, the data will be presumed to be valid indefinitely. |
| `location_references` | Array of [Location Reference objects](#location-reference) objects | Optional | One or more linear references for this Curb Zone. |
| `name` | String | Optional | A human-readable name for this Curb Zone that identifies it to end users. |
| `user_zone_id` | String | Optional | An identifier that can be used to refer to this Curb Zone on physical signage as well as within mobile applications, typically for payment purposes. |
| `street_name` | String | Optional | The name of the street that this Curb Zone is on. |
| `cross_street_start_name` | String | Optional | The name of the cross street at or before the start of this Curb Zone (which cross street is at the start and end of the location is defined in the same direction as the linear reference for this curb; if no linear reference is provided, start and end SHOULD be oriented such that start comes before end when moving in the direction of travel for the roadway immediately adjacent to the curb.) |
| `cross_street_end_name` | String | Optional | The name of the cross street at the end of this Curb Zone. |
| `length` | Integer | Optional | The length, in centimeters, of the Curb Zone when projected along the street centerline. Note that this is the definitive length of the curb area, and not the edge length of the geographic polygon. |
| `available_space_lengths`| Array of Integers | Optional | If availability information is present, an array of numbers containing the lengths (in centimeters) of all known non-overlapping available spaces within this Curb Zone. In cases where availability is known less precisely, this data MAY be inferred from a model. |
| `availability_time` | String | Optional | If availability information is present, the RFC 3339 timestamp corresponding to the most recent time that availability was computed for this zone. |
| `width` | Integer | Optional | The width, in centimeters, that the Curb Zone occupies from the curb to the roadway lane. |
| `parking_angle` | String | Optional | The angle in which passenger vehicles in this Curb Zone are meant to park. May take one of the following values: <ul><li>`parallel`</li><li>`perpendicular`</li><li>`angled`</li></ul> |
| `num_spaces` | Integer | Optional | The number of demarcated spaces within this Curb Zone. Demarcated spaces may also be specified using [Curb Spaces](#curb-spaces). |
| `street_side` | String | Optional | The cardinal or subcardinal direction representing the side of the roadway that this curb is on. May be `N`, `NE`, `E`, `SE`, `S`, `SW`, `W`, or `NW`. For cities with "grid directions", the side MAY be based on the grid direction rather than the closest true-north compass direction, but MUST NOT be more than 90 degrees away from the true compass direction. |
| `median`| Boolean | Optional | If "true", this curb location is on the median of a street, rather than its edge. A median is a strip of land separating two roadways within the same street. Note that, for medians, `street_side` is interpreted relative to the roadway that the particular curb is on, so the curb along the median of the southern roadway of a divided street would have `street_side` of `N`. |
| `entire_roadway`| Boolean | Optional | If "true", this curb location takes up the entire width of the roadway (which may be impassible for through traffic when the Curb Zone is being used for parking or loading). This is a common condition for alleyways. If `entire_roadway` is `true`, `street_side` MUST NOT be present. |
| `curb_area_id`| String | Optional | The UUID of the [Curb Area](#curb-area) that this Curb Zone is a part of. If specified, the area identified MUST be retrievable through the Curb API and its geographical area MUST contain that of the Curb Zone. |

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
    callers MAY NOT rely on a Curb Zone with the same ID remaining identical over time. |

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
| `curb_area_id` | UUID | Required | The ID for the curb area. |
| `geometry` | GeoJSON Polygon geometry | Required | The spatial extent of this curb location. |
| `name` | String | Required | The name of this curb area. |
| `curb_zone_ids` | Array of UUIDs | Required | The IDs of all the Curb Zones included within this Curb Area at the requested time.	|

[Top][toc]

### Curb Space

Defines individual demarcated spaces within a Curb Zone. Important notes about Curb Spaces:
  - Curb Spaces may NOT overlap with other Curb Spaces
  - Curb Spaces must be wholly contained within a single Curb Zone

A Curb Space is represented as a JSON object whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_space_id` | UUID | Required | The ID of the curb space. |
| `geometry` | GeoJSON Polygon geometry | Required |The spatial extent of this curb location. |
| `curb_zone_id` | UUID | Required | The ID of the Curb Zone this space is within. The geometry of the specified Curb Zone MUST contain the geometry of this space. |
| `space_number` | Integer | Optional | The sequence number of this space within its Zone. If specified, two spaces within the same Curb Zone MUST NOT share a space number, and space numbers SHOULD be consecutive positive integers starting at 1. |
| `length` | Integer | Required | Length in centimeters of this Space. If comparing the length of a vehicle to that of a space, note that vehicles may have to account for a buffer for doors, mirrors, bumpers, ramps, etc. |
| `width` | Integer | Optional | Width in centimeters of this Space. | If comparing the length of a vehicle to that of a space, note that vehicles may have to account for a buffer for doors, mirrors, bumpers, ramps, etc. |
| `available` | Boolean | Optional | Whether this space is available for vehicles to park in at the specified time  (‘True’ means the Space is available). |
| `availability_time` | RFC 3339 Timestamp | Optional | If availability information is present, the most recent time that availability was computed for this space. |

[Top][toc]

### Policy

### Location Reference

### Time Span 

### Metadata

## Endpoints

The Curbs API has the following endpoints: 

  - `GET /curbs/zone`: Query Curb Zones
  - `GET /curbs/policy`: Query Curb Policies
  - `GET /curbs/area`: Query Curb Areas
  - `GET /curbs/space`: Query Curb Spaces
  - `GET /curbs/zone/<id>`: Fetch a Curb Zone
  - `GET /curbs/policy/<id>`: Fetch a Curb Policy
  - `GET /curbs/area/<id>` Fetch a Curb Area
  - `GET /curbs/space/<id>`: Fetch A Curb Space

 
