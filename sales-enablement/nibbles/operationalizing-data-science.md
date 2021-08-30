---
title: Operationalizing Data Science
description: 
published: true
date: 2021-08-30T17:45:48.515Z
tags: sales, sales-support, pre-sales, data-science, data-engineering
editor: markdown
dateCreated: 2021-08-19T18:27:03.419Z
---

# Operationalizing Data Science
> Work in progress
{.is-warning}


## The Data Science Assembly Line
Roadblocks to operationalizing Data Science are typically experienced by business stakeholders as lack of throughput. Data Scientists may have been hired and are doing good work, but the results are non-existent or take a very long time to materialize.

Depending on the maturity and culture of a business's organization, bottlenecks can occur at one or more of the following stages in the Data Science assembly line.
  
##### Jay
- need to flesh out the cross-functional aspect of the assembly line -- qbiz uniquely positioned to make the assembly line run smoothly because we can wear many different hats
- technologies: not a lot to say here. We could mention introducing tools like JIRA and Confluence to enable cross-functionality. We are not project managers but we can wear the shoes for a time as the organization ramps up, and we can lay the groundwork for effective project management

##### Jenny
- Initial discovery
  - Talking points:
    - Data scientists are not delivering.
      - Qbiz defines this as "inadequate data science throughput".
    - A company cannot measure or manage its data science throughput without a data science assembly line.
    - Data scientists' core function is "building and validating models".
      - Building and validating models is in the middle of the assembly line.
    - Data scientists say they're spending all of their time not doing data science, instead doing EDA, munging, wrangling.
      - Qbiz names this as "data scientists are spending all of their time getting their data to be 'manufacturing-ready'".
      - This is not their core function; that's why they're less effective at it.
      - Data engineers are best at this.
    - Data science assembly lines:
      1. Produce "manufacturing-ready" data
      2. Build and validate models
      3. Deliver model outputs to decision actors
      4. Feedback on model performance
      5. Making sure models are predictably up-to-date
      6. Real-time building/training of models
      7. &lt;Need to rethink 4,5,6 as maturity, not as separate steps.&gt;
    - The data science assembly line is inherently cross-functional, including IT, data engineers and architects, and even software engineers not in data.
      - Qbiz understands all of these perspectives and how to translate between them.
  - Leading questions:
    - What has your ROI on data science been?
    - If your data scientists aren't delivering, what reasons do they give?
    - Do your data scientists name any specific functions or teams as roadblocks?

## 1. Enabling Data Access (EDA)

> How much time are your Data Scientists spending actually doing Data Science vs data-wrangling and cleanup?"

**Problem**: Data Scientists typically pull data from data lakes or data warehouses, both of which are ill-suited for training models.


**How Qbiz can help**: [The Feature Store](/sales-enablement/nibbles/sales-enablement/ds-feature-store)

##### Jay
- Data is most likely being pulled from a database such as a Redshift or Snowflake. It could also be pulled from a data lake (e.g., S3). We can analyze data models, query performance, discover strategies for optimizations. We can deliver a curated set of data that's optimized for DS model building by working with IT to set up appropriate cloud infrastucture (e.g. AWS Glue, Step Functions, Batch, ECS -- also orchestration tools like Apache Airflow)

##### Jenny
- Talking points:
  - This step is "Produce manufacturing-ready data."
  - Building models requires as input, individual datasets that are shaped a certain way.
    - Data engineers are best suited to put these datasets together.
    - Qbiz recommends a feature-store as machinery to put together these datasets.
  - The most mature vision of the feature-store allows a data scientist to click a few buttons and receive the correct, customized dataset for the desired model type and parameters.
    - A general-purpose product for this does not exist from any product vendor.
    - Qbiz can help companies deliver the machinery for "customized datasets on demand" with the right balance of manual/automated for the maturity level of the company.
    - This is NOT a data lake or a data warehouse.
      - Data lakes capture raw data not in the shape required for building models.
        - Models require a "granularity of action"; raw data is usually not at that granularity.
        - At the specific granularity of action, computing specific features is complex.
      - Data warehouses are for dashboards and are relatively stable.
        - The feature store is for on-demand requests that are complex.
          - The feature store is not stable.
          
##### David E
- Development
  - For exploratory analyses, DSs like to be able to quickly get access to all data. Wide, easily-joinable tables make this easy and fast. 
  - Aggregate tables for commong aggregations speed up analyses and optimize resources. 
  - Feature stores (curated datasets) can fast-track work. But "quality" data may depend on the specific business objective, target metric, model choice, etc. 
- Production 
  - Data for models in production has a different set of requirements (speed etc). 

## 2. Doing Data Science

At this stage, Data Scientists have access to the data features they need and are building and batch training models

> Are your scientists writing code with an eye toward operationalizing it? Are they spending a lot of time wrestling with the nuts and bolts of coding? Are they able to collaborate in code effectively? Do they have the right tools?

**Problem**: Data Scientists are able to build/train their models are likely working in isolation and writing code that others can't easily understand or adapt. They may be working with their tools inefficiently as well as spending too much time worrying about how to connect to data sources, how to write data out, etc.

