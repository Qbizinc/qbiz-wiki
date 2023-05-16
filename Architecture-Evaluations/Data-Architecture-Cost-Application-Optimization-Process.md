---
title: Data Architecture Cost/Application Optimization Process
description: Process to periodically review data architecture costs and provide recommendations for cost optimization as well as perform testing to determine how data applications can be optimizerd
published: true
date: 2023-05-16T23:07:10.458Z
tags: 
editor: markdown
dateCreated: 2023-05-16T00:20:47.434Z
---

# Data Architecture Cost and Application Optimization Process

This wiki is meant to be a general cost/application optimization procedure that can be followed once new data architecture is stable.

The procedure is general enough that it should be able to work for any cloud provider. In general, the idea is to get to a point where cloud costs can be quickly examined and broken down across various groupings to determine where the best areas are for cost optimization. There should also be a standardized, reliable process to follow to performance test data applications and potentially take action based on the results.

In addition, other processes will be involved in order to fully optimize the cost and application performance of the client data architecture.

## Performance Testing

Regularly testing applications for performance is crucial for **BOTH** cost and application optimization. Typically optimizing for performance will also help with optimizing cost and vice versa; however there may be cases where one has to be sacrificed for the other (i.e. always provisioning additional compute resources in a cluster as "overhead" to account for unexpected increases in usage, brand new workloads, etc.). 

The goal thus should not be to minimize costs and/or maximize performance, but instead find the right balance of cost and performance that keeps costs down while also allowing for 


## Cost Optimization
- Find the "Billing" section in the cloud provider UI
  - This should be part of all cloud providers and should contain helpful visualizations showing costs over time as well as the ability to filter data and group by specific dimensions
    - AWS's "Cost Explorer" tool is very useful for breaking down costs by Service, AWS account, etc. and even has some simple forecasts
  - Most cloud providers will provide recommendations in the Billing UI as well
    - Most recommendations tend to be volume based discounts like Committed Usage Discounts (GCP)/Savings Plans (AWS), which may not be an option for unpredictable workloads
- For experienced cloud users: Set up billing and/or monitoring if this hasn't been done already
  - For example: in GCP, every organization gets a billing export BigQuery dataset called a "billing sink" where all the costs for all GCP accounts linked to the organization get sent to
    - This data can be queried via SQL for more in depth insights
    - Various BigQuery to BigQuery pipelines can be created and then visualized in Data Studio with recurring reports that can be linked with specific filters in place
      - This enables the use of custom reports to consistently check cloud costs to catch sudden/gradual increases in costs
- Utilize application performance analysis data
  - Refer to "Performance Testing" section above
  - This step is crucial after the low hanging fruit has already been addressed (i.e. turning off unused services, volume based discounts, etc.)
  - In general, this section concerns itself with:
    - How is the application performing? Are there lots of unused resources, or is resource usage quite high?
    - If the workloads are unpredictable, is there an opportunity to leverage auto scaling?
    - What is the limiting resource (CPU, RAM, Disk, Iops, network bandwidth, etc.)?
    - For the resources that are not limiting, is there an opportunity to cut down on usage? 
      - Typically this will manifest itself in compute power (CPU/RAM); this is one of the easier resources to quickly spin up and down
      - Unless there is a ridiculous amount of disk provisioned but not utilized, it usually makes sense to just keep the disk provisioned and grow into it (assuming data is kept and not deleted after a certain period). Same applies to the Iops of a VM
- Consider application rearchitecting
  - **In terms of cost optimization**, this is usually one of the last options considered, as it typically requires the most amount of upfront engineering work and isn't always justified by cost savings
  - However, depending on the use case it may make sense to consider. For example:
    - Upon examination of billing data, it becomes clear that network egress costs seem much higher than they were projected. Looking closer, it is determined that this is being caused by one service making alot of data transfer requests, often times for data that has already been requested. The application gets rearchitected to make less calls for the redundant data (perhaps leveraging a cache) and reduce networking costs
    - An application is found to perform all of its operations using multiple virtual machines in parallel and is woefully underutilizing the provisioned CPU on each machine. Multiple options could be considered here: decreasing CPU of each machine, or rearchitecting the application to perform all operations in parallel on the same machine. The additional benefit of the second option is that the application would then become simpler to operate moving forward.


## Application Optimization