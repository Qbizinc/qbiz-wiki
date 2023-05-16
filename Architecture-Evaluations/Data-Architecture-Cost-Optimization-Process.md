---
title: Data Architecture Cost Optimization PRocess
description: Process to periodically review data architecture costs and provide recommendations for cost optimization
published: true
date: 2023-05-16T00:32:58.203Z
tags: 
editor: markdown
dateCreated: 2023-05-16T00:20:47.434Z
---

# Data Architecture Cost Optimization Process

This wiki is meant to be a general cost optimization procedure that can be followed once new data architecture is stable.

The procedure is general enough that it should be able to work for any cloud provider.

- Find the "Billing" section in the cloud provider UI
  - This should be part of all cloud providers and should contain helpful visualizations showing costs over time as well as the ability to filter data and group by specific dimensions
  - Most cloud providers will provide recommendations in the Billing UI as well
    - Most recommendations tend to be volume based discounts like Committed Usage Discounts (GCP)/Savings Plans (AWS) 
- For experienced cloud users: Set up billing and/or monitoring if this hasn't been done already
  - For example: in GCP, every organization gets a billing export BigQuery dataset called a "billing sink" where all the costs for all GCP accounts linked to the organization get sent to
    - This data can be queried via SQL for more in depth insights
    - Various BigQuery to BigQuery pipelines can be created and then visualized in Data Studio with recurring reports that can be linked with specific filters in place
      - This enables the use of reports to consistently check cloud costs to catch sudden/gradual increases in costs