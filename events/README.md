# Curb Data Specification: Events API

The Events API is a REST API allowing real-time and historic events at the curb to be sent to cities, and the ability to check on the status of any sensors. Events can come from company data feeds, sensors, payments, check-ins, enforcement, and/or other city data sources. Data sent in the Events API can be connected to the Curbs API over time and space, and events are used for calcuations in the Metrics API. 

## ⚠ Beta
> **This feature is current a draft of the initial CDS beta release. It will change as development and real-world feedback happens.**

# Endpoints

There are two different endpoints that are part of the Events API:

  - A [Curb Event](#curb-event) is an activity that occurs near, at, or within a pre-defined curb area. Defining events is *required* as part of the Events API.
  - A [Status](#status) is the current status of a curb monitoring source. Event status is *optional*.

# Table of Contents

- [REST Endpoints](#rest-endpoints)
  * [Query Event](#query-event)
  * [Query Status](#query-status)
- [Data Objects](#data-objects)
  * [Curb Event](#curb-event)
    * [Event Type](#event-type)
    * [Vehicle Type](#vehicle-type)
    * [Propulsion Type](#propulsion-type)
    * [Activity Type](#activity-type)
    * [Lane Type](#lane-type)
    * [Curb Occupant](#curb-occupant)
  * [Status](#status)

# REST Endpoints

All endpoints return a JSON object containing the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `data` | _Endpoint-dependent_ | Required | The requested data objects. |
| `version` | String | Required | The specification version that the API conforms to (currently, `0.0`) |
| `time_zone` | String | Required | The time zone that applies to parking regulations in this dataset. MUST be a valid [TZ database](https://www.iana.org/time-zones) time zone name (e.g. `"US/Eastern"` or `"Europe/Paris"`). |
| `last_updated` | [Timestamp][ts] | Required | The last time the data in this API was updated. |
| `author` | String | Optional | The name of the organization that produces and maintains this data. |
| `license_url` | URL | Optional | The licensing terms under which this data is provided. |

Servers MUST set the `Content-Type` header to `application/vnd.cds+json;version=0.0` to support
versioning in the future.  Clients SHOULD specify an `Accept` header containing 
`application/vnd.cds+json;version=0.0`. If the server receives a request that contains an `Accept`
header but does not include this value; it MUST respond with a status of `406 Not Acceptable`.

[Top][toc]

##  Query Event

Endpoint: `/events/event`  
Method: `GET`  
`data` Payload: a JSON object with the following fields:
  - `events`: an array of [Curb Event](#curb-event) objects

_This endpoint must be implemented by every Events API server._

### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `curb_area_id`  | [UUID][uuid] | The ID of a [Curb Area](#curb-area). If specified, only return events occuring within this area. |
| `curb_zone_id`  | [UUID][uuid] | The ID of a [Curb Zone](#curb-zone). If specified, only return events occuring within this zone. |
| `curb_space_id` | [UUID][uuid] | The ID of a [Curb Space](#curb-space). If specified, only return events occuring within this space. |

[Top][toc]

##  Query Status

Endpoint: `/events/status`  
Method: `GET`  
`data` Payload: a JSON object with a `status` field containing an array of [Status](#status) objects.

_Optional endpoint; if not implemented, the server should reply with `501 Not Implemented`._

### Query Parameters

All query parameters are optional.

| Name         | Type      | Description                                    |
| ------------ | --------- | ---------------------------------------------- |
| `curb_area_id`  | [UUID][uuid] | The ID of a [Curb Area](#curb-area). If specified, only return sensor statuses within this area. |
| `curb_zone_id`  | [UUID][uuid] | The ID of a [Curb Zone](#curb-zone). If specified, only return sensor statuses within this zone. |
| `curb_space_id` | [UUID][uuid] | The ID of a [Curb Space](#curb-space). If specified, only return sensor statuses within this space. |

[Top][toc]

# Data Objects 

## Curb Event

A Curb Event is a record of activity that happens within the geographic bounds of a Curbs object. 

A Curb Event is represented as a JSON object, whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `event_id` | [UUID][uuid] | Required | The globally unique identifier of the event that occurred. |
| `event_type` | [Event Type](#event-type) | Required | The event_type that happened for this event. |
| `event_source_category` | Enum [Sensor Category](#sensor-category) | Required | General category of the source creating the event. |
| `source_operator_id` | [UUID][uuid] | Required | Unique identifier of the entity responsible for operating the event source. |
| `source_id` | [UUID][uuid] | Required | Unique identifier of this event source, whether sensor, vehicle, camera, etc. Allows agencies to connect related Events as they are recorded. |
| `event_location` | GeoJSON | Required | The geographic point location where the event occurred. |
| `event_time` | [Timestamp][ts] | Required | Time at which the event occurred. |
| `publication_time` | [Timestamp][ts] | Required | Time at which the event became available for consumption by this API. |
| `curb_zone_id` | [UUID][uuid] | Conditionally Required | Unique ID of the Curb Zone where the event occurred. Required for events that occurred at a known Curb Zone for ALL _event_types_. |
| `curb_area_ids` | [UUID][uuid] | Conditionally Required | Unique IDs of the Curb Area where the event occurred. Since Curb Areas can overlap, an event may happen in more than one. Required for events that occurred in a known Curb Area for these event_types:  _enter_area, exit_area, park_start, park_end_ |
| `curb_space_id` | [UUID][uuid] | Conditionally Required | Unique ID of the Curb Space where the event occurred. Required for events that occurred at a known Curb Space for these event_types: _park_start, park_end, enter_area, exit_area_ |
| `provider_id` | [UUID][uuid] | Optional | Unique ID of the provider responsible for operating the vehicle at the time of the event, if any. IDs are global and come from the [providers.csv](/providers.csv) file here in the CDS repo. |
| `provider_name` | String | Optional | Name of the provider responsible for operating the vehicle, device, or sensor at the time of the event, if any. |
| `sensor_id` | [UUID][uuid] | Optional |  If a sensor was used, the globally unique identifier of the sensor that recorded the event. |
| `sensor_status` | Object | Optional | The status of the sensor that reported the event at the time that the event was reported. _is_commissioned_: Boolean, required. Indicates whether the sensor is currently in a state where it should be reporting data. _is_online_: Boolean, required. Indicates whether the sensor is currently online and reporting data. |
| `vehicle_id` | String | Optional | A vehicle identifier visible on the vehicle itself. |
| `vehicle_length` | Integer | Conditionally Required | Approximate length of the vehicle that performed the event, in centimeters. Required for sources capable of determining vehicle length. |
| `vehicle_type` | [Vehicle Type](#vehicle-type) | Conditionally Required | Type of the vehicle that performed the event. Required for sources capable of determining vehicle type. |
| `propulsion_types` | Array of [Propulsion Type](#propulsion-type) | Conditionally Required | List of propulsion types used by the vehicle that performed the event. Required for sources capable of determining vehicle propulsion type. |
| `activity_type` | [Activity Type](#activity-type) | Conditionally Required | Type of activity that the vehicle performed. Required for sources capable of determining activity type for relevant event_types. |
| `blocked_lane_types` | Array of [Lane Type](#lane-type) | Conditionally Required | Type(s) of lane blocked by the vehicle performing the event. If no lanes are blocked by the vehicle performing the event, the array should be empty.  Required for the following event_types: _park_start_ |
| `curb_occupants` | Array of [Curb Occupant](#curb-occupants) | Conditionally Required | Current occupants of the Curb Zone. If the sensor is capable of identifying the linear location of the vehicle, then elements are sorted in ascending order according to the start property of the linear reference. Otherwise, elements appear in no particular order. Required for the following event_types: _park_start, park_end, scheduled_report_ |
| `currency` | String | Optional | Fields specifying a monetary cost use a currency as specified in ISO 4217. All costs should be given as integers in the currency's smallest unit. As an example, to represent $1 USD, specify an amount of 100 (for 100 cents). If the currency field is null, USD cents is implied. |
| `actual_cost` | Integer | Optional | If available from the source, the actual cost, in the currency defined in currency, paid by the curb user for this event. |

[Top][toc]

### Event Type

`event_type`. Curb Event Type enumerates the set of possible types of Curb Event. The values that it can assume are listed below:

| `event_type`   | Description |
|----------------| ----------- |
| `comms_lost`   | communications with the event source were lost |
| `comms_restored` | communications with the event source were restored |
| `decommissioned` | event source was decommissioned |
| `park_start`   | a vehicle stopped, parked, or double parked |
| `park_end`     | a parked vehicle leaving a parked or stopped state and resuming movement |
| `scheduled_report` | event source reported status status at a scheduled interval |
| `enter_area`   | vehicle enters the relevant geographic area |
| `exit_area`    | vehicle exits the relevant geographic area |

[Top][toc]

### Vehicle Type

Type of vehicle, similar to vehicle_type in MDS. For this CDS release the list will be developed independently here to accommodate CDS and MDS use cases, while still aligning to the MDS design principles.  In the next major MDS 2.0 release and next CDS release, alignment between CDS and MDS vehicle types can occur.

| `vehicle_type`   | Description |
|------------------| ----------- |
| `bicycle`        | A two-wheeled mobility device intended for personal transportation that can be operated via pedals, with or without a motorized assist (includes e-bikes, recumbents, and tandems) |
| `cargo_bicycle`  | A two- or three-wheeled bicycle intended for transporting larger, heavier cargo than a standard bicycle (such as goods or passengers), with or without motorized assist (includes bakfiets/front-loaders, cargo trikes, and long-tails) |
| `car`            | A passenger car or similar light-duty vehicle |
| `scooter`        | A standing or seated fully-motorized mobility device intended for one rider, capable of travel at low or moderate speeds, and suited for operation in infrastructure shared with motorized bicycles |
| `moped`          | A seated fully-motorized mobility device capable of travel at moderate or high speeds and suited for operation in general urban traffic |
| `motorcycle`     | A seated mobility device capable of travel at high speeds and suited for operation in general urban traffic or expressways |
| `truck`          | A light or heavy duty 4 wheeled truck |
| `van`            | A van with significant interior cargo space |
| `freight`        | A large delivery truck with attached cab |
| `other`          | A device that does not fit in the other categories |
| `unspecified`    | Unspecified |

[Top][toc]

### Propulsion Type

Propulsion type of the vehicle, similar to propulsion_type in MDS. For this CDS release the list will be developed independently here to accommodate CDS and MDS use cases, while still aligning to the MDS design principles.  In the next major MDS 2.0 release and next CDS release, alignment between CDS and MDS propulsion types can occur. 

| `propulsion`      | Description                                            |
| ----------------- | ------------------------------------------------------ |
| `human`           | Pedal or foot propulsion                               |
| `electric_assist` | Provides power only alongside human propulsion         |
| `electric`        | Contains throttle mode with a battery-powered motor    |
| `combustion`      | Contains throttle mode with a gas engine-powered motor |

A vehicle may have one or more values from the `propulsion`, depending on the number of modes of operation. For example, a scooter that can be powered by foot or by electric motor would have the `propulsion` represented by the array `['human', 'electric']`. A bicycle with pedal-assist would have the `propulsion` represented by the array `['human', 'electric_assist']` if it can also be operated as a traditional bicycle.

[Top][toc]

### Activity Type

Type of activity that the vehicle performed.

| `activity_type`   | Description                                            |
| ----------------- | ------------------------------------------------------ |
| `passenger_transport` | Picking up and/or dropping off of human passengers |
| `food_delivery`   | Delivery of food items ready for consumption to an end consumer |
| `parcel_delivery` | Delivery of parcels, including bulk food goods to a restaurant or other business |
| `construction`    | Construction of hard assets including buildings and roadside infrastructure |
| `waste_management` | Retrieval/disposal of waste |
| `unspecified`     | Unknown or unspecified activity type |

[Top][toc]

### Lane Type

`lane_type`. Type(s) of lane used or blocked by the vehicle performing the event. 

| `lane_type`    | Description                                            |
| -------------- | ------------------------------------------------------ |
| `traffic_lane` | A standard vehicle traffic lane |
| `turn_lane`    | A dedicated turn lane |
| `bike_lane`    | A lane dedicated for usage by cyclists |
| `bus_lane`     | A lane dedicated for usage by busses |
| `unspecified`  | Unspecified |

[Top][toc]

### Curb Occupants

`curb_occupants`. A Curb Occupant object represents a specific vehicle’s occupancy in a curb region at a specific point in time. Curb Occupant objects contain the following fields:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `type` | [Vehicle Type](#vehicle-type) | Required | The vehicle type of the occupant. When the event source is not capable of distinguishing vehicle type, this property must take the value "unspecified".
| `length` | Float | Conditionally required | The approximate length in centimeters of the vehicle. Required when the event source is capable of determining vehicle length.
| `linear_location` | Array of Float | Conditionally required | A two-element array that specifies the start and end of the occupant’s linear location relative to the start of the Curb Zone in that order. Required when the event source is capable of determining the linear location of occupants.

[Top][toc]

## Status

The Curb Status is the current status of a curb monitoring source. 

A Curb Status is represented as a JSON object array of all deployed sources and/or sensors, whose fields are as follows:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `sensor_id` | [UUID][uuid] | Required |  If a sensor was used, the globally unique identifier of the sensor that records events. |
| `sensor_status` | Object | Required | The status of the sensor that reported the event at the time that the event was reported. _is_commissioned_: Boolean, required. Indicates whether the sensor is currently in a state where it should be reporting data. _is_online_: Boolean, required. Indicates whether the sensor is currently online and reporting data. |
| `event_source_category` | Enum [Sensor Category](#sensor-category) | Optional | General category of the source creating the event. |
| `source_operator_id` | [UUID][uuid] | Optional | Unique identifier of the entity responsible for operating the event source. |
| `source_id` | [UUID][uuid] | Optional | Unique identifier of this event source, whether sensor, vehicle, camera, etc. Allows agencies to connect related Events as they are recorded. |

[Top][toc]

[toc]: #table-of-contents
[uuid]: /general-information.md#uuid
[ts]: /general-information.md#timestamp
[polygon]: /general-information.md#polygon
