---
title: Qbiz Looker POC Environment
description: Overview of Qbiz Internal Looker Partner Portal available for POCs
published: true
date: 2021-08-30T21:11:42.712Z
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



