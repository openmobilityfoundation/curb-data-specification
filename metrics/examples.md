# Metrics Examples

This file presents a series of CDS [Metrics](/metrics) endpoint examples to use as templates.

## Table of Contents

- [Dynamic Sessions](#dynamic-sessions)
- [Static Aggregate Metrics](#static-aggregate-metrics)

## Dynamic Sessions

A [Query Session](/metrics#query-session) example of `/metrics/session` with all fields returned for parking events in a specific zone over a short time period. Dynamic endpoint with parameters passed in.

**Query**: 

`/metrics/session?curb_place_type=zone&curb_place_id=d3c862b1-5404-4635-a90b-056537c50e81&start_time=1641738460&end_time=1642119064`

**Payload**:

```csv
session_type,event_id_start,event_id_end,event_location_start_latitude,event_location_start_longitude,event_location_end_latitude,event_location_end_longitude,event_time_start,event_time_end,curb_zone_id,vehicle_length,vehicle_type
parking,f49333b2-d693-4f5b-b187-1a1acd720eae,4cfa75aa-d044-436f-951f-0f3037de0d29,38.257341,-85.7629702,38.256330,-85.7629111,1641738560,1641918255,d3c862b1-5404-4635-a90b-056537c50e81,120,cargo_bicycle
parking,0843d2e7-ce60-44da-a609-fb4317ef15c6,d8599307-27b4-4161-be9a-c0d097cef54d,38.257352,-85.7629804,38.257331,-85.7629810,1641839464,1641919041,d3c862b1-5404-4635-a90b-056537c50e81,670,truck
parking,74e9d1d4-9037-4347-9135-f72010311ef7,f31f1615-adad-4542-83c8-9c858f9ecd0f,38.257349,-85.7629763,38.257836,-85.7630018,1641940456,1642019039,d3c862b1-5404-4635-a90b-056537c50e81,380,van
parking,35c923fd-78f5-471d-a548-efb0dabca3b7,c702915e-9b2e-45a5-ab30-f4413cdb1a1b,38.257359,-85.7629518,38.258139,-85.7629615,1642140839,1642119050,d3c862b1-5404-4635-a90b-056537c50e81,980,freight
```

[Top](#table-of-contents)

## Static Aggregate Metrics

A [Query Aggregate](/metrics#query-aggregate) example of `/metrics/aggregate` that collects 2 aggregate metrics in 2 places over a 1 day period. Static endpoint so no parameters passed in.

**Query**: 

`/metrics/aggregate`

**Payload**:

```csv
curb_place_type,curb_place_id,metric_type,date,hour,value
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,0,3
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,1,7
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,2,2
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,3,7
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,4,9
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,5,10
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,6,13
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,7,16
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,8,17
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,9,13
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,10,15
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,11,19
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,12,29
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,13,27
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,14,26
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,15,38
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,16,34
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,17,35
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,18,33
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,19,20
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,20,16
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,21,12
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,22,-1
Zone,0f6a052d-f934-4159-8da4-3135e453b968,total_sessions,2021-10-31,23,8
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,0,10.5
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,1,5.0
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,2,3.0
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,3,2.9
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,4,8.3
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,5,14.5
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,6,15.3
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,7,16.2
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,8,18.1
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,9,21.3
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,10,15.2
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,11,12.1
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,12,11.8
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,13,9.3
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,14,5.7
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,15,6.7
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,16,9.2
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,17,6.4
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,18,8.3
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,19,10.3
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,20,13.5
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,21,14.2
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,22,12.1
Space,7e6be807-ff61-4f12-85c8-0fb740b24436,average_dwell_time,2021-10-31,23,9.6
```

[Top](#table-of-contents)
