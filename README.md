# Curb Data Specification

## Table of Contents

- [Overview](#overview)
- [Work in Progress](#work-in-progress)
- [Working Group](#working-group)
- [Curb Data Specification APIs](#curb-data-specification-apis)

# Overview

Urban curb is a valuable, limited, and often under-managed part of the public right of way. Curb demand is growing, including from commercial activity like passenger pickup/drop off, traditional and on-demand delivery services, new mobility programs like scooters, bikeshare, and carshare, and goods and freight delivery. While cities have made some progress in digitizing their curb and using curb data, more tools are needed to proactively manage curbs and sidewalks, and in doing so deliver more public value from this scarce resource. Curb data standards can provide a mechanism for expressing static and dynamic regulations, measuring activity at the curb, and providing access and utilization for curb users.

The Curb Data Specification (CDS) will help cities and companies pilot and scale dynamic curb zones that optimize commercial loading activities of people and goods, and measure the impact of these programs.

In the short term we will produce standards that are necessary to support dynamic curb utilization use cases for commercial loading activities by privately operated companies. In the long term we want to help cities manage their curb zone programs and surrounding areas and measure the utilization and impact for all use cases.

The CDS will be consumed by both cities and transportation providers operating in the public right of way. In many cases, the same mobility providers using curbs with CDS may also be interacting with other OMF [MDS Policy](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/policy), [MDS Provider](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/provider), and [MDS Agency](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/agency) data objects within the same [MDS Jurisdiction](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/jurisdiction), and [MDS Geography](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/geography). Consistent with the Technology Design Principles codified in the OMF [Architectural Landscape Document](https://github.com/openmobilityfoundation/governance/blob/main/documents/OMF-MDS-Architectural-Landscape.pdf), the members of this working group are making reasonable best efforts to ensure that work is both _modular_ and _interoperable_ with other technology managed by the OMF as to avoid duplication and downstream implementation complexity.

[Top][toc]

# Work in Progress

The CDS is a work in progress and is under ongoing development by the community under the guidance of the [Working Group Steering Committee](https://github.com/openmobilityfoundation/curb-data-specification/wiki) on specific [discussion topics](https://github.com/openmobilityfoundation/curb-data-specification/discussions) and bi-weekly [public meetings](https://github.com/openmobilityfoundation/curb-data-specification/wiki#meeting-agendas). Work and dicussion is also happening in the [Curb Data Specification - Draft and Ideas Doc](https://docs.google.com/document/d/1UgD2fHtVyIMwNG-qXJRv-gcY7nCdYxokbMsLTs32Em4/edit?usp=sharing) before being committed to GitHub. The goal is to release a beta version of the spec by the end of 2021 and incorporate feedback from pilot programs into future versions.

[Top][toc]

# Working Group

The OMF’s [Curb Management Working Group](https://github.com/openmobilityfoundation/curb-data-specification/wiki), led by a steering committee of individuals representing public agencies and private sector companies, is developing this data specification for curb usage.  The CDS will encompass digitized curb regulations, real time and historic event information, and utilization metrics. The specification will allow sharing of this data between cities, commercial companies, and the public. Get involoved by siging up to the [Curb Mailing List](https://groups.google.com/a/openmobilityfoundation.org/g/wg-curb).

[Top][toc]

# Curb Data Specification APIs

CDS is at its core a set of Application Programming Interfaces (APIs) and endpoints within those APIs, which allow information to flow between organizations using and managing the curb areas. It includes the following APIs and their associated endpoints:

- [Curbs API](/CurbsAPI/README.md)
- [Events API](/TODO)
- [Metrics API](/TODO)

CDS is a data exchange format and a translation layer between internal systems and external entities using data feeds. It is not expected that CDS will be the format used internally to store curb regulations in a city. The internal storage format is something different, and a subset of that data should be able to be converted to CDS for publishing out to the public and curb users. 

[Top][toc]

[toc]: #table-of-contents
