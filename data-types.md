# Curb Data Specification: **Data Types**

This CDS data types page catalogs the data objects (fields, types, requirements, descriptions) used across CDS APIs and endpoints.

## Table of Contents

- [External Reference](#external-reference)
- [Enforcement Reference](#enforcement-reference)

## External Reference

An External Reference object describes a specific feature from a data feed or API that is relevant to the curb place via an external reference. This allows CDS users to reference other data feeds, documents, websites, or URLs that impact or provide information, and see more details in an external URL. Data feeds can be any existing standard (MDS, WZDx, CWZ, GTFS, GBFS, CDS, etc), custom feed, document, web page, etc.

An `external_reference` is a JSON *array* with the following fields within objects:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `data_feed_url` | URL | Required | A web-accessible identifier for the source of the publicly or privately accessible data feed, document, website, or URL. This MUST be a full HTTPS URL pointing to a location which contains more information impacting or explaining the location or policy. |
| `name` | String | Optional | Name of the data feed for reference. E.g. "WZDx", "CWZ", "MDS", "GBFS", "GTFS", "CDS". |
| `public` | Boolean | Optional | Is this data feed able to be viewed with out any sort of authentication? If `true`, the `data_feed_url` is public. If `false`, the `data_feed_url` requires some sort of authentication, authorization, or API key to access. This is an informational field to set access expectations for the feed user, and does not provide any credentials directly unless explicitly contained in the `data_feed_url` URL string. |
| `identifier_name` | String | Optional | The name of the data field or object that is referenced by the unique `ids`. E.g. "id", "trip_id", "vehicle_id", "RoadEventFeature", etc, if relevant and available in the URL. |
| `ids` | Array of Strings | Optional | An array of one or more **ids** from the data feed that impacts the use of or relationship to part of CDS, e.g. a curb zone, curb space, curb area, curb event, etc. The **ids** and their details are be found in the referenced `data_feed_url`. |

[Top][toc]

## Enforcement Reference

The Enforcement Reference object describes a specific set of features from a data feed or API that is relevant to an enforcement Curb Event. This allows CDS users to reference detailed enforcement data separate from the main Event endpoint.

An `enforcement_reference` is a JSON *array* with the following fields within objects:

| Name   | Type   | Required/Optional   | Description   |
| ------ | ------ | ------------------- | ------------- |
| `municipal_code` | string | Required | The unique code created by the municipality or enforcement agency to identify the type of rule being enforced. |
| `ticket_id` | String | Required | the unique id that represents the ticket being given. |
| `name` | String | Optional | Name of the rule being enforced or citation being given. |
| `action_taken` | String | Optional | what action was taken to enforce the rule being violated. Typical values are `ticket_served`, `ticket_posted`, `ticket_registered`, or `ticket_emailed`. |
| `ticket_cost` | String | Optional | the cost associated with the given violation/ticket issued. |

[Top][toc]

[toc]: #table-of-contents
