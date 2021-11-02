# Curb Data Specification: Metrics API

The Metrics API is a REST API allowing historic metrics calculations based on Event activity that happened at defined Curb places.

## ⚠ Beta
> **This feature is current a draft of the initial CDS beta release. It will change as development and real-world feedback happens.**

## Endpoints

There are two different endpoints that are part of the Events API:

  - A [Curb Event](#curb-event) is an activity that occurs near, at, or within a pre-defined curb area. Defining events is *required* as part of the Events API.
  - A [Status](#status) is the current status of a curb monitoring source. Event status is *optional*.

## Table of Contents

- [REST Endpoints](#rest-endpoints)
  * [Query Metrics](#query-metrics)
- [Data Objects](#data-objects)
  * [Metrics](#metrics)

## REST Endpoints

All endpoints return a JSON object containing the following fields:

TBD
