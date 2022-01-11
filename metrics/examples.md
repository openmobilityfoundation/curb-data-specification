# Metrics Examples

This file presents a series of CDS [Metrics](/metrics) endpoint examples to use as templates.

## Table of Contents

- [Static Aggregate Metrics](#static-aggregate-metrics)

## Static Aggregate Metrics

A [Query Aggregate](/metrics#query-aggregate) example of `/metrics/aggregate` with 2 aggregate metrics in 2 places over a 1 day period. Static endpoint so no parameters passed in.

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
