---
title: Architecture Implementation Testing and Validation Process
description: Process for testing/validating that a data architecture was implemented successfully
published: true
date: 2023-05-17T17:03:52.917Z
tags: 
editor: markdown
dateCreated: 2023-05-15T23:04:43.963Z
---

# Architecture Implementation Testing and Validation Process

The purpose of this document is to lay out in detail a process for testing and validating that data architecture for a client was implemented successfully. 

The tests should all contribute to meeting each of the requirements the new architecture is expected to achieve (correct data types, correct data volumes, correct output data, etc.).

This process can be built using this template document as a starting point: https://docs.google.com/document/d/13mK6aW6BINitKzSKXtIbxIcaIvwT8HNGwpgJ5kPwTfs/edit?usp=sharing

Whatever the final chosen data architecture is for the client should be the output of this [template document](https://docs.google.com/document/d/1lb55oAfuF3pOVnWFM-WogGZajYh8RZKhOEI8ystyViI/edit?usp=sharing) (tailored for the client and project).

The document is not meant to be a catch all solution but instead an iterative document that can be tailored for the specific use case.

Some high level points to consider (with examples):
- Determine logistics of solution implementation
  - Is the implementation a migration? Is the implementation standalone?
  - Is there a specific migration plan that will be followed?
    - Is there a strict timeline/budget to follow for a migration?
    - If the migration requires code changes (likely), are there methods for reverting the PRs/redirecting traffic/etc?
- Sketch out a high level validation plan
  - What kinds of tests should be run in order to validate that the data architecture was successfully implemented?
  - Will there be major changes in functionality for end users?
  - Are there specific expectations that this new architecture is expected to meet? How will those be reflected in the tests?
- Solidify the validation plan with specific tests
  - What will the tests actually look like? What does the test design look like in a test environment?
  - How does each test contribute to showing that the architecture was implemented successsfully?
  - [Example document for Nexus project deliverables](https://docs.google.com/spreadsheets/d/1eXhVjY8yDmBXP8QzwQwR5shvkIossGvEjkg363DNtR0/edit?usp=sharing) that could be tailored to track testing/validation during solution implementation (tracking how much each line item is complete, who is responsible for testing, etc.)
- Describe infrastructure management moving forward
  - What sort of tools/services/etc. have been implemented to help with infrastructure management? 
    - Will these methods hold up in the event that the relationship between Qbiz and the client ends?
  - Have logging/monitoring/alerting tools/services been set up to help detect and alert the owning team to issues?
- List out recommendations for the client after the architecture has been setup (ideally upcoming/future projects)
  - Following project wrap up documents contain examples of recommendations after new architectures were implemented:
    - [QBiz - Flodesk Project Wrap-Up](https://docs.google.com/presentation/d/1IMu451fax7oK0V_mqFdRzPVrOIVlSNoWfST9PJF8hWE/edit?usp=sharing)
    - [Qbiz NerdWallet Looker Case Study](https://docs.google.com/document/d/1JETh2kC_mK-qSGzKMIgAS96YI4H4xMRZghw4_d-bh2g/edit?usp=sharing)
