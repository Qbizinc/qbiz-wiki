---
title:  Improving Dashboard Refresh Performance
description: 
published: true
date: 2021-07-30T00:32:28.194Z
tags: business-intelligence
editor: markdown
dateCreated: 2021-07-30T00:32:28.194Z
---

## The problem
When creating a new dashboard it is often a best-practice to create an extract for the underlying data in order to prevent re-calculation on every update of the dashboard UI. However, this presents its own unique challenges, the main one being: when to refresh the extract? You can typically solve this problem by setting a refresh frequency for your underlying SQL script and pick an interval of time that is off peak-hours.

Leaving your dashboard at this point may be sufficient if your underlying data is stagnant. However, if your data is growing incrementally all the time, then you will likely bump into the issue of your dashboard requiring more and more time to refresh the extract.  A temporary solution might be to reconfigure your refresh schedule, but you will eventually run into the same problem again.

## The solution

### Requirements
* Your underlying data must be able to be sorted somehow - preferably by some unique ID
* You'll need the access privileges of being able to configure your dashboard's settings
### Tableau - Configure Incremental Refresh
1. Select a data source on the Data menu and then select Extract Data.
1. In the Extract Data dialog box, select All rows as the number of Rows to extract. Incremental refresh can only be defined when you are extracting all rows in the database. You cannot increment a sample extract.
1. Select Incremental refresh and then specify a column in the database that will be used to identify new rows. For example, if you select a Date field, refreshing will add all rows whose date is after that last time you refreshed. Alternatively, you can use an ID column that increases as rows are added to the database.
1. When finished, click Extract.

[Reference](https://help.tableau.com/current/pro/desktop/en-us/extracting_refresh.htm)

### Looker - Create an Incremental PDT

[Reference](https://docs.looker.com/data-modeling/learning-lookml/incremental-pdts)
