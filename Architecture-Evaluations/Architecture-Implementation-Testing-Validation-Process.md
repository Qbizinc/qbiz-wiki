---
title: Architecture Implementation Testing and Validation Process
description: Process for testing/validating that a data architecture was implemented successfully
published: true
date: 2023-05-15T23:33:29.804Z
tags: 
editor: markdown
dateCreated: 2023-05-15T23:04:43.963Z
---

# Architecture Implementation Testing and Validation Process

Reference document: https://docs.google.com/document/d/13mK6aW6BINitKzSKXtIbxIcaIvwT8HNGwpgJ5kPwTfs/edit?usp=sharing

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
- 