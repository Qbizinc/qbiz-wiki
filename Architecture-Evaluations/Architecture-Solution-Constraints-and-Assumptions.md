---
title: Architecture Solution Constraints and Assumptions
description: Guide for assessing the constraints and assumptions of potential architecture solutions
published: true
date: 2023-05-01T23:55:32.261Z
tags: 
editor: markdown
dateCreated: 2023-04-28T23:02:08.075Z
---

# Architecture Solution Constraints and Assumptions Guide

This guide provides a step by step process for assessing client infrastructure and needs to capture all the requirements and constraints of a new cloud based software architecture.

### Assessing architecture solution for client

Please follow the process here to assess (1) the current state of the client architecture, (2) the needs of the customers of said architecture, (3) the desired end state architecture, and various aspects of the architecture here:

https://docs.google.com/document/d/1RXugNuzgyTjuqb50wvKnwd2hgFR4SaIPiGrHEqC4p94/edit?usp=sharing

After this is done, proceed to the following sections.

### Assumptions of the solution

- What are the assumptions of the solution architecture?
  - Are there any significant assumptions, like requests per second per server? IOPs per request?
- What workloads should this architecture *theoretically* be able to handle?
  - Building off this, what is the limiting factor of this architecture? CPU? Network bandwidth?
- Does this architecture have high availability? Utilize DB backups? Utilize multi region? Or are any/all of these options eschewed for cost savings in anticipation of not needing them?

### Potential constraints of the solution

- What are the constraints for the solution architecture? Will it be able to handle future growth?
- Is there a specific budget and/or timeframe that needs to be adhered to? 
- Are there complicated legacy systems that need to be migrated to the cloud/connected to the cloud? 
- Is the team managing this infrastructure properly staffed and/or trained to maintain this architecture?
- Were there "nice to have" infrastucture pieces that were deprioritized due to constraints like cost?
- Are there specific bottlenecks to this architecture that will need to be closely monitored?
  - I.e. Is the limiting factor CPU or IOPs? Network bandwith or number of concurrent queries? Etc.
- Will this architecture be provisioned automatically via tools like Terraform, or will it be provisioned manually?
- Does this architecture employ continuous integration/deployment (CI/CD), or do code changes need to be deployed manually?
- Does this architecture lack high availability (i.e. multi-zone/AZ or multi-region deployment)?
