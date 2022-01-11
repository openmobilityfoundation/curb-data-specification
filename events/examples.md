# Events Examples

This file presents a series of CDS [Events](/events) endpoint examples to use as templates.

## Table of Contents

- [Curb Event Minimum](#curb-event-minimum)

## Curb Event Minimum

A [Query Event](/events#query-event) example of `/events/event` with minimum required fields returned showing the start of a `park_start` event at a specific curb zone detected by an above ground sensor, and no parameters passed in.

**Query**: 

`/events/event`

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
            "properties": {},
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

[Top](#table-of-contents)
