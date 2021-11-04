# Curb Data Specification

## Beta
> ⚠ **This repository is current a draft of the initial CDS beta, and not an official release. It will be changed and updated as development and real-world feedback happens and we head for a 1.0 beta release. See our current [timeline](https://github.com/openmobilityfoundation/curb-data-specification/wiki#timeline) and watch the [CDS Releases](https://github.com/openmobilityfoundation/curb-data-specification/releases) page for updates.**

## Table of Contents

- [Overview](#overview)
- [Work in Progress](#work-in-progress)
- [Get Involved](#get-involved)
- [Curb Data Specification APIs](#curb-data-specification-apis)
  - [Modularity](#modularity)
  - [Structure](#structure)
- [Versions](#versions)
  - [Technical Information](#technical-information)
- [Membership](#membership) 

# Overview

Urban curb is a valuable, limited, and often under-managed part of the public right of way. Curb demand is growing, including from commercial activity like passenger pickup/drop off, traditional and on-demand delivery services, new mobility programs like scooters, bikeshare, and carshare, and goods and freight delivery. While cities have made some progress in digitizing their curb and using curb data, more tools are needed to proactively manage curbs and sidewalks, and to deliver more public value from this scarce resource. Curb data standards can provide a mechanism for expressing static and dynamic regulations, measuring activity at the curb, and developing policies that create more accessible, useful curbs.

The Curb Data Specification (CDS) will help cities and companies pilot and scale dynamic curb zones that optimize commercial loading activities of people and goods, and measure the impact of these programs.

The CDS will be consumed by both cities and transportation providers operating in the public right of way. In many cases, the same mobility providers using curbs with CDS may also be interacting with other OMF [MDS Policy](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/policy), [MDS Provider](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/provider), and [MDS Agency](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/agency) data objects within the same [MDS Jurisdiction](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/jurisdiction) or [MDS Geography](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/geography), and using similar [MDS Metrics](https://github.com/openmobilityfoundation/mobility-data-specification/tree/main/metrics). Consistent with the Technology Design Principles codified in the OMF [Architectural Landscape Document](https://github.com/openmobilityfoundation/governance/blob/main/documents/OMF-MDS-Architectural-Landscape.pdf), the members of this working group are making reasonable best efforts to ensure that work is both _modular_ and _interoperable_ with other technology managed by the OMF as to avoid duplication and downstream implementation complexity.

[Top][toc]

# Work in Progress

The CDS is a work in progress and is under ongoing development by the community under the guidance of the [Working Group Steering Committee](https://github.com/openmobilityfoundation/curb-data-specification/wiki) on specific [discussion topics](https://github.com/openmobilityfoundation/curb-data-specification/discussions) and bi-weekly [public meetings](https://github.com/openmobilityfoundation/curb-data-specification/wiki#meeting-agendas) around a specific [Scope of Work](https://docs.google.com/document/d/1yY5pRiiei8seVxXOmcYiqCA7wx7DueeatJnjbZoV3hU/). Work and dicussion is also happening in the [Curb Data Specification - Draft and Ideas Doc](https://docs.google.com/document/d/1UgD2fHtVyIMwNG-qXJRv-gcY7nCdYxokbMsLTs32Em4/edit?usp=sharing) before being committed to GitHub. The Steering Committee has created supplemntary resources the [Architectural Decisions](https://github.com/openmobilityfoundation/curb-data-specification/wiki/Curb-Architectural-Decisions), [Curbs Use Cases](https://github.com/openmobilityfoundation/curb-data-specification/wiki/CDS-Curbs-Use-Cases), and [Pilot Project Guide](https://docs.google.com/document/d/1Mb1wpy4AFJ1MLvDpSIJsii4_1j2JC16NxNUVPI5BMNM/edit?usp=sharing) pages. The goal is to release a beta version of the spec by the end of 2021 and incorporate feedback from pilot programs into future versions.

[Top][toc]

# Get Involved

The OMF’s [Curb Management Working Group](https://github.com/openmobilityfoundation/curb-data-specification/wiki), led by a steering committee of individuals representing public agencies and private sector companies, is developing this data specification for curb usage.  The CDS will encompass digitized curb regulations, real time and historic event information, and utilization metrics. The specification will allow sharing of this data between cities, commercial companies, and the public. Get involoved by siging up to the [Curb Mailing List](https://groups.google.com/a/openmobilityfoundation.org/g/wg-curb).

The Curb Data Specification is an open source project with all development taking place on GitHub. Read our **[Community Wiki](https://github.com/openmobilityfoundation/curb-data-specification/wiki)** to get started. Comments and ideas can be shared by [starting a discussion](https://github.com/openmobilityfoundation/curb-data-specification/discussions), [creating an issue](https://github.com/openmobilityfoundation/curb-data-specification/issues), and specific changes can be suggested by [opening a pull request](https://github.com/openmobilityfoundation/curb-data-specification/pulls). Before contributing, please review our OMF [CONTRIBUTING page](https://github.com/openmobilityfoundation/governance/blob/main/CONTRIBUTING.md) and our [CODE OF CONDUCT page](https://github.com/openmobilityfoundation/governance/blob/main/CODE_OF_CONDUCT.md) to understand guidelines and policies for participation.

For questions about CDS please contact by email at [info@openmobilityfoundation.org](mailto:info@openmobilityfoundation.org) or [on our website](https://www.openmobilityfoundation.org/get-in-touch/). Media inquiries to [media@openmobilityfoundation.org](mailto:media@openmobilityfoundation.org).

[Top][toc]

# Curb Data Specification APIs

CDS is at its core a set of Application Programming Interfaces (APIs) and endpoints within those APIs, which allow information to flow between organizations using and managing the curb areas. It includes the following APIs and their associated endpoints:

1. [Curbs API](/curbs)
1. [Events API](/events)
1. [Metrics API](/metrics)

CDS is a data exchange format and a translation layer between internal systems and external entities using data feeds. It is not expected that CDS will be the format used internally to store curb regulations in a city. The internal storage format is something different, and a subset of that data should be able to be converted to CDS for publishing out to the public and curb users. 

Many parts of the CDS definitions and APIs align across each other. In these cases, consolidated information can be found on the [General Information](/general-information.md) page.

[Top][toc]

## Modularity

CDS is designed to be a modular and flexible specification. Regulatory agencies can use the components of the API that are appropriate for their needs. An agency may choose to use only Curbs, while others may use Curbs, Events, and Metrics. Even within each API many endpoints and fields are optional. This design allows agencies, software and hardware companies, and curb users to use what's appropriate for their use cases, work within their operational capabilities, and text CDS in their pilot projects.

## Structure

CDS contains a series of connected endpoints and fields beneath each interconnected API.

![CDS Structure](https://i.imgur.com/L44996v.gif)

[Top][toc]

# Versions

CDS has a **current release**, and an **upcoming releases** in development. For a full list of releases, their status, recommended versions, and timelines, see the [Official CDS Releases](https://github.com/openmobilityfoundation/curb-data-specification/wiki/Releases) page.

## Technical Information

The latest CDS release is in the [`main`](https://github.com/openmobilityfoundation/curb-data-specification/tree/main) branch, and development for the next release occurs in the [`dev`](https://github.com/openmobilityfoundation/curb-data-specification/tree/dev) branch.

The CDS specification is versioned using Git tags and [semantic versioning](https://semver.org/). 

* [Latest Release Branch](https://github.com/openmobilityfoundation/curb-data-specification/tree/main) (main)
* [Development Branch](https://github.com/openmobilityfoundation/curb-data-specification/tree/dev) (dev)
* [All GitHub Releases](https://github.com/openmobilityfoundation/curb-data-specification/releases)
* [CDS Releases](https://github.com/openmobilityfoundation/curb-data-specification/wiki/Releases) - current/recommended versions, timeline

[Top][toc]

# Membership

OMF Members (public agencies and commercial companies) have additional participation opportunities with leadership roles within our OMF [governance](https://github.com/openmobilityfoundation/governance#omf-scope-of-work):

- [Board of Directors](https://www.openmobilityfoundation.org/about/)
- [Privacy, Security, and Transparency Committee](https://github.com/openmobilityfoundation/privacy-committee)
- [Technology Council](https://github.com/openmobilityfoundation/governance/wiki/Technology-Council)
- [Strategy Committee](https://github.com/openmobilityfoundation/governance/wiki/Strategy-Committee)
- [Advisory Committee](https://github.com/openmobilityfoundation/governance/wiki/Advisory-Committee)
- Steering committees of all [Working Groups](https://github.com/openmobilityfoundation/mobility-data-specification/wiki#omf-meetings), currently:
  - [MDS Working Group](https://github.com/openmobilityfoundation/mobility-data-specification/wiki/MDS-Working-Group)
  - [Curb Working Group](https://github.com/openmobilityfoundation/curb-data-specification/wiki)

Read about [how to become an OMF member](https://www.openmobilityfoundation.org/how-to-become-a-member/), [how to get involved and our governance model](https://www.openmobilityfoundation.org/how-to-get-involved-in-the-open-mobility-foundation/), and [contact us](https://mailchi.mp/openmobilityfoundation/membership) for more details. 

[Top][toc]

[toc]: #table-of-contents
