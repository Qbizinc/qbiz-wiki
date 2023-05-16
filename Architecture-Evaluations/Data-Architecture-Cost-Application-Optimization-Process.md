---
title: Data Architecture Cost/Application Optimization Process
description: Process to periodically review data architecture costs and provide recommendations for cost optimization as well as perform testing to determine how data applications can be optimizerd
published: true
date: 2023-05-16T23:31:02.867Z
tags: 
editor: markdown
dateCreated: 2023-05-16T00:20:47.434Z
---

# Data Architecture Cost and Application Optimization Process

This wiki is meant to be a general cost/application optimization procedure that can be followed once new data architecture is stable.

The procedure is general enough that it should be able to work for any cloud provider. In general, the idea is to get to a point where cloud costs can be quickly examined and broken down across various groupings to determine where the best areas are for cost optimization. There should also be a standardized, reliable process to follow to performance test data applications and potentially take action based on the results.

In addition, other processes will be involved in order to fully optimize the cost and application performance of the client data architecture.

## Performance Testing

Regularly testing applications for performance is crucial for **BOTH** cost and application optimization. 

Typically optimizing for performance will also help with optimizing cost and vice versa; however there may be cases where one is sacrificed for the other. Some examples:
- Always provisioning additional compute resources in a cluster as "overhead" that services can utilize to account for unexpected increases in usage
- Overprovisioning SSD that may not be used for months but allows for a database to grow unencumbered, etc. 

For example, if an application is provisioned such that the compute is being heavily utilized, this could be considered cost optimized but vulnerable to a spike in traffic or other unexpected events. Conversely, an application could be overprovisioned such that actual usage of compute resources is low; this architecture would certainly have good performance but would NOT be cost optimized.

Thus, the goal should not be solely to minimize costs and/or maximize performance, but instead find the right balance of cost and performance that keeps costs down while also allowing for the application to be able to scale to handle unexpected events like spikes in traffic without a degredation in performance (within reason). 

Acknowledging the nonzero time/energy cost of engineers having to provision and allocate additional resources should also be taken into account. The more overhead an application has, the less time engineers have to spend provisioning additional resources for it and vice versa. Determining exact overheads will be a process of trial and error, and can be further informed by a regular performance testing process.

A template performance testing document including example reports, tools and sources used, and metrics gathered can be found here: 

INSERT LINK

### Monitoring Systems and Tools

In order to do effective performance testing, monitoring systems and tools in the cloud should be leveraged. This includes

.
.
.

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

FILL IN

A template application optimization document containing evidence of performance analysis and data on iterative improvement of application performance (for example, code performance efficiency, optimized deployment on Google Cloud resources) and examples of implementation for the customer can be found here:

INSERT LINK