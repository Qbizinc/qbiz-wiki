---
title: Data Architecture Cost Optimization Process
description: Process to periodically review data architecture costs and provide recommendations for cost optimization
published: true
date: 2023-05-16T19:21:51.614Z
tags: 
editor: markdown
dateCreated: 2023-05-16T00:20:47.434Z
---

# Data Architecture Cost Optimization Process

This wiki is meant to be a general cost optimization procedure that can be followed once new data architecture is stable.

The procedure is general enough that it should be able to work for any cloud provider. In general, the idea is to get to a point where cloud costs can be quickly examined and broken down across various groupings to determine where the best areas are for cost optimization.

In addition, other processes will be involved in order to fully optimize the cost of the client data architecture.

- Find the "Billing" section in the cloud provider UI
  - This should be part of all cloud providers and should contain helpful visualizations showing costs over time as well as the ability to filter data and group by specific dimensions
    - AWS's "Cost Explorer" tool is very useful for breaking down costs by Service, AWS account, etc. and even has some simple forecasts
  - Most cloud providers will provide recommendations in the Billing UI as well
    - Most recommendations tend to be volume based discounts like Committed Usage Discounts (GCP)/Savings Plans (AWS), which may not be an option for unpredictable workloads
- For experienced cloud users: Set up billing and/or monitoring if this hasn't been done already
  - For example: in GCP, every organization gets a billing export BigQuery dataset called a "billing sink" where all the costs for all GCP accounts linked to the organization get sent to
    - This data can be queried via SQL for more in depth insights
    - Various BigQuery to BigQuery pipelines can be created and then visualized in Data Studio with recurring reports that can be linked with specific filters in place
      - This enables the use of reports to consistently check cloud costs to catch sudden/gradual increases in costs
- Utilize application performance analysis data
  - This will be done in other document(s): INSERT LINK TO DOCUMENT(S) HERE
  - This step is crucial after the low hanging fruit has already been addressed (i.e. turning off unused services, volume based discounts, etc.)
  - In general, this section concerns itself with:
    - How is the application performing? Are there lots of unused resources, or is resource usage quite high?
    - If the workloads are unpredictable, is there an opportunity to leverage auto scaling?
    - What is the limiting resource (CPU, RAM, Disk, Iops, network bandwidth, etc.)?
    - For the resources that are not limiting, is there an opportunity to cut down on usage? 
      - Typically this will manifest itself in compute power (CPU/RAM); this is one of the easier resources to quickly spin up and down
      - Unless there is a ridiculous amount of disk provisioned but not utilized, it usually makes sense to just keep the disk provisioned and grow into it. Same applies to the Iops of a VM