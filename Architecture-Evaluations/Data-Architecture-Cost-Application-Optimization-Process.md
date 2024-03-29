---
title: Data Architecture Cost/Application Optimization Process
description: Process to periodically review data architecture costs and provide recommendations for cost optimization as well as perform testing to determine how data applications can be optimizerd
published: true
date: 2023-05-19T23:39:51.570Z
tags: 
editor: markdown
dateCreated: 2023-05-16T00:20:47.434Z
---

# Data Architecture Cost and Application Optimization Process

This wiki is meant to be a general cost/application optimization procedure that can be followed once new data architecture is stable.

The procedure is general enough that it should be able to work for any cloud provider. Aftetr this procedure is followed there should be:
- A standardized, reliable process to set up performance testing capabilities for client data applications
- A standardized, reliable process to set up monitoring for application health, resource usage, etc. which can also be used to alert the owning team when certain thresholds are breached
- A standardized, reliable process to leverage the results of performance testing to determine recommendations for optimizing the application (rearchitecting, replatforming, etc.)
- A standardized, reliable process to set up tools and services such that cloud costs can be quickly examined and broken down across various groupings and combined with performance testing results to determine the best areas for cost optimization

## Performance Testing

Regularly testing applications for performance is crucial for **BOTH** cost and application optimization. 

Typically optimizing for performance will also help with optimizing cost and vice versa; however there may be cases where one is sacrificed for the other. Some examples:
- Always provisioning additional compute resources in a cluster as "overhead" that services can utilize to account for unexpected increases in usage
- Overprovisioning SSD that may not be used for months but allows for a database to grow unencumbered, etc. 

Specially, if an application is provisioned such that the compute is being heavily utilized, this could be considered cost optimized but vulnerable to a spike in traffic or other unexpected events. Conversely, an application could be overprovisioned such that actual usage of compute resources is low; this architecture could certainly have good performance (assuming the application is optimized) but would NOT be cost optimized.

Thus, the goal should not be solely to minimize costs and/or maximize performance, but instead find the right balance of cost and performance that keeps costs down while also allowing for the application to be able to scale to handle unexpected events like reasonable spikes in traffic without a degradation in performance.

Acknowledging the nonzero time/energy cost of engineers having to provision and allocate additional resources should also be taken into account. The more overhead an application has, the less time engineers have to spend provisioning additional resources for it and vice versa. Determining exact overheads will be a process of trial and error, and can be further informed by a regular performance testing process. Looking into implementing cloud features such as auto scaling is also a useful exercise here.

A template performance testing document including example reports, tools and sources used, and metrics gathered can be found here: 

https://docs.google.com/document/d/1k3gKGt6l4X3RkZEyIvH7xxohveMu5qZIy6xfnx2ohdo/edit#heading=h.hp39q3gd1kg3

## Monitoring Systems and Tools

Proper monitoring systems and tools in the cloud are also crucial for **BOTH** cost and application optimization to (1) understand where there are opportunities to reduce cost (i.e. underutilized resources) as well as (2) understand the potential opportunities to optimize the application (i.e. unusually high resource usage but low user traffic). Proper monitoring can be seen as complimentary to the performance analysis process, as well as the missing link in the architecture management process to be able to react to unexpected changes that require the owning team to take action (i.e. spikes in traffic).

Most cloud providers will have monitoring/logging systems that can export performance related data such as CPU/RAM usage to systems that can then be analyzed/visualized/etc. (i.e. Cloudwatch metrics in AWS Stackdriver, logging metrics in GCP, etc.). Other solutions can be used such as exporting metrics to a time series based architecture like Prometheus. These metrics should then be visualized in a way that is easily accessible for the owning team and allows for quick decision making based on the metrics.

In addition, alerts should be set up that look at these resource usage metrics that are configured to alert the owning team if they reach a selected threshold (i.e. 70% CPU usage on any VM).

Either way, a standardized solution for tracking resource usage metrics should be followed and checked on at a regular cadence in order for the data architecture to be its most effective.

