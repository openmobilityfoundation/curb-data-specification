# Curbs Examples

This file presents a series of CDS [Curbs](/curbs) endpoint examples to use as templates.

## Table of Contents

- [Curb Zones Minimum](#curb-zones-minimum)

## Curb Zones Minimum

A [Query Curb Zones](https://github.com/openmobilityfoundation/curb-data-specification/tree/feature-release-work-1/curbs#query-curb-zones) example at `/curbs/zone` with minimum required fields returned, plus a max parking stay of 60 minutes, and some parameters passed in.

**Query**: 

`/curbs/zone?include_geometry=true&include_policies=true&timestamp=1552678594427`

**Payload**:

```json
{
  "version": "39a653be-7180-4188-b1a6-cae33c280343",
  "time_zone": "US/Eastern",
  "last_updated": 1552678594428,
  "currency": "USD",
  "author": "City of Metropolis",
  "license_url": "https://creativecommons.org/licenses/by/4.0/",
  "data": [
    {
      "zones": [
        {
          "curb_zone_id": "7d8a5885-e949-4ac9-afb7-fa4d43b68530",
          "geometry": {
            "type": "Polygon",
            "coordinates": [[
              [-73.982105, 40.767932],
              [-73.973694, 40.764551],
              [-73.949318, 40.796918],
              [-73.958416, 40.800686],
              [-73.982105, 40.767932]
            ]]
          },
          "curb_policy_ids": [
            "cd0996d7-3765-4f0b-a72e-7caf7cf3fe21"
          ],
          "published_date": 1552678594428,
          "last_updated_date": 1552678594428,
          "start_date": 1552678594428
        }
      ],
      "policies": [
        {
          "curb_policy_id": "cd0996d7-3765-4f0b-a72e-7caf7cf3fe21",
          "published_date": 1552678594428,
          "priority": 1,
          "rules": [
            {
              "activity": "parking",
              "max_stay": 60
            }
          ]
        }
      ] 
    }
  ]
}
```

[Top](#table-of-contents)