**How Qbiz can help**: We can introduce tools and coding best practices (including reusable libraries, unit tests, and scalability tests) to streamline collaboration and make the path to production smoother


#### missing?
* what is to sell data science tooling? what's required?

##### Jay
- We can help spin up Sagemaker instances, iPython notebooks, make them reliable and reusable. We can recommend tools such as VSCode and Pycharm and educate on how to best use them. We can migrate existing code to github and educate on collaborative coding. Tools such as black, pylint, and pytest can introduce discipline and stylistic conformity that make collaboration smoother

##### Jenny
- Talking points:
  - This step is "Building and validating models."
  - Building is time consuming without the right tools.
    - Infrastructure, which requires DevOps.
    - Code standards and libraries, which requires software engineering expertise and curation.
  - Validating in data science culture is academic peer review.
    - The process must be repeatable, which requires tools and coding standards.
    - The process must be replicable by another data scientist on a different environment (another machine).
    - Much of academic peer review is presentation and defense, and tools exist to support this process.

##### David E
- Educating DSs sounds difficult; like herding cats. Especially if individual DS does not want to be educated. 
- Putting standards and best practices into place and supporting their use with documentation is helpful. 
- Developing models in a way that makes them easily deployable (if they will be deployed), and captures data on various model runs, performance, etc. 


## 3. Delivering models outputs to decision actors (machine actor, human actor)
> Are production systems consuming the output of your models? If so, are you able to iterate on changes to your models quickly enough to satisfy business stakeholders?

**Problem**: Data Science often faces roadblocks from IT and operations because Data Science's needs are poorly understood, systems and processes are optimized for stability, and Data Scientists lack the vocabulary to articulate their needs to IT

**How Qbiz can help**: Qbiz Data Engineers are fluent in the languages and cultures of both Data Science and IT/Operations and can therefore bridge that gap. We are able translate Data Science needs into the language, processes, and infrastructure that IT requires.

##### Jay
- We can work with the appopriate stakeholders to deliver data that the production application can consume easily and with minimal risk -- via S3, via messaging services such as SNS/SQS, RESTful API endpoints, database tables that are consumed by Tableau/etc. We know the technologies involved, understand the stakes, and can work with DS and IT stakeholders to find the optimal balance between timely data availibilty and securyt/safety.

##### Jenny
- Talking points:
  - Decision actors typically fall into:
    - Strategic
    - Tactical
    - Operational
  - Strategic is normally a presentation.
  - Tactical is normally a dashboard.
    - Requires integration back to data warehouse and BI/dashboarding.
  - Operational can be human or machine.
    - Requires delivery to human point-of-operation.
      - Sales, marketing, call center...
    - Or requires actual integration with operational system where system calls on model for a decision.
      - Complicated and requires interacting with software engineers not-in-data.


## 4. Closing the model feedback loop

> Once your models are deployed, do you have a reliable and effecient means of adjusting them?

**Problem**: Once models are deployed, it can be difficult to adjust them in production quickly enough to ensure continued performance

**How Qbiz can help**: We will work with IT stakeholders to develop safe and reliable mechanisms that enable quick iteration and performance tuning

> continuous training and deploy of models

##### Jenny
- Talking points:
  - This step is "Feedback on model performance."
  - Feedback depends on audience.
  - Strategic: manual.
  - Tactical: capture human-expert-feedback-or-decisions (i.e. overrides) back to data scientists.
  - Operational:
    - Human: same as tactical.
    - Machine:
      - At a minimum, capture the full outcomes to deliver back for supervised-learning (building/training).
        - Lots of software-engineer-not-in-data plus data engineer work.
      - Better, enable proper statistically-valid, A/B testing.
        - Very, much, lots of software-engineer-not-in-data work.
        - Requires data scientist expertise translated to those software-engineers-not-in-data.

##### David E
- Model performance should be captured over time. (Accurarcy / whatever metric(s) of interest, availability, # errors, ...)
- Can models be adjusted / retrained quickly and easily? Can this be automated? 

## 5. Realtime model feedback

> Do you have tooling in place to monitor model performance in realtime or near-realtime?

**Problem**: Model performance is typically measured via batch adhoc queries and analyses. This can result in issues not being detected quickly enough to course correct in a timely way

**How Qbiz can help**: We can work with Data Science and IT to establish processes to provide continuous monitoring of model performance, appropraite alerting, and visualizations.

##### Jenny
- Talking points:
  - This step is "Real-time building/training of models."
  - Very rarely needed.
  - For these use cases, most data science model types cannot be used. (None of Qbiz's business; data scientists know this.)
  - If needed and possible, then massive infrastructure implications.
  - Blended real-time and batch ETL is immensely difficult.
  - Feature-store in blended real-time and batch ETL is immensely difficult.
  - Very few engineers of any kind have experience with the products in this space.
    - Kappa architecture: Druid is an open source instance.
    - Pure streaming: Nifi, etc., very difficult to get the data right, because testing is extremely difficult.
  - Qbiz has expertise.

## Notes 2021-08-30
- "Rework tax"
- Job title variance
- MDM
- Strategic models not for prediction - looking for strong correlation
- Model ops
