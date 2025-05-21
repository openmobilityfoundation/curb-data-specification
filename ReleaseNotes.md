## 1.0.1

> Released: 2024-12-09

The 1.0.1 patch release cleans up and  clarifies some minor issues and typos to help make the specification clearer.

### CHANGES

See the closed [PRs](https://github.com/openmobilityfoundation/curb-data-specification/pulls?q=is%3Apr+is%3Aclosed+milestone%3A1.0.1) tagged with [Milestone 1.0.1](https://github.com/openmobilityfoundation/curb-data-specification/milestone/6?closed=1) and [Issues](https://github.com/openmobilityfoundation/curb-data-specification/issues?q=is%3Aissue+milestone%3A1.0.1+is%3Aclosed) for a full list of changes.

**Minor updates**

- OpenAPI support
- Reference to MDS
- Internal Links
- Event location description
- Street name description
- Accessibility user classes
- Rate maximum fee clarification
- New data source device name field
- New operators

### ACKNOWLEDGEMENTS

Thank you to our current and past [steering committee members](https://github.com/openmobilityfoundation/curb-data-specification/wiki#steering-committee), GitHub pull request and issue creators (Passport, CurbIQ, INRIX, Umojo, Clevercity, @jamesdwilson) for this release, and for the organizations that participated on our weekly [working group calls](https://github.com/openmobilityfoundation/curb-data-specification/wiki#meeting-agendas).

## What's Changed

**Full Changelog**: https://github.com/openmobilityfoundation/curb-data-specification/compare/1.0.0...1.0.1

### Additional Updates

See the [final release page](https://github.com/openmobilityfoundation/curb-data-specification/releases/tag/1.0.1) for any additional updates to these notes

## 1.0.0

> Released: 2022-04-29

> [Release Plan](https://github.com/openmobilityfoundation/curb-data-specification/wiki/Release-1.0.0)

The 1.0.0 major release is the first release of the Curb Data Specification (CDS) after over 17 months of community work across dozens of public meetings, with 70+ organizations and 160+ individuals participating. It includes three major APIs to define Curbs and policies, track Events at the curb and determine sensor status, and derive Metrics for sessions and aggregate curb usage with a well defined methodology. CDS is currently being used by over [two dozen organizations](https://github.com/openmobilityfoundation/curb-data-specification/wiki#cds-users).

### CHANGES

See work tagged with "Milestone 1.0.0" in [Pull Requests](https://github.com/openmobilityfoundation/curb-data-specification/pulls?q=is%3Apr+is%3Aclosed+milestone%3A1.0.0), [Issues](https://github.com/openmobilityfoundation/curb-data-specification/issues?q=is%3Aissue+milestone%3A1.0.0+is%3Aclosed), and [Discussions](https://github.com/openmobilityfoundation/curb-data-specification/discussions) for a full list of changes and history.

**_General CDS_**

- Creation of spec from working group drafts and community code submissions
  - See [About CDS](https://www.openmobilityfoundation.org/about-cds/) web page for high level details

**_Curbs_**

- The Curbs API is a standard way for cities to digitally publish curb locations and regulations, which can be shared with the public and with companies using curb space. It defines curb policies, curb zones, spaces in zones, and areas around curbs, and is used by Events and Metrics.

**_Events_**

- The Events API is a standard way to transmit real-time and historic commercial events happening at the curb to cities. Event data can be derived from company data feeds, on street sensors, session payments, company check-ins, in-person parking personnel, and/or other city data sources. Connected to Curbs and used by Metrics.

**_Metrics_**

- The Metrics API is a way to track curb usage session details and define common calculation methodologies to measure historic dwell time, occupancy, usage and other aggregated statistics. It defines sessions and aggregates data derived from raw Events data.

_Minor Updates_

- Formatting of spec, links, TOCs, headers, etc
