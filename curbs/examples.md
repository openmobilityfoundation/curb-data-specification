# Curbs Examples

This file presents a series of CDS [Curbs](/curbs) endpoint examples to use as templates.

## Table of Contents

- [Curb Zones Minimum](#curb-zones-minimum)
- [Curb Policies](#curb-policies)

## Curb Zones Minimum

A [Query Curb Zones](https://github.com/openmobilityfoundation/curb-data-specification/tree/feature-release-work-1/curbs#query-curb-zones) example of `/curbs/zone` with minimum required fields returned, plus a max parking stay of 60 minutes, and some parameters passed in.

**Query**: 

`/curbs/zone?include_geometry=true&include_policies=true&timestamp=1552678594427`

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

## Curb Policies

A [Query Curb Policies](https://github.com/openmobilityfoundation/curb-data-specification/tree/feature-release-work-1/curbs#query-curb-policies) example of `/curbs/policy` showing policies giving 3 fleet operators using electric vehicles operating rideshare curb access for max 15 minute drop-offs on weekdays from 10am to 4pm, other commerical activity for up to 60 minutes between 8am and 10pm, but otherwise no stopping (eg overnight). No query parameters passed in.

**Query**: 

`/curbs/policy`

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
      "policies": [
        {
          "curb_policy_id": "cd0996d7-3765-4f0b-a72e-7caf7cf3fe21",
          "published_date": 1552678594428,
          "priority": 1,
          "data_source_operator_id": [
            "b2046faf-2bc2-4f0e-b784-7cc746138555", 
            "aba63473-351c-4624-93ab-456db34f83a6", 
            "984cae91-3a11-49eb-b68c-d325a6cc8970"
          ],
          "time_spans": [
            {
              "days_of_week": ["mon", "tue", "wed", "thu", "fri"],
              "time_of_day_start": "10:00",
              "time_of_day_end": "16:00"
            }
          ],
          "rules": [
            {
              "activity": "parking",
              "max_stay": 15,
              "user_classes": [
                "rideshare", "electric"
              ]
            }
          ]
        },{
          "curb_policy_id": "51f58575-1042-4254-b5fc-fed97124a6c7",
          "published_date": 1552678857362,
          "priority": 2,
          "time_spans": [
            {
              "time_of_day_start": "08:00",
              "time_of_day_end": "22:00"
            }
          ],
          "rules": [
            {
              "activity": "parking",
              "max_stay": 60
            }
          ]
        },{
          "curb_policy_id": "8c0abb35-b8d2-469e-bdb1-b6de52c430ac",
          "published_date": 1552678857362,
          "priority": 3,
          "rules": [
            {
              "activity": "no stopping"
            }
          ]
        }
      ] 
    }
  ]
}
```

[Top](#table-of-contents)
