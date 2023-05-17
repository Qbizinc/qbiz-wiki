---
title: Data Architecture Cost/Application Optimization Process
description: Process to periodically review data architecture costs and provide recommendations for cost optimization as well as perform testing to determine how data applications can be optimizerd
published: true
date: 2023-05-17T21:42:21.263Z
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

Thus, the goal should not be solely to minimize costs and/or maximize performance, but instead find the right balance of cost and performance that keeps costs down while also allowing for the application to be able to scale to handle unexpected events like spikes in traffic without a degradation in performance (within reason). 

Acknowledging the nonzero time/energy cost of engineers having to provision and allocate additional resources should also be taken into account. The more overhead an application has, the less time engineers have to spend provisioning additional resources for it and vice versa. Determining exact overheads will be a process of trial and error, and can be further informed by a regular performance testing process.

A template performance testing document including example reports, tools and sources used, and metrics gathered can be found here: 

INSERT LINK

### Monitoring Systems and Tools

In order to do effective performance testing, monitoring systems and tools in the cloud should be leveraged. 

Most cloud providers will have monitoring/logging systems that can export performance related data such as CPU/RAM usage to systems that can then be analyzed/visualized/etc. (i.e. Cloudwatch metrics in AWS Stackdriver, logging metrics in GCP, etc.). Other solutions can be used such as exporting metrics to a time series based architecture like Prometheus. These metrics should then be visualized in a way that is easily accessible for the owning team and allows for quick decision making based on the metrics.

In addition, alerts should be set up that look at these resource usage metrics that are configured to alert the owning team if they reach a selected threshold (i.e. 70% CPU usage on any VM).

Either way, a standardized solution for tracking resource usage metrics should be followed and checked on at a regular cadence in order for the data architecture to be its most effective.

#### Technology Demonstration

INSERT


## Application Optimization

Application optimization is a natural next step after performance testing. This includes use cases such as improving code performance efficiency, optimizing deployment on the cloud, and more. 

Some examples where these may be warranted:
- An application is found to perform all of its operations using multiple servers in parallel and is woefully underutilizing the provisioned CPU on each machine
  - Multiple options could be considered here (both of which would also optimize cost)
    - Decreasing CPU of each machine
      - This is the simpler option overall but would still be more complex to operate moving forward
    - Rearchitecting the application to perform all operations in parallel on the same machine
      - The additional benefit of this option is that the application would then become simpler to operate moving forward
- An application is found to query the database many times for the same set of data in order to perform its work
  - A natural solution for this is to implement some sort of caching layer that the application can easily access
    - This solution **may** also cost more, but at scale should optimize cost as well
    - Even at a smaller scale (and most likely increased cost), this would allow the application to operate much more efficiently and would very likely pay off in the long run (depending on future growth)
      - There are also plenty of lightweight caching solutions (i.e. Memcached) that can be chosen if only simple data is needed
- The application backend database currently runs directly on VMs but the owning team has been unable to keep up with system updates, software patches, downstream impact of feature releases, etc.
  - This is pushing the owning team to the limit by constantly having to respond to incidents and forcing them to depriorize certain initiatives
  - In order to alleviate the owning team of managing the database themselves, the database is migrated to use a "managed" solution (i.e. AWS RDS)
    - This will increase cloud costs, but this will decrease "management" costs of the database, allowing the team to focus on higher priority/value objectives
    - This is yet another example of having to balance the needs of application optimization with managing costs; the key is to be able to justify the increase in cost (in this case, alleviating the owning team so they can work on other things)

A template application optimization document containing evidence of performance analysis and data on iterative improvement of application performance and examples of implementation for the customer can be found here:

INSERT LINK

## Cost Optimization

Cost optimization, while not as directly related to performance testing as application optimization, is crucial for being able to manage a well architected application and still is usually intertwined with performance optimization.

In addition to everything that's been mentioned to this point, here are some helpful tips for managing cloud costs:
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
- Right size data architecture by utilizing application performance analysis data
  - This step is crucial after other low hanging fruit has already been addressed (i.e. turning off unused services, volume based discounts, etc.)
  - **NOTE: this step should be done only after application optimization has been done to ensure the application will continue to operate effectively after being right sized**
  - In general, this section concerns itself with "right sizing" the application by trying to answer the following:
    - How is the application performing? Are there lots of unused resources, or is resource usage quite high?
    - If the workloads are unpredictable, is there an opportunity to leverage auto scaling rather than always being overprovisioned during non peak times?
    - What is the limiting resource (CPU, RAM, Disk, Iops, network bandwidth, etc.)?
    - For the resources that are not limiting, is there an opportunity to cut down on usage? 
      - Typically this will manifest itself in compute power (CPU/RAM); this is one of the easier resources to quickly spin up and down
      - Unless there is a ridiculous amount of disk provisioned but not utilized, it usually makes sense to just keep the disk provisioned and grow into it (assuming data is kept and not deleted after a certain period)
        - Same applies to the Iops of a VM
  - Examples of rightsizing an application
    - When the application was first launched, extra resources were provisioned in case of more extreme unexpected events; however, after the application has existed at a steady state it is determined that none of the extra resources provisioned were needed
      - The owning team takes back the extra resources while also leaving some overhead for the application to naturally grow into
    - The application currently runs on "custom" VMs (custom CPU/RAM specifications) but does not fully utilize the resources on them
      - Running on "standard" VMs (predefined ratios of CPU to RAM) is cheaper; an analysis shows that the application can be moved to standard VMs with minimal performance impact, achieving cost optimization in the process
- Consider application rearchitecting
  - **In terms of cost optimization**, this is usually one of the last options considered, as it typically requires the most amount of upfront engineering work and isn't always justified by cost savings
  - However, depending on the use case it may make sense to consider. For example:
    - Upon examination of billing data, it becomes clear that network egress costs seem much higher than they were projected
      - Looking closer, it is determined that this is being caused by one service making alot of data transfer requests, often times with data that is not crucial for the next step in the process
      - The application gets rearchitected to make less calls for the redundant data (perhaps leveraging a cache) as well as filtering out unneeded data and reduce networking costs