---
title: Pytest Plugin for Mocking and Seeding Upstream Tables
description: 
published: true
date: 2023-03-01T18:40:40.823Z
tags: bench, opensource, tools, python, pytest
editor: markdown
dateCreated: 2023-03-01T18:40:40.823Z
---

# Description
In order to effectively mock upstream tables when testing ETL pipelines it would be nice if you had a pytest fixture that could accept a DDL statement and some optional constraints, then reliably create and populate a mock version of table with test data for use in tests. This could theoretically be done with [Pytest plugins](https://docs.pytest.org/en/7.1.x/how-to/writing_plugins.html) and the [faker](https://semaphoreci.com/community/tutorials/generating-fake-data-for-python-unit-tests-with-faker) library. [Tonic.ai](http://tonic.ai/?)?

# Meta
**Advocate**: Billy
**Interval value**: High
**External value**: High
**Difficulty**: 4 - Multiple people and/or weeks

# Notes
