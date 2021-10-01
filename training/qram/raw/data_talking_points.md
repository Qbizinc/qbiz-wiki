---
title: Data Talking Points
description: 
published: true
date: 2021-10-01T22:25:45.943Z
tags: 
editor: markdown
dateCreated: 2021-09-28T23:02:42.705Z
---

# References
See https://qbiz-wiki.com/en/training/qram/raw/reading_list for resources.

Based on roundup of:
- Gartner, Forrester, G2 choice of how to categorize products
- Fortune, Forbes, Inc.com, CIO concentrations of talking points

# Summary
- There seem to be three lenses on the entire data space:
  - BI, which lumps in dashboarding
  - AI/ML
  - Supporting operations (governance down to infrastructure)

The purchasing audience is treated as distinct for each, so all of the industry talk treats them as separate, even though they all share 80% of the same infrastructure and relevant processes/tooling.

> The challenge is a set of talking points that allows Qbiz to pivot between these three contexts seamlessly to sell the large transformation projects. Also, when some C-level shows up asking for wholesale transformation, talking to the other stakeholders in those three distinct contexts without giving them context that yanks them out of their charter.
> -- Jenny Kwan

# BI vs. AI/ML vs. Admin/DataOps

## BI
Talking points center around:
- Data driven decision making
- Actionable insights
- Understanding why metrics are out- or under-performing

> G2 has the most succinct roundup of these that I have found: https://www.g2.com/categories/business-intelligence
> -- Jenny Kwan

