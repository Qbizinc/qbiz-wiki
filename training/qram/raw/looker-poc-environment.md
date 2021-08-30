---
title: Qbiz Looker POC Environment
description: Overview of Qbiz Internal Looker Partner Portal available for POCs
published: true
date: 2021-08-30T22:11:23.109Z
tags: looker, poc, lookml
editor: markdown
dateCreated: 2021-08-30T15:57:17.235Z
---

# Qbiz Looker POC Environment
### Overview
- Qbiz has an environment that runs on the Looker cloud offering available for internal POCs an familiarization with the Looker functionality.  If you would like access to this environment please reach out to Scott Mitchell (scott.mitchell@qbizinc.com)

- The environment can be accessed at the following url.
https://qbiz.cloud.looker.com/login

### Setup of the environment 
- A sample project has been setup against a mysql external dimensional tutorial DB.  This project and corresponding Data Warehouse DB can be used for POC and evaluation of the Looker functionalities.  Below are the steps followed to setup the POC environment.

#### Looker Project Creation and Configuration
- Setup Connection to mysql DB for DW tutorial
https://docs.looker.com/setup-and-management/connecting-to-db
https://docs.looker.com/setup-and-management/database-config?version=21.12
https://docs.looker.com/setup-and-management/database-config/mysql


![config-db-1.png](/images/looker-screenshots/config-db-1.png)

![config-db-2.png](/images/looker-screenshots/config-db-2.png)


- Project Creation
![create-prj-1.png](/images/looker-screenshots/create-prj-1.png)

![create-prj-2.png](/images/looker-screenshots/create-prj-2.png)

![create-prj-3.png](/images/looker-screenshots/create-prj-3.png)

- Setting up GIT
create new GIT repository
![git-config-1.png](/images/looker-screenshots/git-config-1.png)
![git-config-2.png](/images/looker-screenshots/git-config-2.png)
copy the ssh url
![git-config-3.png](/images/looker-screenshots/git-config-3.png)
in looker click configure git from the model page
![git-config-4.png](/images/looker-screenshots/git-config-4.png)
paste in the ssh url
![git-config-5.png](/images/looker-screenshots/git-config-5.png)
copy the key
![git-config-6.png](/images/looker-screenshots/git-config-6.png)
in git settings select deploy keys
![git-config-7.png](/images/looker-screenshots/git-config-7.png)
paste key in git
![git-config-8.png](/images/looker-screenshots/git-config-8.png)
authenticate to git
![git-config-9.png](/images/looker-screenshots/git-config-9.png)

#### Dimension Setup
- Import Views
![import-view-1.png](/images/looker-screenshots/import-view-1.png)
view import creates simple dimension(attribute) for each field in imported table
![import-view-2.png](/images/looker-screenshots/import-view-2.png)

- Create additional dimensions
best practices for referencing dimensions
![dim-setup-1.png](/images/looker-screenshots/dim-setup-1.png)

![dim-setup-2.png](/images/looker-screenshots/dim-setup-2.png)
reference dimension in metrics
![dim-setup-3.png](/images/looker-screenshots/dim-setup-3.png)
>   measure: sum_qty_sold {
>     type:  sum
>     sql:  ${qty_sold} ;;
>   }
{.is-info}

![dim-setup-4.png](/images/looker-screenshots/dim-setup-4.png)

![dim-setup-5.png](/images/looker-screenshots/dim-setup-5.png)
dimension type example string
>   dimension: fullname {
>     type: string
>     sql:  ${cust_first_name}||''||${cust_last_name};;
>   }
{.is-info}



![dim-setup-6.png](/images/looker-screenshots/dim-setup-6.png)
dimension type example number
>   dimension: days_since_order_date {
>     type: number
>     sql:  DATE_DIFF(current_date(),${order_date},day) ;;
>   }
{.is-info}



![dim-setup-7.png](/images/looker-screenshots/dim-setup-7.png)

![dim-setup-8.png](/images/looker-screenshots/dim-setup-8.png)
dimension type example yesno and tier 
>   dimension: is_new_order {
>     type: yesno
>     sql: ${days_since_order_date}<=30 ;;
>   }
>   
>   dimension: order_date_tier {
>     type:  tier
>     sql:  ${days_since_order_date} ;;
>     tiers: [0,10,20,30,60,90,180,360,720]
>     style: integer
>   }
{.is-info}

![dim-setup-9.png](/images/looker-screenshots/dim-setup-9.png)

![dim-setup-10.png](/images/looker-screenshots/dim-setup-10.png)

![dim-setup-11.png](/images/looker-screenshots/dim-setup-11.png)
dimension type example dimension group type time
>   dimension_group: order {
>     type: time
>     timeframes: [
>       raw,
>       date,
>       week,
>       month,
>       quarter,
>       year
>     ]
>     convert_tz: no
>     datatype: date
>     sql: ${TABLE}.ORDER_DATE ;;
>   }
{.is-info}

