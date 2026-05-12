---
title: GCP
description: Brief description of Google Cloud Platform Administration, IT and  Projects
published: true
date: 2026-05-12T18:41:40.988Z
tags: cloud, it
editor: markdown
dateCreated: 2026-05-12T18:17:01.932Z
---

# Google Cloud Platform
## Accessing
As a member of the Qbiz Google Workspace, you have access to GCP using your Google Workspace Credentials.  Navigate the the [Google Cloud Console](https://console.cloud.google.com/welcome) and login.

## Projects
Anyone should be able to create their own Project.  Projects are organized under the qbizinc.com organization.

### Sandbox Data Pipeline
This project contains users and resources related to the Sandbox Data Pipeline __Tools Evaluation__ initiative, includeing BigQuery, object storage and credentials.  This project incures a small, monthly cost for the resources.

### Sandbox Authentication
A project to control access to various lab resource, such as the Airflow running on an EC2 instance.  Access for all Qbiz Workspace users can authenticate using OAHTH2 to any so configured resources.

### Soren's Gemini Project
This project was created to research using the Gemini LLM API for bench users and application research.  At this time, there is a single Default API key used by multiple users.  The intension was to setup one key per user, it seems the Continue extension can't prompt the user to authenticate.  Therefore, all users will need to share the default API Key.  For security, the key should be routed monthly and published to the Slack tools_eval channel.  

## Billing
If your project(s) required a billing account, you will need to use your personal credit card -- unlike AWS, GCP hasn't been setup with a corperate credit card.  If your expenses are for training or project-reated, you can request to be reimbust.  Get written permission from leadership beforehand.

## Gemini API
