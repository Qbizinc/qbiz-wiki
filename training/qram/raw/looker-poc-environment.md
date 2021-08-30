---
title: Qbiz Looker POC Environment
description: Overview of Qbiz Internal Looker Partner Portal available for POCs
published: true
date: 2021-08-30T20:24:25.384Z
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

- Import Views
![import-view-1.png](/images/looker-screenshots/import-view-1.png)

![import-view-2.png](/images/looker-screenshots/import-view-2.png)


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





