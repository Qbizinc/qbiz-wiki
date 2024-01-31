---
title: AWS
description: Page to help new Qbiz consultant onboard onto Qbiz's AWS Account
published: true
date: 2024-01-31T20:52:30.016Z
tags: 
editor: markdown
dateCreated: 2024-01-31T20:25:14.970Z
---

# Quickstart Onboard

Qbiz has an AWS account for consultants to test, experiment and gain skills in using AWS tools.

Here is a quick guide on how to get up and running on AWS ASAP:

- AWS login for IAM users: https://qbizinc.signin.aws.amazon.com/console
  - Have another consultant that already has access create an IAM user for you
  - FOR THE CONSULTANT WITH ACCESS: 
    - Make sure the IAM user is added to the various AWS IAM groups already made (`developers`, `qbiz-users`, etc.)
    - Set up a temp password and mark that it will be required for user to change password
  - FOR NEW CONSULTANTS:
    - Once logged in, go to the IAM user UI and setup MFA (i.e. add a compatible device; Okta/Duo should do it)
      - Might have trouble finding, can search for "MFA" with CTRL + F
    - **Log out and log back in with MFA** (This will grant your IAM user all the permissions from the IAM groups)