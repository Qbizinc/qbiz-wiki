---
title: AWS Sandbox Data Pipeline
description: Landing Page for AWS Sandbox Data Pipeline
published: true
date: 2024-02-27T21:58:04.543Z
tags: 
editor: markdown
dateCreated: 2024-02-22T17:53:03.781Z
---

# Sandbox Data Pipeline

Qbiz has a sandbox data pipeline orchestrated by Airflow running on an AWS EC2 instance named `airflow`.

The source code can be found here: https://github.com/Qbizinc/sandbox-data-pipeline

*Note: The EC2 instance is scheduled to shut down every day at 5pm PT and start up again at 9am PT.*

## Required Resource Access

- Airflow UI (ask someone for the user/password if needed): http://airflow.qbiz-wiki.com/home
- AWS: https://qbizinc.signin.aws.amazon.com/console
- GCP: https://console.cloud.google.com/welcome?project=sandbox-data-pipeline
- Anomalo: https://app.anomalo.com/dashboard/home/search?sort=status
- Datahub (Qbiz EC2 instance): http://34.210.197.39:9002/login?redirect_uri=%2F
- Snowflake: https://app.snowflake.com/a4877751795961/qbiz_partner

## Accessing EC2 instance

Currently the code is manually edited on the EC2 instance itself; the current way consultants have been accessing/editing the code has been through remote connections (ssh) via local text editor.

Instructions for setting up remote connections via:
- Visual Studio Code: https://dev.to/cindyledev/remote-development-with-visual-studio-code-on-aws-ec2-4cla
- Pycharm: INSERT

## Setup

*Note: This section will be modified as the optimization work below is implemented.*

### Connection variables
In the event that the Airflow instance loses its metadata, some Airflow connection parameters need to be set up:
- `qbiz_snowflake_admin` (Make sure your Snowflake account has access to the `ACCOUNTADMIN` role)
  - Connection Id: `qbiz_snowflake_admin`
  - Connection Type: `Snowflake`
  - Login: YOUR-SNOWFLAKE-USER-NAME
  - Pass: YOUR-SNOWFLAKE-PASSWORD
  - Role: `ACCOUNTADMIN`
  - Account: `a4877751795961-qbiz_partner` (if this doesn't work, try `LMB13195`)
- `sandbox-data-pipeline-gcp`
  - Connection Id: `sandbox-data-pipeline-gcp`
  - Connection Type: `Google Cloud`
  - Keyfile Path: `/home/airflow/sandbox-data-pipeline-bigquery.json` (local path on EC2 instance)

## Maintenance

The existing Airflow EC2 setup has a number of existing issues requiring additional maintenance (see below backlog list).

The following steps must be performed regularly (i.e. at least once a week):
- Purging files from the Airflow metadata DB
	- Example command (remove --dry-run to actually remove): 
      - `airflow db clean --clean-before-timestamp 2024-01-01T00:00:00+00:00 --dry-run --skip-archive`
- Remove logs from local Airflow storage
	- Will be in `~/airflow/logs` directory
- Drop temporary tables
  - `select 'drop table '||table_name||';' from information_schema.tables where table_schema='public' and table_name like '\_%';`

## Optimization Work Backlog

## DAG
- Update Pipeline to wait in the event of too many requests to the RapidAPI
  - Sample response: `{'message': 'Too many requests'}`
  - Has not been needed yet, but could still happen in the future
- Update DAG to assume AWS service roles and get temp credentials
- Snowflake table writing problem (i.e. BQ path problem)?

### DevOps
- Implement CI/CD with the source code above 
- Turn existing Airflow EC2 setup completely into Terraform code to automate resource creation (i.e. in a failure scenario)

### Instance Optimizations
- Figure out why logs are still being written to the local Airflow instance (the DAG is writing the logs to S3)
- Look into increasing the EBS volumes on the instance (would incur additional costs)
  - https://aws.amazon.com/ebs/pricing/
- Create a DAG to purge Airflow metadata DB (https://fig.io/manual/airflow/db/clean)

### AWS optimizations
- Lock down the AWS admin account
  - https://aws.amazon.com/blogs/security/how-to-create-a-limited-iam-administrator-by-using-managed-policies/
  - https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html#access_policies_boundaries-delegate