## 1.0.0

**Release Candidate submitted 2022-01-25**

[Release Plan](https://github.com/openmobilityfoundation/governance/wiki/Release-1.2.0)

The 1.0.0 major release is the first release of the Curb Data Specification (CDS) after over 15 months of community work. It includes three major APIs to define Curbs and policies, track Events at the curb and determine sensor status, and derive Metrics for sessions and aggregate curb usage with a well defined methodology.

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
