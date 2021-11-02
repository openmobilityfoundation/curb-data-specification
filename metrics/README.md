# Curb Data Specification: Metrics API

The Metrics API is a REST API allowing historic metrics calculations based on Event activity that happened at defined Curb places.

## ⚠ Beta
> **This feature is current a draft of the initial CDS beta release. It will change as development and real-world feedback happens.**

## Endpoints

There are two different endpoints that are part of the Metrics API:

  - A [Activities](#curb-event) is infomration about an activity that occurs near, at, or within a pre-defined curb area. Activities is a subset of items from the Events API.  Activities is *optional*.
  - A [Aggregates](#status) is aggregated counts and methodology of curb events. Aggregates is *optional*.

## Table of Contents

- [REST Endpoints](#rest-endpoints)
  * [Update Frequency](#update-frequency)
  * [Query Activities](#query-activities)
  * [Query Aggregates](#query-aggregates)
- [Data Objects](#data-objects)
  * [Activities](#activities)
  * [Aggregates](#aggregates)

## REST Endpoints

All endpoints return a CSV file that can either be pre-computed or created on demand.

If returning data from a dynamic server, they MUST set the `Content-Type` header to `application/vnd.cds+csv;version=0.0` to support
versioning in the future.  Clients SHOULD specify an `Accept` header containing 
`application/vnd.cds+csv;version=0.0`. If the server receives a request that contains an `Accept`
header but does not include this value; it MUST respond with a status of `406 Not Acceptable`.

You may choose to serve a CSV file directly from a web-based file system, service, or data portal, in which case adding a header is not required.

### Update Frequency

The agency serving the data may choose how frequently they want to update the data. At least monthly is recommended but it may be longer or weekly, daily, hourly, or even more frequently.

[Top][toc]

###  Query Activities

Endpoint: `/metrics/activities`  
Method: `GET`  
`data` Payload: a CSV object with the following fields:
  - `activities`: an array of [Activities](#activity) objects

_Optional endpoint; if not implemented, a server should reply with 501 Not Implemented._

#### Query Parameters

No query parameters for this endpoint.

[Top][toc]

###  Query Aggregates

Endpoint: `/metrics/aggregates`  
Method: `GET`  
`data` Payload: a CSV object with the following fields:
  - `activities`: an array of [Aggregates](#aggregates) objects

_Optional endpoint; if not implemented, a server should reply with 501 Not Implemented._

#### Query Parameters

No query parameters for this endpoint.

[Top][toc]

## Data Objects 

### Activities

Activities are a subset of curb events, with some rows combined, some columns removed for clarity and privacy, and for some curb event types. 
Activities are meant to provide a granular view of activity happening around the curb places, so consumers can do their own analysis and aggregation.

An Activity is represented as a CSV object, whose fields are as follows, pulled from the Curb Events endpoint in the Events API:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `event_id` | [UUID][uuid] | Required | The globally unique identifier of the event that occurred. |
| `event_type` | [Event Type](#event-type) | Required | The event_type that happened for this event. |
| `event_location` | GeoJSON | Required | The geographic point location where the event occurred. |
| `event_time` | [Timestamp][ts] | Required | Time at which the event occurred. |
| `curb_zone_id` | [UUID][uuid] | Conditionally Required | Unique ID of the Curb Zone where the event occurred. Required for events that occurred at a known Curb Zone for ALL _event_types_. |
| `curb_area_ids` | [UUID][uuid] | Conditionally Required | Unique IDs of the Curb Area where the event occurred. Since Curb Areas can overlap, an event may happen in more than one. Required for events that occurred in a known Curb Area for these event_types:  _enter_area, exit_area, park_start, park_end_ |
| `curb_space_id` | [UUID][uuid] | Conditionally Required | Unique ID of the Curb Space where the event occurred. Required for events that occurred at a known Curb Space for these event_types: _park_start, park_end, enter_area, exit_area_ |
| `vehicle_length` | Integer | Conditionally Required | Approximate length of the vehicle that performed the event, in centimeters. Required for sources capable of determining vehicle length. |
| `vehicle_type` | [Vehicle Type](#vehicle-type) | Conditionally Required | Type of the vehicle that performed the event. Required for sources capable of determining vehicle type. |
| `propulsion_types` | Array of [Propulsion Type](#propulsion-type) | Conditionally Required | List of propulsion types used by the vehicle that performed the event. Required for sources capable of determining vehicle propulsion type. |

#### Event Types

The following Event Types are included in the Activities data, and the others event types are not returned.

- **park_start**: a vehicle stopped, parked, or double parked
- **park_end**: a parked vehicle leaving a parked or stopped state and resuming movement
- **enter_area**: vehicle enters the relevant geographic area
- **exit_area**: vehicle exits the relevant geographic area

