---
title: AWS Cloud Practitioner
description: This is the foundational AWS certification. Above this there are more focused certifications (e.g. ML Specialty).
published: true
date: 2023-04-25T19:11:50.245Z
tags: training, certification
editor: markdown
dateCreated: 2023-01-18T19:37:33.663Z
---

# AWS Cloud Practitioner
The AWS Cloud Practitioner certification is meant to validate cloud fluency and foundational AWS knowledge. The exam is 90 minutes, 65 questions, and costs $100. The exam covers: 
- Technology (~33%)
- Cloud Concepts (~26%)
- Security & Compliance (~25%)
- Billing & Pricing (~16%)

## Preparation
AWS provides excellent free preparation material online. A quick search should get you to the right place with a few clicks. 

At present the best study guide is [here](https://aws.amazon.com/certification/certification-prep/?ch=cta&cta=header&p=2). It provides: 
- Exam guide
- Sample question (10)
- Course: AWS Cloud Practitioner Essentials
  - Has 60 question practice exam at the end + practice throughout
- Subset of (3) whitepapers to study
- Exam prep webinars
- Practice exam

### Other Prep
- Scott put together a [Google doc](https://docs.google.com/document/d/1afmLKGne04hfWemw4tDSxNN6z14VBde6yJiRMQcv7j0/edit#)
  - It covers the course, but also has material at the end on content that wasn't necessarily in the course, but was on the exam.
- Lots of questions around well-architected framework on the exam. (e.g. What are the pillars? Which pillar does __ fall into?)
- Deepest parts of the exam is around roles and users. Other stuff tends to be more surface level. 
- Material on Bryan's exam (March 2023):
	- Specifically, make sure you understand specific AWS solutions, particularly around Machine Learning (Rekognition, QuickSight, etc.)
  - Review the different types of storage systems and their tradeoffs/compatibility with other resources
  - Lots of questions about various database systems and their compatibility/efficiency with RDS
  - Review all the different types of network security, specifically as it relates to blocking SQL injection attacks or unauthorized web traffic
  - Also understand the various ways to connect ioT devices and/or private on-prem data centers to VPC
- A set of six practice tests by Udemy can be found [here](https://drive.google.com/drive/folders/1qGI4t6G-3TK2gFfN0xxtqp9U1SX8K7Ng)
- Nate also put together a [Google Doc](https://docs.google.com/document/d/1rgpkTq3sau5BA5BFWoefMFnL5_j591eB1VTjbD9YV4U/edit?usp=sharing)
	- Lots of different resources, free and paid
  - Would recommend paying for "A Cloud Guru" and expensing this cost: https://acloudguru.com/
- Material on Nate's Exam (April 2023):
	- Multiple questions on the "Cloud Adoption Framework" and "AWS Well Architected Framework"
  - Multiple questions on VPCs, subnets, Network Access Control Lists (ACL), security groups; make sure to know differences
  - Multiple questions on various ways that AWS resources can be accessed (AWS console, application code via SDKs, CLI)
  - LOTS of questions on AWS shared responsibility model, make sure to know this very well
  	- I.e. difference between what is AWS's responsibility regarding non-managed/non-serverless services (i.e. EC2) vs managed/serverless architectures (i.e. S3, RDS, etc.)
  - Had a question regarding best practices on securing AWS accounts, my chosen answers were:
  	- Implementing MFA on AWS users
    - Restricting IP access to AWS users
  - Had a question regarding the following (was either "Redshift" or "DynamoDB"; not 100% sure but went with "DynamoDB"):
  	- "Company has alot of unstructured data (DynamoDB?) and requires up to millions of concurrent users (Redshift?); which DB solution should be used?"