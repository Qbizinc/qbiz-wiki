---
title: dbt certifications
description: 
published: true
date: 2025-07-30T21:44:31.492Z
tags: 
editor: markdown
dateCreated: 2025-05-23T01:49:36.130Z
---

# dbt Certifications
https://www.getdbt.com/dbt-certification

- dbt Analytics Engineering Certification Exam
- dbt Cloud Architect Certification Exam
- discount codes: `5YDYM`, `WWFYJ`, we likely have  vouchers to cover the entire exam through the partner program (ask Mayra)

> When updating notes, please specify who and give rough exam time for those who may have questions
{.is-success}



## dbt Analytics Engineering Certification
aka dbt Certified Developer

The general consensus is that this exam is pretty challenging. It is very syntax heavy / requires a solid understanding the imapct of different combinations of configs, flags, and environment variables. 


### Study materials: 

- [exam notes](https://docs.google.com/spreadsheets/d/1j9QQJ2DAgz_ej4RhgzHWlmbNtcUom9VF0Ltz93KXqyE/) created by David (certified September 2024)

- dbt's official certification learning path and exam guide from their website 

### 3rd party materials 

As of today (May 2025) dbt does not provide any practice exam - there are sample questions in the learning path and 5 sample questions in the exam guide. Some third party materials with assessment of helpfulness.

- https://www.qanalabs.com/courses/dbt-developer - found this free resource - tons of practice questions. This was the best 3rd party resource I found. Thought it was pretty good prep but not sure how maintained it is (some newer topics might not be covered here).

- Udemy course: as of May 2025 this is outdated and questions are MUCH simpler than actual exam. I'd skip this one (unless updated / low reviews have been addressed).

- [Quizlet](https://quizlet.com/489070274/dbt-certification-practice-exam-questions-flash-cards/?i=6lewqw&x=1jqt) - Actually found this one after taking the exam 

### Natalia's post-exam notes (May 2025): 
This was a tough one. 
Some random material / questions I remember showing up on the test...

- Use of [doc blocks](https://docs.getdbt.com/docs/build/documentation#using-docs-blocks): Jinja syntax (specifically block naming), where to put them (in `.md` file, in any resource path directory), and how to reference the block (does it have to be in the same directory as the resource referencing it?).

- `dbt_project.yml`
	- which of the given configurations are valid to set globally for your models (options: tags, target, schema, materialized, tests)?
  - what each of these keys mean: `config-version`, `version`, `require-dbt-version` (question asked how to ensure a specific version got installed. There were choices included that weren't valid keys i.e. `dbt-version` - so make sure to know these exactly)

	- specific question about what directory path they get installed to (by default, and how to change in dbt_project.yml file)
  
- artifacts from runs 
  - `manifest.json`, `catalog.json`, `run_results.json`, `sources.json`
  - where they are generated / when / what commands cause them to be generated
  - SQL in `target/run/` vs. `target/compiled/` (which one/both/neither: have fully resolved jinja, contain DML/DDL statements, preserve comments)

- error troubleshooting

- threads / what happens when 2 models are running in parallel and one fails  

- [indirect-selection](https://docs.getdbt.com/reference/global-configs/indirect-selection) flag for tests 
 - default behavior

- exposures 

- materialization - best materialization by use case

- dbt retry (what does/doesn't get rebuilt)

- development workflows (general git branching/PR creation, environments+schema mapping, CICD - questions about using state_modeified flag in this step when a PR is created)
  
- [node selection](https://docs.getdbt.com/reference/node-selection/syntax) (complex scenarios to get sets of nodes) 
    - `--select` vs. `--selector` flag (and `.yml` config for using selector)
    - unions and intersections (space, comma)
    -  `@` and `+` for parent/children selection
    
- nuances of dbt clone vs. defer (similar result but when would one be chosen over the other? a few which/both/none questions on these)
	- uses manifests
  - ensures no data warehouse credits are used
  

- grants 
	- [config inheritience](https://docs.getdbt.com/reference/resource-configs/grants#grant-config-inheritance)

- groups/access modifiers
  - default behavior 
  - restrict access to models defined in a package (see [docs](https://docs.getdbt.com/docs/mesh/govern/model-access#how-do-i-restrict-access-to-models-defined-in-a-package))
  
  
- state
	- passing path to manifest directory using `--state` flag
  - selecting specific state i.e. `--select state:modified --state path/to/previous/artifacts` / `--select +state:modified+ --state path/to/previous/artifacts`

- result selector (i.e. `--result:failed` - what are the possible values here)

- incremental mode 
	- ways to configure this for a model 
  - when [is_incremental() macro](https://docs.getdbt.com/docs/build/incremental-models#understand-the-is_incremental-macro) evaluates to true
  - what happens when an incremental model is built for the first time?
  - using defer / clone in development to avoid expensive initial build when doing local testing
  
  
- python models and their limitations 
	- can they reference/depend on SQL models? / can you use the `ref()` macro here? and vice-versa - can SQL models depend/reference python models?
  
	- what materialization options are available (table? ephemeral? view?)
  
## dbt Architect Certification

Natalia's post-exam notes (July 2025). 

### Note regarding exam timing
> A lot of features released just before taking the exam as guide/exam may have been updated since. ALSO, note dates of the dbt-provided training content.
{.is-warning}

Limitations / best practices discussed in 2023, for example, likely have changed. Docs are generally up to date but double check these things when reviewing video content and blog posts (even if linked in the exam guide).

Most notably:
- Environment scope added to RBAC (in addition to project scope)

- `defer` functionality within job settings (`Compare changes against` drop down): previously chose a specific job to compare artifacts against. Now, you chose a specific Environment or "this job" for self deferral.

- Some training content doesn't treat connections as resuable / refers to connection settings at the project level. 

- Renaming 
 	- pricing tiers: i.e. Teams is now Starter. Though, I got no questions about this - exam seemed to test from the perspective of the highest tier. 
  
  - New Analyst license type (though, again, not tested for me)
  
  - general rebranding from "dbt Cloud" to just "dbt" can make comparisons to dbt core a little confusing
  
  - dbt Explorer is now dbt Catalog. Cloud IDE is now Studio IDE. 

- SCIM support is available, contrary to some training materials referring to it as not supported 

- Some training materials covering how to set up CI/CD jobs are out dated since Merge Jobs and CI Jobs were introduced as job types, streamlining this process

### Supplemental study materials 
looking up things after the exam, I found some answers in the FAQ sections
- [data mesh FAQ](https://docs.getdbt.com/best-practices/how-we-mesh/mesh-5-faqs#how-dbt-mesh-works) 

- [Source freshness](https://docs.getdbt.com/docs/deploy/source-freshness) - asked about this 

### Questions 

Some misc questions/concepts from memory:
- source freshness checks: which causes the job to fail/stop executing if a source is stale (`dbt source freshness` or using the checkbox in the job [ref](https://docs.getdbt.com/docs/deploy/source-freshness#:~:text=Review%20the%20following%20options%20and%20outcomes%3A))

- CI and merge job setup questions 

- know where certain functionality or permissions are configured: account vs. project vs. environment vs. account connection settings vs. user specific settings vs. warehouse grants
i.e. 
	- Threads (Development: dbt development credentials in user profiles, Deployment: set at JOB level) 
  - Git branch, target schema, and dbt version are all required for environments (where are these configured for deployment vs. dev environments)

- Resolving values of environment variables, know where overrides are available 

- Knowing generally where you can find things in Explorer/Catalog: 
- overview, performance, recommendations sections of models and projects

- dbt clone vs. defer and how these interact with the compare changes against checkboxes on a job

- state selector (modified, new, old)

#### Threads

Given: Dag below, warehouse concurrency limit of 4 simultaneous queries 
![dag_example.png](/images/dbt-cert-screenshots/dag_example.png)

- 8 threads
- 6 threads
- 4 threads
- 3 threads
- 2 threads


<details>
    <summary>Answer speculation</summary>
  I think 2 queued for 8 and 6 threads, 0 for 4,3,2 threads
</details>

Know also how many threads would be idle without the warehouse concurrency limitation 

#### Permission / RBAC
Minimum permissions needed to enable advanced CI?
I remember some options being: git admin, job admin, account admin
<details>
    <summary>Answer speculation</summary>
  I think only account admin (see dbt docs: <a href="https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions">Enterprise permissions</a>)
</details>

- Handful of permission-related questions, referncing the tables in the [Enterprise permissions](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions) doc would be good. From memory: 
	- asked about what permission sets:
  		- can run / create jobs
    	- create git integrations
      
	- what specific permisions sets can/can't do
  		- analyst 
      - developer 
      - all the different admins
      
  - what developer can/can't do by default, what analyst can/can't do by defualt 
  
  - how permisions interact with licenses, who can view public models in Catalog
  
  - Know generally only account admin can do/what falls under "Account Settings"
  

#### Setting up git integration
- PR templates: where do you set this up / is it a requirement of a dbt project? when is one auto-generated for you? what happens when not set up correctly?

- Where would you navigate to find git deploy keys?
  

- Know the differences between native dbt-git integrations and using URL+deploy keys to set up a different git provider. Also know the limitations of managed repos. There was a matching question asking about native git integration vs. one of these other setup options. Select native, non-native, or both for each "feature"
	- Commit code via Studio IDE
  - Resolve merge conflicts in Studio IDE
  - Use ssh protocol 
  - configure automated CI jobs
  - updating GitHub pull requests with dbt run statuses
  
  
#### dbt Mesh 
This was heavily tested 
- Cross project refs – given downstream project Environment (DEV, STAGING, PROD, general), how will the references to an upstream project resolve (what environment?). Know the nuances here - i.e. STG environment does not exist in upstream project
[review](https://docs.getdbt.com/docs/mesh/govern/project-dependencies#staging-with-downstream-dependencies)

- question about targeting downstream project dependencies on upstream project's fresher sources (package depency is used in this case so it's technically possible to use `+` dependency syntax) / setting up job to trigger when another finishes 

- understand how access modifiers/using packages for dependencies works

- other questions about model access modifiers w/ cross project dependencies, job chaining (upstream project job triggers downstream project job) 

- shown a DAG, asked to click on the indicators that dbt mesh is being utilized: 
	- dotted lines indicate an external dependency (i.e. another project)
  - downstream project's nodes, should only depend on public models from an upstream project if cross-project refs are being utilized 
  


#### Resolving database paths
Resolving how model will show up in the warehouse as `database.schema.table_name`. Knowing where these values can be inherited from.
- `generate_database_name`, `generate_schema_name`, `generate_alias_name` macro logic can be overwritten


database name can come from:
- Account → connection settings (required for snowflake, optional for other providers)
- Environment → connection settings (where the default is typically configured)
- `database` config key at model level (or applied to entire project or subdirectory of models in `dbt_project.yml`)

schema name:
- Default is set at Environment level (deployment credentials or user’s development credentials)
- Custom value can be concatenated to this default when specified using the `schema` config key at model level (or applied to entire project or subdirectory of models in `dbt_project.yml`) 
- if custom value is provided, resulting schema name is: `default_custom`

object name: 
- if no `alias` is configured for a model, the object name is named after the `.sql` filename: `model_name.sql`
- If an `alias` is defined, the object name will be the value provided instead 

NOTE: there is also the `name` in the property for a model 
```
models:
- name: [<model-name>]
```
defines what is referenced in the ref macro `ref{{'<model-name>'}}`. Best practice is usually to match this to the `.sql` filename but changing this doesn't change name the database object in the warehouse.

### Jobs
Job queuing: 
in general if the same job is triggered while it is still running, a new run will be queued to start after the current run completes.

If a job is triggered more than once while it is still running, previously queued jobs will be canceled, the one most recently triggered one is queued and will begin after the current run completes.

Different behavior for CI jobs (see below)

### Common job patterns cheat sheet
common commands/selectors 

#### CI jobs
Usually jobs are configured in a seperate qa or STG environemnt. Triggered when PR is created against `main`, compare changes against PROD **Environment**. 

`dbt build --select state:modified+` 

default schema is ignored (custom pr schema used instead) which is deleted from warehouse on merge or PR close.

if project contains incremental models, may need to add a `clone` step before build i.e. 
`dbt clone --select state:modified+,config.materialized:incremental,state:old`

CI jobs and job queuing: 
- N PRs will result in N CI jobs running at once (unlimited). Targets will be in different schemas so there is no overlap. 
- If a CI job is still running and there is a new commit on the SAME open PR branch, the currently running job is canceled and a new job is triggred, testing fresh code

if 2+ trunk / inderect promotion is used, then in addition to the above...another CI job triggered on PRs created against `custom_qa_branch` (same dbt command/selector). Compare changes is set to the custom qa or STG **Environment**. Additional regularly scheduled `dbt compile` jobs may help stablize the environment state if many PRs are in flight

#### Merge jobs 
Run from PROD Environment, Triggered when someone merges changes into `main` branch. 
`dbt build --select state:modified+`, compare changes against PROD **Environment**

### Deploy job patterns 
##### Fresher builds
build models only when source data is fresher than last run
`dbt source freshness`
`dbt build --select source_status:fresher+`
`dbt docs generate`

Self deferral can be useful here, especially if other selectors are included. Compare changes set to "This job" (Need to run it at least once before this can be set).