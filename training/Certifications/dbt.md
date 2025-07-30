---
title: dbt certifications
description: 
published: true
date: 2025-07-30T18:05:57.097Z
tags: 
editor: markdown
dateCreated: 2025-05-23T01:49:36.130Z
---

# dbt Certifications
https://www.getdbt.com/dbt-certification

- dbt Analytics Engineering Certification Exam
- dbt Cloud Architect Certification Exam
- discount codes: `5YDYM`, `WWFYJ`
- ask about partner program / exam vouchers


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

- packages
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

Natalia's post-exam notes (July 2025):

### Note regarding exam timing
A lot of features released just before taking the exam, refer to the exam guide and note dates of the provided training content. Limitations / best practices discussed in 2023, for example, likely have changed. Docs are generally up to date but double check these things when reviewing video content and blog posts (even if linked in the exam guide).

Most notably: 
- `defer` functionality within job settings (`Compare changes against` drop down): previously chose a specific job to compare artifacts against. Now, you chose a specific Environment or "this job" for self deferral.

- Some training content doesn't treat connections as resuable / refers to connection settings at the project level. 

- Renaming 
 	- pricing tiers: i.e. Teams is now Starter. Though, I got no questions about this - exam seemed to test from the perspective of the highest tier. 
  
  - New Analyst license type (though, again, not tested for me)
  
  - general rebranding from "dbt Cloud" to just "dbt" can make comparisons to dbt core a little confusing

- SCIM support is available, contrary to some training materials referring to it as not supported 

- Training materials cover how to set up "CI jobs" using 








  
