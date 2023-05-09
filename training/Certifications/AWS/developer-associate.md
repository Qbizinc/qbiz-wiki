---
title: AWS Developer Associate
description: 
published: true
date: 2023-05-09T22:40:03.594Z
tags: 
editor: markdown
dateCreated: 2023-04-20T04:47:20.992Z
---

Note: these are not complete notes but are rather items I needed to remember. [Google doc](https://docs.google.com/document/d/1xwlkUBe_Nat9P9opwH5R_0obYa-xEea1rHrhtSUsIVg/edit?usp=sharing)

A few key areas that aren't well-covered here but appeared heavily on the exam:
* API Gateway and CodeBuild/CodeDeploy should be studied in significant detail, ideally with some hands on labs and/or experimentation
* Quickest vs safest/least-disruptive deployment stratgies are important topics
* You should thoroughly understand how Lambda does and doesn't work  with resources in a VPC. The trainings I took and the notes here DO NOT cover newer VPC-specific Lambda features or a new feature called Lambda@Edge
* Knowing how to enforce encryption at rest and in transit for a variety of services is extremely important. Sometimes the answer is as detailed as putting a specific tag in an HTTPS request header

# S3


## Encryption


### Types



* In transit: SSL/TLS, will use HTTPS requests
* At rest - server side
    * SSE-S3: S3 managed keys, AES-256 bit
    * SSE-KMS: Key Management Service managed keys
        * Envelope key: encrypts your encryption key
        * Audit trail
    * SSE-C: customer managed keys
        * AWS manages encryption/decryption but you manage keys
* At rest - client side
    * You encrypt files before loading them


### Enforcement



* Server side can be enabled
    * via console
    * via bucket policy
        * Bucket policy should deny any request that does not have x-amz-server-side-encryption parameter
* In transit
    * Bucket policy should deny requests without aws:SecureTransport parameter


## CORS - Cross Origin Resource Sharing



* For static web hosting, allows resources to be loaded from other buckets



## CloudFront


### Overview



* Content Delivery Network (CDN) – speeds up delivery of web content
* **Geographically distributed**
* Edge location (located near end users) requests content from main/remote server and caches results
* Works with both AWS services and your own servers


### Terminology



* CloudFront Edge Location: local cache
* CloudFront Origin: origin of files that will be cached/served by Edge Location
* CloudFront Distribution: Origin + configuration for content you want to distribute
* Origin Access Identity: special user that can directly access origin buckets
    * Public access to origin bucket should be denied to force Edge Locations to be used
* TTL: Time to Live before clearing cache
    * default 1 day
    * Cache can be cleared manually but costs $
* S3 Transfer Acceleration: optimized network paths for S3 transfers
    * Can be used in conjunction with CF to upload files to S3 efficiently (e.g., enables upload to S3 via Edge Locations)	


### AllowedMethods



* GET, HEAD – read only
* GET, HEAD, OPTIONS – read only
* GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE – read/write – required if users can write (e.g. shopping cart)


### Other



* Total number of objects you can store is unlimited
* Largest single PUT: 5GB
* Largest objects: 5TB
* Glacier Instant Retrieval allows millisecond retrieval for data accessed once per quarter


# Lambda


## API Gateway



* Supported API types:
    * **RESTful — serverless, stateless (e.g., for use with Lambda, primary exam focus)**
    * Websocket – realtime, stateful, 2 way (e.g., chat app)


### RESTful features



* For use with Lambda and other services including EC2, Elastic Beanstalk, DynamoDB, Kinesis
* Different API endpoints can be sent to different targets
* Versioning
* Throttling
* CloudWatch


## Versioning



* Version labels can be appended to the ARN
* When you create a function or upload a revision, the version is labeled $LATEST
    * $LATEST is implicit in an unqualified ARN
* Aliases (e.g., "prod", "test") can be created for prior versions
    * The alias can be appended to the ARN


## Concurrent Executions



* Limited per Account/Region per second: 1000
    * If exceeded, http error code is 429
    * Limit can be extended by request
* Reserved concurrency: reserve concurrency limits for critical functions 


## X-Ray



* Lambdas can be configured to use X-Ray traces for Cloudwatch error handling
* But they must be enabled via a Lambda layer


## VPC



* To access resources in a VPC, Lambda requires:
    * VPC ID
    * Private subnet ID
    * Security group ID
* This sets up an Elastic Network Interface (ENI) using an available subnet IP address


# Step Functions


## Standard Workflows



* Long-running
* Non-idempotent
* At-most-once model (runs at most once unless retriggered)


## Express Workflows



* Short, up to 5 minutes
* Idempotent
* At-least-once model


### Synchronous Express



* Waits for completion before returning result
* Good for operations that are run one time
* E.g., process payment and send order


### Asynchronous Express



* Does not return result
* Does not wait for completion
* Results can be found in Cloudwatch logs
* Typically run in the background
* E.g., send a 1-way message and then move on


# X-Ray



* For analyzing/debugging distributed applications
* Service map: visual diagram of requests/relationships between components
    * Shows latency, HTTP status, error messages
* Captures metadata for API calls
* On most systems (e.g., EC2, ECS), you must install X-Ray agent/deamon and use X-Ray SDK
* Lambda use requires an uploaded Layer


## Annotations



* Custom key/value pairs you can add to your instrumentation
* Indexed by X-Ray, can use filter expressions to search


# API Gateway


## Importing APIs



* API definition file using OpenAPI protocol (formerly Swagger, see swagger.io).
    * RESTful, returns JSON
* SOAP
    * Legacy
    * Returns XML
    * can configure API Gateway as XML web service passthrough
    * or use API Gateway to transform XML to JSON


## Versioning



* APIs have "stages" and releases can be rolled back


## Caching & Throttling



* Caching
    * Default TTL is 300 seconds
* Throttling
    * steady-state request rate is 10k requests per second per Region
    * max concurrent requests is 5k per Region across all APIs
    * 429 Too Many Requests error
    * Throttling can be configured on a per-client/key basis to prevent abuse


# DynamoDB



* Low latency NoSQL
* Supports both document and key-value data models
    * document types: JSON, XML, HTML
* Can autoscale
* Serverless, works well with Lambda
* SSD storage
* Resilient – always spread across 3 geographically distinct data centers
* Consistency:
    * Eventually consistent (default)
        * all 3 copies of data consistent within 1 second
        * best read performance
    * Strongly consistent
        * all 3 copies always consistent
* ACID transactions (called DynamoDB transactions)
* Data stored in tables
    * records are called "items"
        * max item size is 400k
    * columns are called "attributes"
        * Attributes can vary between items
* Primary keys: partition and composite


## Primary keys



* Key by which items are retrieved
* Partition key
    * unique attribute (e.g., customer_id)
    * value is hashed and this is used to physically distribute data
* Composite key
    * Primary key + sort key
    * Useful when partition key is not unique
    * Timestamps are typical for sort key


## Access Control



* Uses IAM – users and roles
* You can restrict data access to a user's own records (or other basis)
    * via "Condition" IAM policy
        * dynamodb::leadingKeys – partition key filter for which rows can be viewed
            * Can be dynamic. E.g., "www.mygame.com:user_id"


## Secondary Indexes



* Query attribute not based on primary key
* Local secondary index
    * Only created when creating table
    * Same partition key as table
    * But different sort key
    * Limit of 5
* Global secondary index
    * Can be created after table creation
    * Different partition key, different sort key from main table
    * Limit of 20
        * But can be increased with request


## Scan vs Query


### Query



* Finds items based on primary key, sort key (optional)
* Most efficient
* Returns all attributes by default
* ProjectionExpression to return only the attributes you're looking for
* Results always sorted by sort key
    * ScanIndexForward parameter to false to get descending
* Eventually consistent by default but you can specify Strongly consistent
* Smaller page sizes can result in faster response times (gives other operations a chance due to more frequent "pauses", thus lower impact)


### Scan



* Examines every item in table
* Least efficient
    * Scanning large tables can take up entire throughput
* Returns all attributes by default (same as Query)
* ProjectionExpression to return only the attributes you're looking for (same as Query)
* Attribute filters are used to limit results (but entire table is still scanned)
* Improving performance:
    * Scans are sequential, returning 1MB at time
    * One partition at a time
    * Use parallel scans to get around this
        * Avoid when db is busy
    * Write same data to 2 tables, one for scanning, one for normal operations
    * Set smaller page size (applies to Queries too) – operations are smaller, allowing other operations to complete without throttling


## API Calls



* create-table / CreateTable
* put-item / PutItem (add OR replace)
* get-item / GetItem
* update-item / UpdateItem (edit/add/delete attributes)
* update-table / UpdateTable
* list-tables / ListTables
* batch-get-item / BatchGetItems


## Capacity/Throughput


### Provisioned Throughput



* Provisioned throughput specified at table creation (can be modified)
* 1 write unit = 1kb write per second
* 1 read unit = 4kb read per second
    * 1 read for Strongly Consistent
    * 2 reads for Eventually Consistent (8kb total)
* Cost-wise, good for predictable loads
* There are quotas but they can be increased
    * 40k/40k read/writes per table
    * 80k/80k writes per account (not applicable for on-demand)


### On-demand Capacity



* Throughput is automatically scaled up and down
* Cost-wise, good for unpredictable/spiky loads


### DynamoDb Accelerator (DAX)



* Clustered in-memory cache
* 10x read performance improvement (microsecond response times)
* Ready heavy and/or bursty applications
* "Write-through" caching service
    * Data written to cache and backend store at same time
* APIs can hit DAX first. If item isn't available, DAX performs Eventually Consistent read against Db
* Eventually Consistent reads only


### Throughput Exceeded and Exponential Backoff



* ProvisionedThroughputExceededException
* SDK automatically retries using exponential backoff (all AWS SDKs do this)
* Not using SDK, application needs to implement exponential backoff strategy


## TTL



* Set expiry time for items
* Items marked for deletion will be deleted within 48 hours
* Expiry date/time is an attribute you populate in your table, expressed as epoch


## Streams



* Time-ordered sequence of item-level modifications
* Stored in logs, encrypted at rest, stored for 24 hours only
* Uses:
    * Trigger events based on changes
    * Auditing
    * Replication across multiple tables
* Accessed via dedicated endpoint
* By default, only primary key is recorded
    * But before/after images of items can be recorded too
* Services like Lambda poll streams to detect changes


# KMS (Key Management Service)


## CMK



* Customer Master Key
* Only encrypts/decrypts max of 4kb
* AMS can rotate on annual basis
* Encrypts/decrypts Data Key, which in turn ecrypts/decrypts the data ("Envelope Encryption")
* Symmetric: single key
    * Ok if using key exclusively within KMS
    * Can also be used for HMAC codes (verifying authenticity of a message, e.g., JWT token, credit card encryption token)
* Asymmetric: public/private key pair
    * Needed if also using outside KMS
* Must specify 2 IAM users: Key Manager and Key User
* Key Material Origin
    * Key Material = cryptographic sequence
    * Origin
        * KMS
        * External
        * Custom key store (Cloud HSM): single-tenant hardware, much more expensive
* Single or multi-region
* Key states: enabled, disabled, pending deletion, unavailable


## Envelope Encryption



* Generate Data Key (or "Envelope Key") to encrypt data >4k
* GenerateDataKey API to generate from CMK
* Encrypted Data Key is stored with the data, not stored in KMS. KMS decrypts it using the CMK
* Only the data key goes over the network to/from KMS, not your data


# SQS



* Simple Queue Service
* Temporary repository of messages awaiting processing
* Service polls the queue for messages (pull based)
* When messages are read, they are marked as invisible so they don't get processed by anything else
    * Visibility timeout: the amount of time the service has to process/delete the message. Default 30 seconds, max 12 hours
    * If timeout reached, message goes back into queue
    * The enables "decoupling of infrastructure" – e.g., the downstream thing can fail but the message won't be lost – SQS acts a buffer between components
* Messages can be up to 256kb
* Minimum in queue is 1 min, max 14 days
* Messages will be processed at least once


## Standard Queues



* Default
* Best-effort ordering
* Nearly unlimited transactions per second
* Duplicates possible


## FIFO Queues



* First-in-first-out
* Strict ordering
* Exactly once processing – no duplicates
* 300 transactions per second


## Polling



* Short polling
    * Returns immediate response even if queue is empty
    * You pay for empty returns
* Long polling
    * Polls queue periodically
    * No response until message received or timeout
    * Lower cost
    * Almost always choose this one


## Delay Queues



* Postpone delivery for a few seconds, messages remain invisible during delay period
    * Default 0, max 900 seconds
* Standard queues: turning on setting only affects future messages
* FIFO queues: affects messages already in queue
* Use cases:
    * large distributed systems. E.g., you might need to add a delay in sending a confirmation email to a customer to ensure processing had a chance to take place


## Large Messages



* 256kb to 2GB
* Store message in S3
* Use Amazon SQS Extended Client Library for Java
    * Cannot use AWS CLI, console, any other AWS SDK/API
    * Will also need AWS Java SDK - API for S3 bucket operations
    * Allows you to:
        * Specify whether messages are always stored in S3, or just those >256k
        * Send messages referencing message objects in S3
        * Get message object from S3
        * Delete message object from S3


# SNS



* Simple Notification Service
* Pub-sub/push model. Near instantaneous delivery
* Can send SMS, email, etc, even messages to SQS. Can send to any HTTP endpoint
    * Can also trigger Lambda
* Pushes to a "topic"; receivers subscribe to topic
* Durable storage to prevent messages from being lost
    * Stored redundantly across multiple AZs
* Can fanout to multiple services/subscribers


# SES



* Simple Email Service
* Scalable, highly available
* Can both send and receive; received emails are put in S3
* Incoming emails can trigger SNS and Lambda
* Often used for automated emails, especially for marketing
* Email address is all that's required to send emails
* Receivers don't need to subscribe to a topic (unlike SQS)
* Cannot fanout to multiple services


# Kinesis



* Family of services for working with streaming data
* Streams consist of continuous, very frequent, small, typically unstructured messages.
* Typical use cases:
    * Stock prices,
    * in-game data as the gamer plays,
    * social media feeds,
    * location tracking data
    * IoT sensors
    * clickstream data,
    * log files.
* Kinesis Streams: Data and Video streams
* Kinesis Firehose: capture, transform, and load data streams into data stores (e.g., for BI tools)
* Kinesis Data Analytics: realtime analytics. Use SQL to query/transform streaming data, store in data stores


## Data Streams



* Producers -> Kinesis Streams -> Consumers
* Default retention 24 hours, max 365 days
* Data stored in "shards"
    * Each shard is a sequence of data records
    * Fixed units of capacity
        * 5 read transactions per second, max 2MB per second 
        * 1000 write records per second, max 1MB per second
    * Capacity increased by increasing number of shards ("resharding")
* Order is always maintained – each record has a unique sequence number


### Consumers



* Use the Kinesis Client Library (KCL)
* KCL manages number of record processors based on number of shards and consumers
* KCL load balances between consumers
* CPU, not number of shards, should drive number of consumer instances
    * Use autoscaling


## Video Streams



* Secure video streams from devices connected to AWS
* Videos can be used for analysis or ML


## Firehose



* No shards
* No retention
* Capacity/sizing is handled for you
* No consumers
    * All data goes directly to S3 or to Lambda and then S3


## Kinesis Analytics



* Run SQL on realtime Streams or Firehose
* Store results in Redshift, S3, or OpenSearch


# Elastic Beanstalk



* Deploy and scaled web applications without worrying about EC2/infrastructure
    * Will provision load balancers, instances, autoscaling groups, application server platform, S3 buckets, databases
    * You have complete control of resources
* Code can be in most major languages
* No additional charges for using Beanstalk (only pay for created resources)
* Integrates with CloudWatch and CloudTrail but also has own health dashboard
* Application server platforms include Tomcat and Docker
* Can migrate from .NET via Application Migration Assistant (Windows-only)


## Deployments



* All at once deployment
    * Deploys to all hosts concurrently
    * Total outage while deployment (or rollback) takes place
    * Only suitable for dev/test environment
* Rolling
    * Deploys new version in batches
    * No outage but will reduce capacity
    * Good for low-performance requirements
* Rolling with Additional Batch
    * Launches additional instances then deploys in batches
    * Maintains full capacity through deployment

    Immutable

    * Launches new instances, deploys there, destroys old ones
    * No capacity degradation
    * Easy rollback
    * Preferred for mission-critical applications
* Traffic splitting
    * Deploys on new instances, leaves old ones,  then diverts % of traffic to them
    * Canary testing


## Custom Configuration



* AWS 1
    * Configuration file in YAML or JSON
    * File must have .config extension and be located in .ebextensions in top-level dir
* AWS 2
    * Buildfile
        * short commands that run and exit
        * goes in root directory
        * key value pairs &lt;process name>:&lt;command>
        * must include make: ./build.sh
    * Procfile
        * commands to start/run long-running applications
        * goes in root directory
        * same format as Buildfile
        * Beanstalk expects process to run continuously, will restart if fails
    * Platform hooks
        * Processes to run at various EC2 provisioning stages
        * Stored in dedicated directories
            * .platform/hooks/prebuild
            * .platform/hooks/predeploy
            * .platform/hooks/postdeploy


## RDS



* Can be within or without Beanstalk environment
* If created as part of Beanstalk, it will be terminated when environment is terminated
    * Ok for dev/test, not for prod
* If RDS is outside:
    * We need new security group to open port
    * Need to provide connection string
        * using Beanstalk environment properties


## Migrating Legacy to Beanstalk



* Tool for Windows only
* Windows Web Application Migration Assistant
    * Formerly .Net Migration Assistant
* Interactive PowerShell utility
* Open source


## Docker



* Can deploy Dockerized applications
    * Single container on EC2 instance
    * Multiple containers on an ECS cluster
    * Upload via zip file


# CI/CD



* Continuous Integration / Continuous Delivery or Deployment
    * Continuous Delivery: humans decide when to deploy
    * Continuous Deployment: deployment happens automatically
* CodeCommit (integration)
    * Fully managed private git repository
* CodeBuild (delivery)
    * compiles source code, runs tests, etc
* CodeDeploy (delivery)
    * Works with services like Lambda and EC2 but also on-prem
* CodePipeline (deployment)
    * CI/CD orchestrator


## CodeDeploy



* In-place deployment ("rolling")
    * Application stopped on each instance and new revision (release) installed
    * Will reduce capacity during deployment and ELB needs to not point to stopped instance
    * After deployment, instance is started and registered with ELB
    * Rollback takes just as much time
    * Lambda not supported
    * Fine for first-time deployments
* Blue/Green deployment
    * New revision goes on new instances (green, with blue being the existing instances)
    * ELB directed to new instances
    * Rollback is easy – ELB can be pointed back to old instances
    * Old instances eventually terminated
    * More costly because more instances
    * Safest option, no reduced capacity


### AppSpec File



* Deployment configuration
* Lambda and EC2/on-prem have different file formats
* EC2 is YAML only, Lambda supports JSON
* 4 significant sections of YAML file:
    * Version: always 0.0
    * OS: linux or windows
    * Files: config files or packages
    * Hooks: lifecycle event hooks. Some are:
        * BeforeInstall
        * AfterInstall
        * ApplicationStart
        * ValidateService
* Root folder contains appspec.yml, subfolders, e.g., Scripts, Config, Source


### Lifecycle Event Hooks



* For in-place deployments
    * Phase 1: De-registering
    * Phase 2: Deploy
    * Phase 3: Re-registering
* Phase 1
    * BeforeBlockTraffic: run before de-registering
    * BlockTraffic: de-registering
    * AfterBlockTraffic: after de-registering
* Phase 2
    * ApplicationStop: scripts for gracefully stopping the application
    * DownloadBundle: CodeDeploy agent copies application revision files
    * BeforeInstall: pre-installation scripts
    * Install: copy files to final location
    * PostInstall: post-install scripts (e.g. permissions)
    * ApplicationStart: start services that were stopped
    * ValidateService: run tests to validate service
* Phase 3
    * BeforeAllowTraffic
    * AllowTraffic
    * AfterAllowTraffic


## CodePipeline



* Automated releases
* Automates Codecommit, Codebuild, and Codedeploy
* Triggered every time there is a change to code


## CodeArtifact



* Artifact repository to store, publish, share software packages
    * documentation
    * compiled applications
    * deployable packages
    * libraries
* Integrates with public repos like npm, pypi
    * IT-approved packages/versions
* Integrates with CI/CD (e.g., CodeBuild)


### Public repo integration



* Create a "domain"
* Create a repo in the domain
* Create upstream repo
    * This is what connects to the public repo via External Connection
* Everything encrypted in transit and at rest


## Elastic Container Service (ECS)



* Containers are standardized virtual operating environments
* Highly scalable, stateless, fault tolerant
* Useful for microservices
    * 12-factor best practices (12factor.net)
* Docker or Windows
* Fargate - serverless
* ECS managed clusters
* EC2 instances


### Elastic Container Registry



* Docker images are published here. ECS pulls the images


## CloudFormation



* Infrastructure as code (templates)
* Templates support YAML or JSON
    * Uploaded to CF via S3
* Templates can be version controlled
* Resulting resources called a "stack"
* Free to use (pay for resources only)
* Rollback capability
* Manages resource dependencies


### Template Structure



* TemplateFormatVersion: always 2010-09-09
* All sections are optional except for Resources
* Description
* Metadata
* Parameters
    * Input values
* Conditions
    * Base actions on conditions (e.g., environment is prod or dev)
* Mappings
    * E.g., set values based on region
* Transform
    * include code from outside of the template (e.g. CloudFormation Snippets or Lambda code)
* Resources
    * AWS resources that will be deployed
* Outputs
    * Displayed on console and can be inputs to another stack


### Serverless Application Model (SAM)



* Extension to CF
* Primarily for Lambda
* Simplified syntax
* CLI for packaging, uploading, deploying
    * sam package
    * sam deploy


### Nested Stacks



* Standard templates (e.g., a load balancer) that can be reused within other templates
* Declared within Resources section
* TemplateURL: S3 URL of nested stack


# Advanced IAM


## Web Identity Federation



* Users authenticate with web-based identity provider (e.g., Google)
* Users receive authentication token (JWT token)  from provider
* Can trade this token for temporary access to AWS resources


### Cognito



* Identity broker – manages auth between your app and web ID providers
    * Provides temp credentials that map to an IAM role
    * No additional code required
    * No need for app to locally store credentials
* Supports multiple devices
    * Can sync user data across devices
* Recommended approach for all mobile apps
* User pools
    * User directories to manage sign-up/sign-in
    * Users can sign in directly to the pool or web identity provider
    * Can enable MFA
* Identity pools
    * Provides temporary credentials upon receiving JWT token
    * These credentials map to an IAM role
* Push synchronization
    * Uses silent SNS notifications to push updates to multiple devices


## Advanced IAM Policies



* AWS managed policies
    * For users, groups, and roles
    * For common use cases based on job function
    * You can't modify these
* Customer managed policies
    * For users, groups, and roles
    * Can start by copying an AWS managed then modifying
* Inline policies
    * 1:1 relationship between entity and policy
    * Policy is deleted when entity is deleted


## STS AssumeRoleWithWebIdentity API



* STS: Security Token Service
* Calls STS to get token from web identity provider
* Cognito makes this API call for you for mobile apps
* Within response AssumedRoleUser, Arn and AssumedRoleId can be used to programmatically refer to temp credentials


## Cross Account Access



* Create IAM role in one account to grant permissions to users in another account
    * This primary account role defines a trust relationship
    * The second account requires an IAM policy that allows the primary IAM account role to be assumed – sts:AssumeRole
    * The role can be assumed from the secondary account directly via the console via Switch Roles under the upper-right user dropdown
* Useful for working with prod account data from a dev account


# Monitoring


## Cloudwatch



* Can monitor most AWS services out of the box
* CloudWatch Agent to define your own custom metrics
* CloudWatch Logs to monitor operating system/application logs


### EC2



* By default: CPU, network, disk, and status checks
    * But no OS-level metrics
    * Sends every 5 minutes by default
    * For extra charge, can go down to 1 minute 
* Install CloudWatch agent for OS-level metrics
    * E.g., memory usage, disk space, CPU idle time, etc

### Metrics



* Time series with optional value sent to CloudWatch
* Uniquely defined by name, namespace, and 0 or more dimensions
* Namespace is a "container" for metrics. E.g., EC2 uses the "AWS/EC2" namespace
    * Can create your own
    * Each namespace is completely isolated from others
* Dimensions are like filters (e.g., instanceId)
    * CloudWatch can aggregate across dimensions for some services (e.g., EC2)
* Can be added to CloudWatch Dashboards


### CloudWatch Actions



* CloudWatch API supports actions to publish, monitor, alert on various metrics
* PutMetricData
    * Publishes metric name, namespace, value, and timestamp
* PutMetricAlarm
    * Creates an alarm that alerts on threshold being reached


### CloudWatch Logs Insights



* Interactive query/analysis for CloudWatch logs
    * Filter, visualization, etc
* Bespoke query language
    * Console provides many examples


# CloudTrail



* Records events (API calls) related to management activity within AWS
    * E.g., creating/destroying resources
    * Delivers logs to S3
    * Can be integrated with CloudWatch Logs
        * Which means we could alert, e.g., on failed login attempts


## EventBridge



* Event-driven architecture that responds to changes in state
* Rules invoke appropriate Targets
* Targets initiate Actions
    * (e.g., shutting down an EC2 instance)
* Can also create rules for cron-like schedules
* EventBridge and Cloudwatch Rules use same underlying API
    * EventBridge is preferred


## Common HTTP Error Codes



* 4XX – client side errors
    * 400 Access denied
    * 403 Missing auth token
        * No valid X.509 certificate or access key ID
    * 404 Malformed query string
        * Always a client side error
        * File not found is most typical response message
* 5XX - server side errors
    * 500 Internal failure
        * Generic, unknown internal error
    * 503 Service Unavailable
        * Server is not responding or is failing


# Practice Exam Notes



* Use multipart uploads for files larger than 100MB
* Parameter store does not rotate secrets. Secrets and Parameter Store both:
    * Integrate with IAM
    * Store hierarchical credentials
    * Support encryption at rest using customer KMS keys
* For CloudFront, set **viewer** protocol for secure encrypted requests via https
* If someone needs 1-time access to AWS, create an IAM user and delete it afterwards
* If a question contains REST, it's usually API Gateway
* S3 uses read-after-write consistency so uploaded objects are readable immediately
* Can't configure CPU capacity for Lambda
* If you need X-Ray in EC2, run it in its own container alongside other containers
* Serverless Application Model (SAM) is a simplified CloudFormation for serverless
* "Transformations" is not part of the CloudFormation template (but "transform" is)
* TLS Termination for encryption in transit in Elastic Load Balancer 
* CodePipeline has a manual approval feature
* Refreshing the DNS lookup is used in conjunction with exponential backoff for S3 requests
* Exponential backoff is almost always the answer if it appears
* Lambda can use custom runtimes (which can be useful for runtimes with additional packages, etc)
* Pre-signed URLs are useful for securely sharing S3 data
* API Gateway has a caching feature
* Time-ordered is a keyword with regard to DynamoDB streams. Streams are more useful for troubleshooting management events than CloudTrail
* Extended Client Library for Java SDK cannot create a new bucket and move messages to them; however, it can delete messages from S3
* KMS-CLI: generate-data-key is an important call
* There is an Amazon Encryption SDK – use this for client-side encryption
* Trusted Advisor is about best practices. Policy simulator is for everything to do with policies and permissions
* `amz-server-side-encryption:&lt;type>` can be required in headers sent to S3 for encryption in transit
* Use Cognito for tracking user devices
* ELB doesn't do session management (but ElasticCache does)
* Lambdas must be configured to access VPC in order to access resources in a private subnet (but now there are in-VPC features as well as Lambda@Edge)
* String, Number, and Binary can be used as sort keys in DynamoDb
* Use DeletionPolicy in CloudFormation to deal with deletion of resources
* EC2 instances must be running a CodeDeploy agent to use CodeDeploy – this agent is what will pull from CodeCommit. CodeDeploy doesn't do the pull
* DynamoDb is low latency but it isn't used to REDUCE latency – use caching for that
* Use exponential backoff for 5xx errors. Exponential backoff is almost ALWAYS the right answer when it appears
* S3 cross region replication requires bucket versioning – buckets are region-specific, only bucket NAMES are region agostic