![dim-setup-12.png](/images/looker-screenshots/dim-setup-12.png)

![dim-setup-14.png](/images/looker-screenshots/dim-setup-14.png)

![dim-setup-13.png](/images/looker-screenshots/dim-setup-13.png)
dimension type example dimension group type duration
>   dimension_group: durations_order_to_ship {
>     type:  duration
>     intervals: [second,minute,hour,day,week,month,quarter,year]
>     sql_start: ${order_date};;
>     sql_end: ${ship_date};;
>   }
{.is-info}

![dim-setup-15.png](/images/looker-screenshots/dim-setup-15.png)

![dim-setup-16.png](/images/looker-screenshots/dim-setup-16.png)

#### Measure Setup
![measure-setup-1.png](/images/looker-screenshots/measure-setup-1.png)

![measure-setup-2.png](/images/looker-screenshots/measure-setup-2.png)

![measure-setup-3.png](/images/looker-screenshots/measure-setup-3.png)
measure type examples
>   measure: total_order_cost  {
>     type:  sum
>     sql:  ${order_cost} ;;
>   }
>   
>   measure: avg_order_cost  {
>     type:  average
>     sql:  ${order_cost} ;;
>   }
>   
>   measure: number_of_orders{
>     type: count
>   }
>   
>   measure: count_of_orders{
>     type: count
>     sql: ${order_id} ;;
>   }
>   
>   measure: distinct_orders{
>     type: count_distinct
>     sql: ${order_id} ;;
> {.is-info}

#### Explore Setup
- use include statement in model to makes views accessible in explore
> connection: "tutorial"
>
> '# include all the views`
> #include: "/views/*.view"
> #include: "/z_tests/*.lkml"
> #include: "/**/*.dashboard"
> include: "/**/*.view.lkml"                 # include all views in this project
> 
> 
> datagroup: mysql_tutorial_default_datagroup {
>   '# sql_trigger: SELECT MAX(id) FROM etl_log;;`
>   max_cache_age: "1 hour"
> }
> 
> persist_with: mysql_tutorial_default_datagroup
> 
> explore: order_fact {
>   join: lu_customer {
>     type: left_outer
>     sql_on: ${order_fact.customer_id} = ${lu_customer.customer_id} ;;
>     relationship: many_to_one
>   }
> }
> {.is-info}

- model reference links
https://docs.looker.com/data-modeling/learning-lookml/explore-menu-and-field-picker
https://docs.looker.com/data-modeling/getting-started/model-development#model_files
https://docs.looker.com/reference/explore-params/explore
https://docs.looker.com/reference/field-reference/dimension-type-reference
https://docs.looker.com/data-modeling/marketplace
https://help.looker.com/hc/en-us/articles/360001784587-Best-Practice-Writing-Sustainable-Maintainable-LookML


- explore best practices
![explore-setup-1.png](/images/looker-screenshots/explore-setup-1.png)

![explore-setup-2.png](/images/looker-screenshots/explore-setup-2.png)

![explore-setup-3a.png](/images/looker-screenshots/explore-setup-3a.png)

![explore-setup-3b.png](/images/looker-screenshots/explore-setup-3b.png)

![explore-setup-4.png](/images/looker-screenshots/explore-setup-4.png)

![explore-setup-5.png](/images/looker-screenshots/explore-setup-5.png)

![explore-setup-6.png](/images/looker-screenshots/explore-setup-6.png)

- symmetric aggregates (fanout problem)
![symmetric-agg-1.png](/images/looker-screenshots/symmetric-agg-1.png)

![symmetric-agg-2.png](/images/looker-screenshots/symmetric-agg-2.png)

![symmetric-agg-3.png](/images/looker-screenshots/symmetric-agg-3.png)
- to prevent fanout Looker uses symmetric aggregation 
to ensure symmetric aggregation is applied make sure primary key and relationships are defined for all joins
![symmetric-agg-4.png](/images/looker-screenshots/symmetric-agg-4.png)

![symmetric-agg-5.png](/images/looker-screenshots/symmetric-agg-5.png)

![symmetric-agg-6.png](/images/looker-screenshots/symmetric-agg-6.png)

![symmetric-agg-7.png](/images/looker-screenshots/symmetric-agg-7.png)

![symmetric-agg-8.png](/images/looker-screenshots/symmetric-agg-8.png)

![symmetric-agg-9.png](/images/looker-screenshots/symmetric-agg-9.png)

![symmetric-agg-10.png](/images/looker-screenshots/symmetric-agg-10.png)

![symmetric-agg-11.png](/images/looker-screenshots/symmetric-agg-11.png)

![symmetric-agg-12.png](/images/looker-screenshots/symmetric-agg-12.png)

![symmetric-agg-13.png](/images/looker-screenshots/symmetric-agg-13.png)
