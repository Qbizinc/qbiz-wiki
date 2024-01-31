---
title: AWS
description: Page to help new Qbiz consultant onboard onto Qbiz's AWS Account
published: true
date: 2024-01-31T23:47:43.183Z
tags: 
editor: markdown
dateCreated: 2024-01-31T20:25:14.970Z
---

# Quickstart Onboard

Qbiz has an AWS account for consultants to test, experiment and gain skills in using AWS tools.

The AWS login for IAM users can be seen here: https://qbizinc.signin.aws.amazon.com/console

Here is a quick guide on how to get up and running on AWS ASAP:

- Find another consultant that already has AWS access to create an IAM user for you

- FOR THE CONSULTANT WITH ACCESS: 
  1. Make sure the new IAM user is added to the various AWS IAM groups already made (`developers`, `qbiz-users`, etc.)
  2. Set up a temporary password and check the box making it required for the new user to change their password after first sign in
  3. Send the new consultant their username and temporary password

- FOR NEW CONSULTANTS:
  1. Log in [here](https://qbizinc.signin.aws.amazon.com/console) with your username/password
  2. Set up a new strong password and log in again (recommended to use Lastpass, 1password, etc.)
  3. Go to the IAM UI (search "IAM" in the searchbar + click on the "IAM" block) 
  4. Click on "Users" in the "IAM Resources" section and find your user (will be your username)
  5. Click on "Security Credentials" and find the "Multi-factor authentication (MFA)" section
  6. Click on "Assign MFA Device" and follow the instructions to setup MFA for your IAM user  
    - *NOTE: The recommended method is the "Authenticator app" method (compatible phone apps include Okta & Duo)*
  7. Log out and log back in using MFA (This will grant your IAM user all the permissions from the IAM groups)
  


CONGRATS! You should now have access to most AWS services.

If you require additional permissions, please reach out to one of the Staff Consultants.

## Datahub

There is a EC2 instance called "Datahub" which has a Qbiz version of Datahub.

Here are the steps to get the Datahub service up and running on that EC2 instance:
1. Download [datahub.pem file](https://drive.google.com/file/d/1LturddKPDgEAcI_7WhJVd3II0CJu4zU1/view?usp=drive_link)
2. Move to `~/.ssh` directory via `mv ~/Downloads/datahub.pem ~/.ssh/`
3. Save Datahub ssh command as alias: `login_datahub='ssh -i ~/.ssh/datahub.pem ec2-user@34.210.197.39'`
4. Run `$login_datahub` to login into EC2 alias
   a. If you run into "bad permissions" issues: 
      - Change permissions of datahub.pem file via `chmod 400 ~/.ssh/datahub.pem`
5. Run `bash start_datahub.sh` to start Datahub
   a. Might take a couple of tries
6. Confirm Datahub has started by:
   a. Running the `datahub` command and seeing the various datahub commands
   b. Navigating to the [Datahub UI](http://34.210.197.39:9002/login?redirect_uri=%2F)
      i. User/Pass: `datahub`