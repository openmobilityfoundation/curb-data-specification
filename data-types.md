# Curb Data Specification: **Data Types**

This CDS data types page catalogs the data objects (fields, types, requirements, descriptions) used across CDS APIs and endpoints.

## Table of Contents

- [Enforcement](#enforcement)
- [External Reference](#external-reference)

## Enforcement

The Enforcement object describes a specific set of features from a data feed or API that is relevant to an enforcement Curb Event. This allows CDS users to reference detailed enforcement data separate from the main Event endpoint.

The `enforcement` object is a JSON *array* with the following fields:

| Name             | Type   | Required/Optional   | Description   |
| ---------------- | ------ | ------------------- | ------------- |
| `municipal_code` | string | Required            | The unique code created by the municipality or enforcement agency to identify the type of rule being enforced. |
| `ticket_id`      | String | Required            | The unique id that represents the ticket being given. |
| `name`           | String | Optional            | Name of the rule being enforced or citation being given. |
| `action_taken`   | String | Optional            | What action was taken to enforce the rule being violated. Typical well-known values are `ticket_served`, `ticket_posted`, `ticket_registered`, or `ticket_emailed`. |
| `ticket_cost`    | String | Optional            | The cost associated with the given violation/ticket issued. |

[Top][toc]

## External Reference

An External Reference object describes a specific feature from an external data source that is relevant to a part of CDS data. This allows CDS users to reference other data sources that impact or provide information about a CDS object, and see more details at an external URL. Data sources can be anything available via a URL, including an existing data standard (MDS, WZDx, CWZ, GTFS, GBFS, CDS, etc), a custom feed, API, document, web page, report, etc.

The `external_reference` object is a JSON *array* with the following fields:

| Name              | Type    | Required/Optional   | Description   |
| ----------------- | ------- | ------------------- | ------------- |
| `reference_url`   | URL     | Required            | A web-accessible identifier URL for the source of the publicly or privately accessible data feed, document, website, etc. This MUST be a full HTTPS URL pointing to a location which contains more information impacting or explaining the location, event, or policy, etc. |
| `name`            | String  | Optional            | Name of the data source for reference. E.g. "WZDx", "CWZ", "MDS", "GBFS", "GTFS", "CDS". |
| `public`          | Boolean | Optional            | Is this data source able to be viewed with out any sort of authentication? If `true`, the `reference_url` is public. If `false`, the `reference_url` requires some sort of authentication, authorization, or API key to access. This is an informational field to set access expectations for the feed user, and does not provide any credentials directly unless explicitly contained in the `reference_url`. |
| `identifier_name` | String  | Optional            | The name of the data field or object that is referenced by the unique `ids`. E.g. "id", "trip_id", "vehicle_id", "RoadEventFeature", etc, if relevant and available in `reference_url`. |
| `ids`             | Array of Strings | Optional   | An array of one or more **ids** from the `reference_url` that impacts the use of or relationship to part of CDS, e.g. a curb zone, curb space, curb area, curb event, etc. The **ids** and their details are be found in the referenced `reference_url`. |

[Top][toc]

[toc]: #table-of-contents
