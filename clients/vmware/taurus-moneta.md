---
title: Taurus/Moneta
description: 
published: true
date: 2022-06-29T17:55:47.546Z
tags: 
editor: markdown
dateCreated: 2022-06-28T21:50:41.851Z
---

# Taurus / Moneta

## Org Chart (2022-06-27)

### Job Titles
- In engineering, senior -> staff -> senior staff
- In management, senior manager -> director -> senior director

### People
- Archana Balakrishnan - Product (https://www.linkedin.com/in/archanabalakrishnan/)
- Stefan Pulov - Director tech side (https://www.linkedin.com/in/stefan-pulov-01a0625/)
- Dragomir Nikolov - Sr. Director tech side (https://www.linkedin.com/in/dragomir-nikolov-8481443/) (promoted a year ago, coincides with SC success)
- Denitsa Gencheva - PM tech side? (https://www.linkedin.com/in/denitsa-gencheva-56835256/)
- Vladimir Petkov - Technical Lead for "Control Plane" data catalog (https://www.linkedin.com/in/vladimir-petkov/)
  > Technical lead of the control plane for VMware data analytics platform. The microservices based system enabled over 130 data teams to integrate with the data platform in a complete self-service manner reducing support work with 70%.
  > Designed metadata management system for VMware's data platform capable of storing data catalogue and lineage information for hundreds of databases and tens of thousands of tables.
- Stefan Vladov - Tech lead of Lakehouse (https://www.linkedin.com/in/stefan-vladov-278bb612/)
- Konstantin Georgiev - contractor Scrum Master from Aardwark (https://www.aardwark.com/?lang=en)? (https://www.linkedin.com/in/konstantingeorgievsofia/) (4 months; Aardwark does IT outsourcing in Czech Republic and Slovakia)
- Rumen Barov - Tech lead above service level? (https://www.linkedin.com/in/pymeh/)
  > Currently on the drawing board to find a way beyond trillions of DB rows and into the quadrillion range.

## Meetings

### Kickoff (Thu 2022-06-23 8 am to 9 am PDT)
- People
  - VMware
    - Archana
    - Stefan
  - Qbiz
    - Mayra
    - Jenny
- Notes
  - Archana driving process; this is a PM engagement.
  - Data governance taken off the table as a leading offering; will evaluate "capture the kitchen sink" features as a vitamin/not-quite-painkiller to augment value of other offering.
  - Stefan named "DataOps" and IaaS (my term) as two other areas of product offering.
  - "DataOps" defined as best SW-like engineering practices for DEs.
  - Prioritized IaaS, AWS/Azure/GCP-style, as high-level approach for incremental launch of services in a VMware data portfolio.
  - Will f/u with meetings with individual service leads in tech-side (R&D) to evaluate internal product one at a time.

### Data Catalog (Tue 2022-06-28 8:30 am to 9:30 am PDT)
- People
  - VMware
    - Denitsa
    - Vladimir
    - Rumen
  - Qbiz
    - Jenny
- Notes
  - Vlado head of data catalog.
  - Part of http://supercollider.vmware.com/ internal web app
    - Has query interface.
    - Metadata driving Google-search-like discoverability (similar to Amundsen).
    - Metadata showing ownership for responsibility.
    - Hinted at data lineage (did not see demo).
    - In lower left corner, "powered by Taurus"; don't yet know what Taurus is separate from SC.
  - SC currently serving 150 internal teams.
  - Three sets of hypotheses, mainly driven by Rumen:
    - Is data governance compelling?
      - Jenny
        > No, current-gen is more active in the vendor space for DG b/c precision reqs lower, therefore product can exist to meet the market need.
        > Biggest current issue is failure during implementation / rollout of DG.
      - Vlado confirmed this is internal experience.
    - Discoverability?
      - Jenny
        > Problem with discoverability is that smaller companies do not have teams touching the same datasets, usually, i.e. marketing analysts are the only ones touching marketing datasets. On small teams, just ask the person, no need for discoverability.
        > Necessarily smaller market size b/c of scale of personnel involved (as opposed to scale of semantic complexity).
      - Rumen and Deni
        > How about discoverability to nail down key datasets that you depend on, i.e. proactively make sure upstream teams don't move your cheese?
      - Jenny
        > Temporary problem. Once key data dependencies are known, you formalize into team structure with dedicated SLA. Don't feel this is compelling at most places.
        - Did not say during meeting that temporary problem w/ good product is a service sales opportunity, will f/u.
        > May be compelling at orgs with shifting reqs. Personal experience is venture-backed startups with data objectives that shift with each series raised.
    - DevOps support, i.e. troubleshooting?
      - Jenny
        > Tooling has to be very good to overcome "2 am in pilot seat stupid" effect.
        > Much bigger market though.
        > Very immature.
        > Amundsen came out of that; the more tech sophisticated metadata plays are ops in nature b/c both precise and larger market demand.
        - Open source is driving this; not ready to make the leap into vendor space. Current adopters are very open-source biased and are contribs themselves. Something to f/u w/ Rumen on.
  - Two key competitor questions:
    - Open source
      - OpenLineage
      - Egeria
      - Jenny
        > Never heard of them.
        > At cursory glance during call:
        > OpenLineage is a good effort to abstract interface for storing "verb log".
        > Egeria is command-and-control Semantic Web. Non-starter.
        - Recommend mid-term investment in OpenLineage.
    - Collibra
      - Jenny
        > Raised the granularity issue, i.e. emails vs. structured data like tuplesets.
        - Recommend mid-term investment in data lineage visualization on tupleset primitives. Will f/u with Vlado and Rumen.
  - Jenny
    > Philosophy of engagement:
    > Figure out definite "no's".
    > Identify "maybe no's" and "maybe yes's" to f/u on.
    > Rapidly converge on 2-3 top "maybe yes's".
    > Friday check-in with Archana.