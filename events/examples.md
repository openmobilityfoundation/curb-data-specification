# Events Examples

This file presents a series of CDS [Events](/events) endpoint examples to use as templates.

## Table of Contents

- [Curb Event Minimum](#curb-event-minimum)
- [Fleet Operator](#fleet-operator)

## Curb Event Minimum

A [Query Event](/events#query-event) example of `/events/events` with minimum required fields showing the start of a `park_start` event at a specific curb zone detected by an above ground sensor, and no [query parameters](/events#query-parameters) passed in.

**Query**: 

`/events/events`

**Payload**:

```json
{
  "version": "1.0",
  "time_zone": "US/Eastern",
  "last_updated": 1552678594428,
  "currency": "USD",
  "author": "City of Metropolis",
  "license_url": "https://creativecommons.org/licenses/by/4.0/",
  "data": [
    {
      "events": [
        {
          "event_id": "7d8a5885-e949-4ac9-afb7-fa4d43b68530",
          "event_type": "park_start",
          "event_location": {
            "type": "Feature",
            "properties": { "timestamp": 1552678574581 },
            "geometry": {
              "type": "Point",
              "coordinates": [ -85.7629808, 38.257341 ]
            }
          },
          "event_time": "1552678578632",
          "event_publication_time": "1552678594428",
          "curb_zone_id": "02885f6f-ef93-441b-9e74-487279380656",
          "data_source_type": "above_ground",
          "data_source_device_id": "0fcb51b0-4529-4d28-a339-81e9a9bbcdb3"
        }
      ]
    }
  ]
}
```

## Fleet Operator

A [Query Event](/events#query-event) example of `/events/events` from a fleet operator's data feed with many optional fields showing the start of a `park_start` with event details and vehicle properties, and the curb_zone_id [query parameter](/events#query-parameters) passed in.

**Query**: 

`/events/events?curb_zone_id=02885f6f-ef93-441b-9e74-487279380656`

**Payload**:

```json
{
  "version": "1.0",
  "time_zone": "US/Eastern",
  "last_updated": 1552678594428,
  "currency": "USD",
  "author": "City of Metropolis",
  "license_url": "https://creativecommons.org/licenses/by/4.0/",
  "data": [
    {
      "events": [
        {
          "event_id": "7d8a5885-e949-4ac9-afb7-fa4d43b68530",
          "event_type": "park_start",
          "event_purpose": "parcel_delivery",
          "event_location": {
            "type": "Feature",
            "properties": { "timestamp": 1552678574581 },
            "geometry": {
              "type": "Point",
              "coordinates": [ -85.7629808, 38.257341 ]
            }
          },
          "event_time": "1552678578632",
          "event_publication_time": "1552678594428",
          "curb_zone_id": "02885f6f-ef93-441b-9e74-487279380656",
          "data_source_type": "data_feed",
          "data_source_operator_id": "16a85550-51d3-4f52-8ea9-e8a10306cab2",
          "data_source_operator_name": "USPS",
          "data_source_device_id": "618e92a4-e557-42a9-8f0a-0273545f7f49",
          "data_source_manufacturer": "Grumman",
          "data_source_model": "LLV",
          "vehicle_id": "USDOT 33213",
          "vehicle_permit_number": "KYVID-319380-A",
          "vehicle_length": "670",
          "vehicle_type": "truck",
          "vehicle_propulsion_types": ["electric"],
          "vehicle_blocked_lane_types": ["parking"]
        }
      ]
    }
  ]
}
```


[Top](#table-of-contents)
