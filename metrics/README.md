# Curb Data Specification: Metrics API

<a href="/metrics/"><img src="https://i.imgur.com/5u6S0H8.png" width="100" align="right" alt="CDS Metrics Icon" border="0"></a>

The Metrics API is a REST API allowing historic metrics calculations based on Event activity that happened at defined Curb places. Defines common calculation methodologies to measure historic dwell time, occupancy, usage and other aggregated statistics. 

**See [other CDS APIs](/README.md#curb-data-specification-apis) on the homepage.**

# Endpoints

There are two different endpoints that are part of the Metrics API:

  - [Session](#session) is information about an activity that occurs near, at, or within a pre-defined curb area. Sessions is a subset of items from the Events API.  Sessions is *optional* within Metrics.
  - [Aggregate](#aggregate) is aggregated counts and methodology of curb events. Aggregates is *optional* within Metrics.

**See [examples](examples.md) for these endpoints.**

# Table of Contents

- [Representative Sample Data](#representative-sample-data)
- [REST Endpoints](#rest-endpoints)
  - [Authorization](#authorization)
  * [Update Frequency](#update-frequency)
  * [Query Session](#query-session)
  * [Query Aggregate](#query-aggregate)
- [Data Objects](#data-objects)
  * [Session](#session)
  * [Aggregate](#aggregate)
    * [Methodology](#methodology)
- [Examples](#examples)

# Representative Sample Data

All data returned by the Metrics API should be viewed as using representative sample data, and not necessarily a 100% accurate picture of what happens at every defined curb space. This is because CDS Events can come from multiple sources (company data feeds, sensors, video analysis, payments, check-ins, enforcement, and/or other city data sources), cities may implement only one or more of these sources, each source returns different types and accuracy of data, and sources may not be easily cross-comparible. It is up to the city consuming Events and producing Metrics to determine accuracy and methodology details within their circumstances, and we welcome feedback, refinement, clarification, and more defined methodology per source type for future CDS releases.

# REST Endpoints

All endpoints return a CSV file that can either be pre-computed or created on demand dynamically.

If returning data from a static CSV file directly (e.g, from a web-based file system, service, or data portal), then adding header information is not required.

If returning data from a dynamic server, they MUST set the `Content-Type` header to `application/vnd.cds+csv;version=1.0` to support
versioning in the future.  Clients SHOULD specify an `Accept` header containing 
`application/vnd.cds+csv;version=1.0`. If the server receives a request that contains an `Accept`
header but does not include this value; it MUST respond with a status of `406 Not Acceptable`.

[Top][toc]

## Authorization

[Authorization](/general-information.md#authorization) is **required** for all the Metrics endpoints, since depending on implementation, use cases, fields required, local laws, and audience it may contain information only city transportation agencies should have access to. 

Future versions of Metrics may contain publicly available endpoints or reports to help enable cross-vendor collaboration, data analysis, and Open Data goals. Authorization on Metrics is a [beta feature](/general-information.md#beta-features) and will be revisited in future releases. In the meantime, agencies wishing to publicly release Metrics data are encouraged to limit releases to static [Aggregate](#aggregate) data that has been reviewed for potential privacy risks. Consult our [Privacy Guidance](/README.md#data-privacy) for more details.



[Top][toc]

## Update Frequency

The agency serving the data may choose how frequently they want to update the data. At least monthly is recommended but it may be longer, or weekly, daily, hourly, or even more frequently.

[Top][toc]

##  Query Session

Endpoint: `/metrics/sessions`  
Method: `GET`  
`data` Payload: a CSV object with the following fields:
  - `session`: an array of [Session](#session) objects

_Optional endpoint; if not implemented, a server should reply with 501 Not Implemented._

### Query Parameters

An agency may choose to make this endpoint static (and return all the available data at once in a single file) or dynamic (and allow the use of any or all of the query parameters below to filter the data). If dynamic, all query parameters are optional.

| Name         | Type      | Required/Optional | Description                                    |
| ------------ | --------- | ----------------- | ---------------------------------------------- |
| `curb_place_type` | Enum | Optional | The type of curb place this aggregate applies to from the Curbs API: `area`, `zone`, `space`. Required with `curb_place_id`. |
| `curb_place_id` | [UUID][uuid] | Optional | The ID of this single curb place. If specified, only return data contained within this area. Required with `curb_place_type`. |
| `min_lat`<br/>`min_lng`<br/>`max_lat`<br/>`max_lng` | Numeric | Optional | Specifies a latitude and longitude bounding box. If one of these parameters is specified, all four MUST be. If specified only return Curb Zones that intersect the supplied bounding box. |
| `lat`<br/>`lng`<br/>`radius` | Numeric | Optional | Specifies a latitude and longitude bounding point and a radius away from that point. If one of these parameters is specified, all three MUST be. Returns only Curb Zones that are within `radius` centimeters of the point identified by `lat`/`lng`. Curb Zones in the response MUST be ordered ascending by distance from the center point. |
| `start_time` | [Timestamp][ts] | Optional | The start of the time period to return data (_inclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). |
| `end_time` | [Timestamp][ts] | Optional | The end of the time period to return data (_exclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). |

[Top][toc]

##  Query Aggregate

Endpoint: `/metrics/aggregates`  
Method: `GET`  
`data` Payload: a CSV object with the following fields:
  - `aggregate`: an array of [Aggregate](#aggregate) objects

_Optional endpoint; if not implemented, a server should reply with 501 Not Implemented._

### Query Parameters

An agency may choose to make this endpoint static (and return all the available data at once in a single file) or dynamic (and allow the use of any or all of the query parameters below to filter the data). If dynamic, all query parameters are optional.

| Name         | Type      | Required/Optional | Description                                    |
| ------------ | --------- | ----------------- | ---------------------------------------------- |
| `curb_place_type` | Enum | Optional | The type of curb place this aggregate applies to from the Curbs API: `area`, `zone`, `space`. Required with `curb_place_id`. |
| `curb_place_id` | [UUID][uuid] | Optional | The ID of this single curb place. If specified, only return data contained within this area. Required with `curb_place_type`. |
| `metric_type` | Enum | Optional | The single metric to return from the [Methodology](#methodology): `total_sessions`, `turnover`, `average_dwell_time`, `occupancy_percent`. |
| `min_lat`<br/>`min_lng`<br/>`max_lat`<br/>`max_lng` | Numeric | Optional | Specifies a latitude and longitude bounding box. If one of these parameters is specified, all four MUST be. If specified only return Curb Zones that intersect the supplied bounding box. |
| `lat`<br/>`lng`<br/>`radius` | Numeric | Optional | Specifies a latitude and longitude bounding point and a radius away from that point. If one of these parameters is specified, all three MUST be. Returns only Curb Zones that are within `radius` centimeters of the point identified by `lat`/`lng`. Curb Zones in the response MUST be ordered ascending by distance from the center point. |
| `start_time` | [Timestamp][ts] | Optional | The start of the time period to return data (_inclusive_, see [Range Boundaries](/general-information.md#range-boundaries)).  |
| `end_time` | [Timestamp][ts] | Optional | The end of the time period to return data (_exclusive_, see [Range Boundaries](/general-information.md#range-boundaries)). |

[Top][toc]

# Data Objects 

## Session

Sessions are a historic subset of curb events, with some rows combined, some columns removed for clarity and privacy, and for only some curb event types. 
Sessions are meant to provide a granular view of parking and area sessions happening around the curb places, so consumers can do their own analysis.

A Session is represented as a CSV object, whose fields are as follows, pulled from the Curb Events endpoint in the Events API:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `session_type` | Enum | Required | The type of session that happened for this event: `parking` or `area` |
| `event_session_id` | [UUID][uuid] | Optional | If known and recorded to tie two Events together, then include the `event_session_id` from the [Curb Event](/events#curb-event). |
| `event_id_start` | [UUID][uuid] | Conditionally Required | The globally unique identifier of the **start/enter** event that occurred. |
| `event_id_end` | [UUID][uuid] | Conditionally Required | The globally unique identifier of the **end/exit** event that occurred. |
| `event_location_start_latitude` | Number | Conditionally Required | The geographic latitude point location where the **start/enter** of the event occurred. |
| `event_location_start_longitude` | Number | Conditionally Required | The geographic longitude point location where the **start/enter** of the event occurred. |
| `event_location_end_latitude` | Number | Conditionally Required | The geographic latitude point location where the **end/exit** of the event occurred. |
| `event_location_end_longitude` | Number | Conditionally Required | The geographic longitude point location where the **end/exit** of the event occurred. |
| `event_time_start` | [Timestamp][ts] | Conditionally Required | Timestamp (date/time) at which the event started with the `park_start` or `enter_area` event types. |
| `event_time_end` | [Timestamp][ts] | Conditionally Required | Timestamp (date/time) at which the event occurred. |
| `curb_zone_id` | [UUID][uuid] | Conditionally Required | Unique ID of the Curb Zone where the event occurred. Required for events that occurred at a known Curb Zone for ALL _event_types_. |
| `curb_area_ids` | [UUID][uuid] | Conditionally Required | Unique IDs of the Curb Area where the event occurred. Since Curb Areas can overlap, an event may happen in more than one. Required for events that occurred in a known Curb Area for these event_types:  _enter_area, exit_area, park_start, park_end_ |
| `curb_space_id` | [UUID][uuid] | Conditionally Required | Unique ID of the Curb Space where the event occurred. Required for events that occurred at a known Curb Space for these event_types: _park_start, park_end, enter_area, exit_area_ |
| `vehicle_length` | Integer | Conditionally Required | Approximate length of the vehicle that performed the event, in centimeters. Required for sources capable of determining vehicle length. |
| `vehicle_type` | [Vehicle Type](/events#vehicle-type) | Conditionally Required | Type of the vehicle that performed the event. Required for sources capable of determining vehicle type. |

### Event Types

The following Event Types are relevant to the Session data, and the other event types are not utilized.

For `session_type` of `parking`:
- **park_start**: a vehicle stopped, parked, or double parked
- **park_end**: a parked vehicle leaving a parked or stopped state and resuming movement

For `session_type` of `area`:
- **enter_area**: vehicle enters the relevant geographic area
- **exit_area**: vehicle exits the relevant geographic area

**Note:** It is preferable to return both start/end or enter/exit pairs of events. However, even if only one of these is present, the available data should be returned with the corresponding missing values of `event_id_X`, `event_location_X`, `event_time_X` returned as _null_.

A "session duration" or "dwell time" can be calculated by calculating the difference between the `event_time_start` and `event_time_end`.

[Top][toc]

## Aggregate

Aggregates are historic pre-computed counts and metrics of Events occurring in curb places, aggregated to the hour. All Aggregates can be calculated from the data included in [Session](#session).

An Aggregate is represented as a CSV object, whose fields are as follows, as calculated from the Metrics [Methodology](#methodology):

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `curb_place_type` | Enum | Required | The type of curb place this aggregate applies to from the Curbs API: `area`, `zone`, `space`. |
| `curb_place_id` | [UUID][uuid] | Required | The ID of this curb place. |
| `metric_type` | Enum | Required | The metric this aggregate applies to from the [Methodology](#methodology): `total_sessions`, `turnover`, `average_dwell_time`, `occupancy_percent`. |
| `date` | date | Required | The date the event occured in ISO 8601 format, local timezone, in "YYYY-MM-DD" format. E.g. "2021-10-31" |
| `hour` | integer | Required | The hour of the day the event occured in ISO 8601 format, local timezone, in "hh" format. E.g. "23" |
| `value` | number | Required | The results of the calculations for this metric from the [Methodology](#methodology). Note that "-1" means that the sensor/source was offline for the majority of the time. E.g. "6", "2.9", "-1", or "0.05" |

### Methodology

Cities are facilitating access to the curb for different users based on the curb access priorities of that particular area. The following metrics will be used in understanding how curb usage aligns with priorities. The metrics may be calculated using the [Session](#session) data and returned here, 

#### Total Sessions

`count[sessions]` for a specific time period  
Name: `total_sessions`

_Use Case_

Cities use this to determine ‘demand’ for curb space and understand how much activity is happening at the curb. A session is a parking event, defined by the `park_start` and `park_end` event types.

#### Turnover
 
`count[sessions]/hour` for a specific time period  
Name: `turnover`

_Use Case_

Used together with Average Dwell Time by cities to understand how long vehicles are parked at the curb. When evaluated alongside a vehicle type breakdown, cities can see if the curb space is being used as intended and design better rules for compliance.

#### Average Dwell Time

`sum[dwell time] / count[sessions]` for a specific time period  
Name: `average_dwell_time`

_Use Case_

Turnover and Average Dwell Time are used together by cities to understand how long vehicles are parked at the curb. When evaluated alongside a vehicle type breakdown, cities can see if the curb space is being used as intended and design better rules for compliance.
 
For example, a city may want to impose a 5 minute time limit for a restaurant pick-up and drop-off zone if they see that a lot of vehicles are in that space for 30 minutes to help prioritize the quick pick-up and drop-offs. 

Another example would be if a city sees that a space has large vehicles with an average dwell time of 90 minutes or more they may want to make sure their time limits and operating hours can accommodate these deliveries so that companies aren’t having to leave mid-delivery to move their vehicle.

#### Occupancy Percent

`sum[dwell time] / total duration` for a specific time period  
Name: `occupancy_percent`

_Use Case_

Occupancy is a metric from parking that cities would like to apply to curbs. With parking there’s an optimal occupancy where most of the parking spaces are in-use but there are enough open spaces for any vehicle to be able to drive up and find a space. Cities are interested to learn what the optimal occupancy would be for curbs. With so much competition at the curb, cities want to make sure that any loading zones they create are being used a lot, but not so much that they’re full and drivers have to double-park instead.

[Top][toc]

# Examples

See a series of [CDS Metrics endpoint examples](examples.md) to use as templates. 

[Top][toc]

[toc]: #table-of-contents
[uuid]: /general-information.md#uuid
[ts]: /general-information.md#timestamp
[polygon]: /general-information.md#polygon