In addition, GCP has an additional offering called ["Recommenders"](https://cloud.google.com/recommender/docs/recommenders) which should be enabled that can provide performance and cost based recommendations for various GCP products.

### Technology Demonstration

INSERT


## Application Optimization

Application optimization is a natural next step after performance testing. This includes use cases such as improving code performance efficiency, optimizing deployment on the cloud, and more. 

GCP's advice on optimizing workload performance: https://cloud.google.com/architecture/framework/performance-optimization

### Examples
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
- The application backend database currently runs directly on VMs but the owning team has been unable to keep up with system updates, software patches, impact of feature releases from upstream services, etc.
  - This is pushing the owning team to the limit by constantly having to respond to incidents and forcing them to depriorize certain initiatives
  - In order to alleviate the owning team of managing the database themselves, the database is migrated to use a "managed" solution (i.e. AWS RDS)
    - This will increase cloud costs, but this will decrease "management" costs of the database, allowing the team to focus on higher priority/value objectives
    - This is yet another example of having to balance the needs of application optimization with managing costs; the key is to be able to justify the increase in cost (in this case, alleviating the owning team so they can work on other things)
- The application workloads are unpredictable, leading to the application having to be overprovisioned during nonpeak times (which is most of the time)
  - If the application runs on VMs, auto scaling would be an excellent option to leverage here, optimizing both performance and cost at the same time
    - Auto scaling typically requires configuring a few parameters (which can be determined through performance testing, trial and error, etc.):
      - Minimum number of virtual instances
      - Desired number of virtual instances
      - Maximum number of virtual instances

### Template
A template application optimization document containing evidence of performance analysis and data on iterative improvement of application performance and examples of implementation for the customer can be found here:

https://docs.google.com/document/d/1k3gKGt6l4X3RkZEyIvH7xxohveMu5qZIy6xfnx2ohdo/edit#heading=h.hp39q3gd1kg3

## Cost Optimization

Cost optimization, while not as directly related to performance testing as application optimization, is crucial for being able to manage a well architected application and still is usually intertwined with performance optimization.

GCP has some tips for Managing Cloud Costs [here](https://cloud.google.com/blog/topics/cost-management/best-practices-for-optimizing-your-cloud-costs).

### Tools/Reporting Set Up
- Find the "Billing" section in the cloud provider UI
  - This should be part of all cloud providers and should contain helpful visualizations showing costs over time as well as the ability to filter data and group by specific dimensions
    - AWS's "Cost Explorer" tool is very useful for breaking down costs by Service, AWS account, etc. and even has some simple forecasts
  - Most cloud providers will provide recommendations in the Billing UI as well
    - Most recommendations tend to be volume based discounts like Committed Usage Discounts (GCP)/Savings Plans (AWS), which may not be a great option for unpredictable workloads
- For experienced cloud users: Set up billing and/or monitoring if this hasn't been done already
  - For example: in GCP, every organization gets a billing export BigQuery dataset called a "billing sink" where all the costs for all GCP accounts linked to the organization get sent to
    - This data can be queried via BigQuery SQL for more in depth insights
    - Various BigQuery to BigQuery pipelines can be created and then visualized in Data Studio with recurring reports that can be linked with specific filters in place
      - This enables the use of custom reports to consistently check cloud costs to catch sudden/gradual increases in costs
  - If in GCP, it is strongly encouraged to enable ["Recommenders"](https://cloud.google.com/recommender/docs/recommenders) to provide cost optimization recommendations
  
### Some Cost Optimization Options
- Turn off unused services
  - It is crucial to regularly keep stock of services that may no longer be needed and should be turned off
  - Ideally a Program Manager or someone else assists with this, but consultants should also offer recommendations if possible
- Purchase volume discounts
  - This typically only makes sense when the client has the scale to support it
  - These discounts can come in the form of 
    - Committing to a certain amount of compute **usage** over a 1 or 3 year period (i.e. GCP Committed Usage discounts, AWS Savings Plans); VMs do not have to be up continuously for this
      - GCP also offers Committed Usage discounts for certain disk like SSDs
    - Committing to a "reserved instance" for compute (AWS)
- Consider implementing data policies (data lifecycle, data retention, etc.)
  - Alot of times, data is kept by a company indefinitely even after it has served its purpose
    - If the data is in object storage, lifecycle policies can be implemented to the item holding the object (i.e. S3 bucket)
  - In addition, infrequently accessed data may be in a frequent access tier (i.e. in S3)
    - AWS S3 offers "intelligent tiering" where it automatically moves items that haven't been accessed in a user specified amount of time
  - In some cases, implementing these policies will involve application rearchitecturing; this will be addressed below
- Consider cheaper compute for short term workloads
  - AWS offers something called "EC2 Spot instances" which are VMs that can be purchases for much cheaper than regular VMs but can be interrupted unexpectedly
    - GCP has an equivalent offering called "Spot VMs"
  - Works best for short term workloads that can withstand interruptions and have lower resource demands
- "Right size" data architecture by utilizing application performance analysis data (specifically compute)
  - This step is crucial after other low hanging fruit (listed above) has already been addressed
  - **NOTE: this step should be done only after application optimization has been done to ensure the application will continue to operate effectively after being right sized**
  - In general, this section concerns itself with trying to answer the following:
    - What is the limiting resource (CPU, RAM, Disk, Iops, network bandwidth, etc.)? Put another way, at what resource usage does the application begin to experience performance degradation?
    - Are the non limiting resources being underutilized? Is there an opportunity to take back resources to save on costs?
      - Typically this will manifest itself in compute power (CPU/RAM); this is one of the easier resources to quickly spin up and down
  - Examples of rightsizing an application
    - When the application was first launched, extra resources were provisioned in case of more extreme unexpected events; however, after the application has existed at a steady state it is determined that none of the extra resources provisioned were needed
      - The owning team takes back the extra resources while also leaving some overhead for the application to naturally grow into
    - The application currently runs on "custom" VMs (custom CPU/RAM specifications) but does not fully utilize the resources on them
      - Running on "standard" VMs (predefined ratios of CPU to RAM) is cheaper
      - An analysis shows that the application can be moved to standard VMs with minimal performance impact, also achieving cost optimization in the process
- Consider application rearchitecting
  - **In terms of cost optimization**, this is usually one of the last options considered, as it typically requires the most amount of upfront engineering work and isn't always justified by cost savings
  - However, depending on the use case it may make sense to consider. For example:
    - Upon examination of billing data, it becomes clear that network egress costs seem much higher than they were projected
      - Looking closer, it is determined that this is being caused by one service making alot of data transfer requests, often times with data that is not crucial for the next step in the process
      - The application gets rearchitected to make less calls for the redundant data (perhaps leveraging a cache) as well as filtering out unneeded data and reduce networking costs
    - The client keeps all data in their databases indefinitely, and some of the more dense data (minutely, hourly, etc.) is beginning to have a huge footprint as well as cost
      - After discussion with the donwstream data consumers, 1-2 data types are chosen to have data retention policies implemented 
      - An analysis shows that in order to implement this data retention policy the application will require rearchitecting
        - This could be some backend logic that needs to be introduced or a separate process that goes and deletes all data that is older than a selected retention period