# Curb Data Specification: **Data Types**

This CDS data types page catalogs the data objects (fields, types, requirements, descriptions) used across CDS APIs and endpoints.

## Table of Contents

- [Custom Attributes](#custom-attributes)
- [Enforcement](#enforcement)
   - [Violations](#violations)
- [External Reference](#external-reference)

## Custom Attributes

Custom Attributes are optional additional attributes that do not fit in other fields and objects in CDS. They are unique for the organizations created and consuming the endpoint, that may not apply to other jurisdictions. Examples include custom identifiers, information required by ordinance, vendor attributes, supplemental data, etc. 

The format is one or more JSON name/value pairs, and the values must be a string. If a `custom_attributes` field is provided in Curbs, Areas, Spaces, Objects, or Events endpoints, then the relevant [endpoint](general-information.md#rest-endpoints) must contain the `custom_attribute_dictionary` field, which describes details of the custom fields provided.

Before creating any custom attributes, the preference is to use existing CDS fields and data objects first. If the fields and data you provide in custom attributes apply to multiple jurisdictions, vendors, and/or scenarios, please open an issue to include new fields in a future CDS release.


[Top][toc]

## Enforcement

The Enforcement object describes a specific set of features from a data feed or API that is relevant to an enforcement Curb Event. This allows CDS users to reference detailed enforcement data separate from the main API endpoints.

Where a citation could represent multiple violations, an enforcement object contains an array that enumerates the violations for a single citation. Where a citation can only represent a single violation, multiple Curb Events may be published, each with a single violation in the array.

The `enforcement` object is a JSON *object* with the following fields:

| Name             | Type    | Required/Optional | Description   |
| ---------------- | ------- | ----------------- | ------------- |
| `enforcement_id` | UUID    | Required          | An identifer unique to the enforcement incident, generated the first time an enforcement event is recorded, and referenced in future related enforcement events. Multiple Curb Events (ex: `vehicle_violation_start`, `vehicle_violation_end`, or `citation_issued`) that relate to the same enforcement activity can share the same `enforcement_id`. | 
| `citation_id`    | String  | Optional          | A unique id which represents a single citation. |
| `is_warning`     | Boolean | Optional          | A boolean value to indicate if the enforcment action is being processed as a warning.  |
| `action_taken`   | String  | Optional          | Indicates how the violation was enforced. Typical well-known values are `citation_registered`, `citation_posted`, `citation_served`, or `citation_emailed`. |
| `citation_cost`  | String  | Optional          | The total cost of all violations associated to this enforcement action. |
| `violations`     | Array of [Violations](#violations) | Optional          | An array of Violation objects that indicate the one-to-many violations associated to this enforcement event. |

[Top][toc]

### Violations

The Violations object describes the violations associated to an enforcement action that can occur as a Curb Event. 

The `violations` object is a JSON *object* with the following fields:

| Name             | Type   | Required/Optional | Description   |
| ---------------- | ------ | ----------------- | ------------- |
| `violation_code` | String | Optional          | The unique code created by the municipality, city, county, state, federal, or enforcement agency to identify the type of rule being enforced. |
| `violation_name` | String | Optional          | The city/municipal, county, state, provincial, or federal code that was violated. |
| `violation_cost` | String | Optional          | The original cost associated with the violation. |

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
