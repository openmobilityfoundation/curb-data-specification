# Events Examples

This file presents a series of CDS [Events](/events) endpoint examples to use as templates.

## Table of Contents

- [Curb Event Minimum](#curb-event-minimum)

## Curb Event Minimum

A [Query Event](/events#query-event) example of `/events/event` with minimum required fields returned, and no parameters passed in.

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
          "event_id": "7d8a5885-e949-4ac9-afb7-fa4d43b68530"
        }
      ]
    }
  ]
}
```

[Top](#table-of-contents)