These are all the same thing:
- When facing a decision I know I face, do I have the right data to make that decision?
- I want the data platform to tell me that decisions are even available to be made (i.e. are there decisions I don't know that I could be making?).
- The data platform and supporting processes/tooling should run continuously to monitor for these opportunities.
- Implicit in "do I have the data to make that decision" is the ability to research to identify a point of intervention, i.e. blame.
  > People think blame / root cause is the same as the best point of intervention...
  > -- Jenny Kwan

> The infographic embedded in this Inc.com article from 2014 (stale) is still a pretty good set of talking points. (The graphic is from the CIS department at Metropolitan College at Boston University.) https://drive.google.com/file/d/1esWFZpKeq5jtfknU5FpFQf5vnG85AQh-/view?usp=sharing
> - Jenny Kwan

### Stakeholders
- Board, C-level
  - Dashboards with KPIs.
    > Everyone wants to feel like a fighter pilot.
    > -- Jenny Kwan
- Functional leaders, i.e. head of marketing, sales, etc.
  - Same dashboards C-levels are holding them accountable to, of course.
    - Opportunity here is selling that the functional leader gets to define how they are measured.
  - Alerting and monitoring for points of management intervention
    - Stale leads/cases/tickets
    - Risk monitoring
      > Best practice is in conjunction with the actual IC doing the task so the IC isn't surprised when the manager swoops in. Oy, when the culture doesn't support this...
      > -- Jenny Kwan

#### Operations and Operational Platforms
i.e. Salesforce, any point-of-sale, etc.

> - I have never seen BI/dashboard rollouts geared towards operational IC staff to ever work in practice. In order for these not to be a PITA, the information has to be integrated into the operational platform so the IC isn't tab switching to get all of the information necessary to do their job.
> - NB: Expecting an IC to do this is adoption graveyard. They either hold info in their heads as they tab switch or have to write things down on paper? Come on!
> - NB: this is data! But no one who talks data includes integration with operational platforms, at least in press. Operational platforms control their own points of integration, so the product vendors take this over and then offer shitty, siloed data, and it's in their interest to pull every competing operational platform's data into their own proprietary operational platform.
> - NBB: Don't get me started on Salesforce / Force.com.
>
> -- Jenny Kwan

Helping clients see that centralizing their data into a single operational platform means losing total control:
- At the very least, the client takes on the product vendor's KPI definitions.
- Their dashboards work for ICs and tactical team leads, but the higher up you go, the less they fit.
- Their dashboards never work outside their function. A CRM platform's dashboard will never offer good integration with supply chain data.
- Later, even if they want to switch out their non-data operational workflows (redefine roles, teams, processes), the fact of data gravity means they're extra locked in.

## AI/ML
Two distinct sets of talking points emerge in press, often from the same people a few months apart (weird):
- AI/ML is now!
  > Impulse buy!
  > -- Jenny Kwan
- AI/ML isn't ready but prepare for it now b/c when the pistol fires...

Product vendors sort into the two based on what they sell:
- DS platforms and other tools: now!
- Centralized data management, especially governance: prepare!

Industry analysts are inconsistent. (It doesn't help that many of the articles are paid for by product vendors, and competitors go to the same analyst firms.)

A challenge for Qbiz is a coherent set of talking points that help clients integrate the conflicting press, and help stakeholders help other stakeholders that Qbiz doesn't have direct access to do the same.

One minefield to navigate is the tension between **pioneers** and **guardians** in this space. In blog space:
- DS professionals uniformly promote that AI/ML is now.
- Compliance and risk management folks, and to a lesser extent, IT, promote that AI/ML is prepare.
- This sorts by vertical too. Regulated verticals...

> I did not do a roundup saving blog articles. The points are consistent, though.
> -- Jenny Kwan

> The IT/CIO bias I've seen doesn't seem to have an easy answer... I don't see press touting, "hey, CIO, invest to get ahead of DS cowboys!" Perhaps b/c in most non-tech-centric verticals, IT is a cost control center.
> -- Jenny Kwan

### AI/ML is now!
Still rounding up.
> Obligatory joke about the 10th year in a row everyone claims "this is the year for AI/ML!"
> -- Jenny Kwan

### Prepare for AI/ML!

#### Gartner

Gartner offers a 5-segment "Foundational Components of AI and ML":
- Business Process Management
- Data Management
- Applied Use Cases
- Supporting Technologies
- Software / Systems Engineering

> The architect in me finds these 5 incoherent. Looking deeper, these are not "Foundational Components" but are simply stakeholder roles abstracted to make non-political.
> -- Jenny Kwan

Gartner again:
- Business Process Management: function heads, i.e. Sales, Marketing
- Supporting Technologies: things you buy from product vendors, may be in anyone's GL, so goes into the Miscellaneous bucket
- Software / Systems Engineering: IT/CIO
- Data Management:
  - Data excluding DS, typically means only DEs, whether in IT or not
  - Data governance / compliance / risk
- Applied Use Cases: DS

> Gartner's model doesn't translate to concrete initiatives / projects.
> -- Jenny Kwan

#### IDC

#### Forrester

# Admin/DataOps
Three sets of talking points entirely based on role in ecosystem:
- Data admin vendors, such as governance, lineage, security / access control, auditing
- IT/CIO/those who bear the cost center but not the revenue generating center
- Established data / BI vendors incrementally adding to their product portfolios

## NB: Master Data Management
**Master data** is coming up with a single version of truth for:
- Which entities (nouns) exist:
  - How many customers, prospects, products, transactions?
- For each entity (noun), what information about them (adjectives, history of verbs)?
  - This customer had exactly these transactions at these locations, etc.
  
This is closely related to "single-version of the truth" in BI. However, MDM is operationally focused, whereas BI is analytically focused. In practical terms:
- BI just cares about integrating across and keeping it in the warehouse/lake/analytical store
- MDM cares about integrating and pushing back out to operational systems like CRM, ERP, call center, POS; MDM may or may not store it in an analytical store

Obviously, the two go together. The talking points:
- BI product vendors talk about a freeform toolkit to bespoke integrate, with no talking points at all about Reverse ETL.
  - This is changing with the advent of Reverse ETL, which specializes in pushing back out
  - Auditing correctness of integration / ETL is also bespoke
- MDM biases toward tight integration with a subset of operational vendors, i.e. if you use both Salesforce and Oracle ERP, plug our product vendor offering in and sync is set up
  - Avoids talking about bespoke b/c bad at it
  - Very much about convenience
  - Touts audit trails to prove sync quality
  
One talking point is helping clients distinguish clearly between BI and MDM.
> Profisee (product vendor) has some excellent press on this:
> - https://drive.google.com/file/d/1dmXlF8OEtFf6cPRugMzQerm11x6lkkuv/view?usp=sharing
>
> Look at Collibra trying to persuade people to do data governance as part of MDM:
> - https://www.collibra.com/blog/mdm-needs-data-governance-heres
> - MDM has urgency because it helps clients reduce operational risk (getting a consumer's address wrong).
> -- Jenny Kwan

## Data admin vendors
Product vendors have carved up their feature space by purchaser stakeholder roles.
- Data governance: CIO, CCO, CRO
- Data lineage: BI / DE
- Data security / access control and data auditing: CISO, sometimes CRO

Efforts by these product vendors to brand to more than one target stakeholder persona seem to not work.

In reality, there's a lot of product function overlap between these separate product families.

> Need to think more about this in context of Qbiz.
> -- Jenny Kwan

## IT/CIO/Cost Center w/o Revenue
Data governance.
> Passive-aggressively bury innovation in paperwork.
>
> In all seriousness, this perspective is valid, but Qbiz navigating the conflicting motivations of pioneers vs. guardians is the key.
>
> -- Jenny Kwan

## Established BI vendors with add-ons
This is scattershot and needs to be by vendor.

> Much more work needed on a per-product level to have opinions on when to recommend the add-on and when to recommend a standalone.
> -- Jenny Kwan

One new, hot area is data lineage.

> The mathematician in me appreciates Delta Lake by Databricks. It's data lineage actually done right. (Well, 80% of the way; they stopped short because it would be impossible to adopt the final 20%.) Don't know if this correct math is actually usable by normal people in practice, though.
> -- Jenny Kwan
