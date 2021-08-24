---
title: AWS ML Specialty
description: AWS has a certification for Machine Learning specialty. 
published: true
date: 2021-08-24T19:44:03.684Z
tags: 
editor: markdown
dateCreated: 2021-08-16T16:47:39.772Z
---

# Exam Information

## Background
The AWS Certified Machine Learning - Specialty certification is intended for individuals who perform a development or data science role. It validates a candidate's ability to design, implement, deploy, and maintain machine learning (ML) solutions for given business problems.

### Exam Outline
The exam is 65 questions in length which must be answered within 190 minutes.  Scores are provided on a scale from 100, 1000, with 750 the minimum passing score.  The exam is available for taking remotely, but proctoring is required and provided.  There are no breaks (so manage your liquids!), and no resources can be used, online or otherwise, during the exam.  

There are two question types on the exam, multiple choice, and multiple response.  For multiple response questions, the number of responses expected is provided  (i.e., select 2 of 5 choices).  

The exam is organized into four key topics, with several objectives under each topic, as outlined below.  

**Domain 1: Data Engineering (20%)**
- **1.1** Create data repositories for machine learning.
- **1.2** Identify and implement a data-ingestion solution.
- **1.3** Identify and implement a data-transformation solution.

**Domain 2: Exploratory Data Analysis (24%)**
- **2.1** Sanitize and prepare data for modeling.
- **2.2** Perform feature engineering.
- **2.3** Analyze and visualize data for machine learning.

**Domain 3: Modeling (36%)**
- **3.1** Frame business problems as machine learning problems.
- **3.2** Select the appropriate model(s) for a given machine learning problem.
- **3.3** Train machine learning models.
- **3.4** Perform hyperparameter optimization.
- **3.5** Evaluate machine learning models.

**Domain 4: Machine Learning Implementation and Operations (20%)**
- **4.1** Build machine learning solutions for performance, availability, scalability, resiliency, and fault tolerance.
- **4.2** Recommend and implement the appropriate machine learning services and features for a given problem.
- **4.3** Apply basic AWS security practices to machine learning solutions.
- **4.4** Deploy and operationalize machine learning solutions.

More details on the structure of the exam are available via [AWS's provided Exam Guide](https://d1.awsstatic.com/training-and-certification/docs-ml/AWS-Certified-Machine-Learning-Specialty_Exam-Guide.pdf).


## Recommended Learning Path
**Learning ML**
[Elements of Data Science](https://www.aws.training/Details/eLearning?id=26598) - (8 hours)
[Developing Machine Learning Applications](https://www.aws.training/Details/Curriculum?id=27243) - (2.5 hours)
[Machine Learning Exam Basics](https://www.aws.training/Details/Curriculum?id=27271) - (2 hours)

**Exam Specific**
[Exam Readiness Digital Course](https://www.aws.training/Details/eLearning?id=42183) - (4.5 hours)

**Important APIs**
[SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html "KNOW THIS!") - (as many hours as it takes)
[SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/overview.html) - (high level understanding)
[All Other AWS AI Service APIs](https://aws.amazon.com/machine-learning/ai-services/) - (high level understanding)

**Important Blog Posts**
[SageMaker Local Mode](https://aws.amazon.com/blogs/machine-learning/use-the-amazon-sagemaker-local-mode-to-train-on-your-notebook-instance/)
[Scaling Hyperparameter Tuning on GPUs](https://aws.amazon.com/blogs/machine-learning/the-importance-of-hyperparameter-tuning-for-scaling-deep-learning-training-to-multiple-gpus/)
[S3 VPC Endpoints](https://tomgregory.com/when-to-use-an-aws-s3-vpc-endpoint/)

**Practice Tests**
[Udemy Practice Test](https://www.udemy.com/course/aws-machine-learning-practice-exam/ "65 questions")
[Braincert Practice Test](https://www.braincert.com/course/22419-AWS-Certified-Machine-Learning-%E2%80%93-Specialty-Practice-Exams "3 tests with 50 questions each and 1 test with 37 questions")
[Official AWS Certified Machine Learning - Specialty Practice (MLS-P01)](https://www.certmetrics.com/amazon/)

**Optional**
[Math for Machine Learning](https://www.aws.training/Details/eLearning?id=26597) - (8 hours)
[Linear and Logistic Regression](https://www.aws.training/Details/eLearning?id=26599) - (8.5 hours) 
  
# AWS Services 
## AWS Machine Learning Stack
![aws_ml_stack.png](/images/aws_ml_certification/aws_ml_stack.png)
The AWS ML stack is made up of three key layers of abstraction.  When building a machine learning solution, key priorities to balance may consist of:
- speed to market
- level of customization
- machine learning skill level

At the top layer, you have AI Services, which are “fully managed services for quickly adding machine learning capabilities to workloads using API calls”; ML knowledge is not required to utilize these.  

The middle layer, ML Services, is primarily made up of SageMaker related tools.  SageMaker spans the machine learning pipeline by offering solutions for labeling data as well as building, training and deploying machine learning models while largely abstracting away infrastructure maintenance requirements.  

The bottom layer, ML Frameworks + Infrastructure, contains the “closest to the metal” solutions, allowing machine learning practitioners to build machine learning solutions from the ground up.  With deep learning containers and images, AWS supports many common machine learning frameworks such as TensorFlow, PyTorch and Apache MXNet.
<p></br>

### AI Services
[Amazon Augmented AI](https://aws.amazon.com/augmented-ai/)
- machine learning service which makes it easy to build the workflows required for human review
<p></br>

[Amazon CodeGuru](https://aws.amazon.com/codeguru/)

[Amazon Comprehend](https://aws.amazon.com/comprehend/)
- natural language processing (NLP) service
- uses machine learning to find insights and relationships in text
- learn the sentiment hidden inside language (identifying negative reviews, or positive customer interactions with customer service agents), at almost limitless scale
<p></br>

[Amazon Comprehend Medical](https://aws.amazon.com/comprehend/medical/)

[Amazon Forecast](https://aws.amazon.com/forecast/)
- fully managed service that uses machine learning to deliver highly accurate forecasts
- combine time series data with additional variables to build forecasts
- no servers to provision and no machine learning models to build, train, or deploy
<p></br>

[Amazon Fraud Detector](https://aws.amazon.com/fraud-detector/)

[Amazon HealthLake](https://aws.amazon.com/healthlake/)

[Amazon Kendra](https://aws.amazon.com/kendra/)

[Amazon Lex](https://aws.amazon.com/lex/)

- service for building conversational interfaces into any application using voice and text (chatbots)
- advanced deep learning functionalities of automatic speech recognition (ASR) for converting speech to text, and natural language understanding (NLU) to recognize the intent of the text
- build applications with highly engaging user experiences and lifelike conversational interactions
<p></br>

[Amazon Lookout for Equipment](https://aws.amazon.com/lookout-for-equipment/)

[Amazon Lookout for Vision](https://aws.amazon.com/lookout-for-vision/)

[Amazon Monitron](https://aws.amazon.com/monitron/)

[Amazon Personalize](https://aws.amazon.com/personalize/)
- real-time personalized product and content recommendations, and targeted marketing promotions
- simple APIs to easily integrate sophisticated personalization capabilities into your systems and platform
- automates build, train, tune, and deploy a machine learning recommendation model so you can deliver personalized user experiences faster
- data is encrypted to be private and secure
<p></br>

[Amazon Polly](https://aws.amazon.com/polly/)
- Text-to-Speech (TTS) service uses advanced deep learning technologies to synthesize natural sounding human speech
- turns text into lifelike speech
- create applications that talk, and build entirely new categories of speech-enabled products
- dozens of lifelike voices across a broad set of languages
<p></br>

[Amazon Rekognition](https://aws.amazon.com/rekognition/)
- add image and video analysis to your applications using proven, highly scalable, deep learning technology
- requires no machine learning expertise
- identify objects, people, text, scenes, and activities in images and videos, as well as detect any inappropriate content
- highly accurate facial analysis and facial search capabilities that you can use to detect, analyze, and compare faces for a wide variety of user verification, people counting, and public safety use cases
<p></br>

[Amazon Textract](https://aws.amazon.com/textract/)
- automatically extracts text and data from scanned documents to identify, understand, and extract data from forms and tables
- fully managed machine learning service
- goes beyond simple optical character recognition (OCR) 
- instantly read and process any type of document, accurately extracting text, forms, tables and, other data without the need for any manual effort or custom code
<p></br>

[Amazon Transcribe](https://aws.amazon.com/transcribe/)
- deep learning process called automatic speech recognition (ASR) to convert speech to text quickly and accurately
- add speech to text capability to applications
- recorded speech often needs to be converted to text before it can be used in applications
- transcribe customer service calls, automate closed captioning and subtitling, and to generate metadata for media assets to create a fully searchable archive
- Amazon Transcribe Medical to add medical speech to text capabilities to clinical documentation applications
<p></br>

[Amazon Translate](https://aws.amazon.com/translate/) 
- delivers fast, high-quality, and affordable language translation
- uses deep learning models to deliver more accurate and more natural sounding translation than traditional statistical and rule-based translation algorithms
- localize content - such as websites and applications - for international users
- translate large volumes of text efficiently and easily
<p></br>

[AWS Panorama](https://aws.amazon.com/panorama/)
- allows organizations to bring computer vision (CV) to on-premises cameras to make predictions locally with high accuracy and low latency
<p></br>

### ML Services
[SageMaker](https://aws.amazon.com/sagemaker/)
SageMaker is a fully managed service, empowering developers and data scientists to process, build, train and deploy machine models quickly and efficiently.  All components required for machine learning are included in a single toolset so models get to production faster, with less effort, and at lower cost.  

![sagemaker_workflow.png](/images/aws_ml_certification/sagemaker_workflow.png)

**Prepare**
[Data Wrangler](https://aws.amazon.com/sagemaker/data-wrangler/)
[Feature Store](https://aws.amazon.com/sagemaker/feature-store/)
[**Ground Truth**](https://aws.amazon.com/sagemaker/groundtruth/)
- fully managed data labeling service
- workflows support a variety of use cases including 3D point clouds, video, images, and text
- labelers have access to assistive labeling features such as automatic 3D cuboid snapping, removal of distortion in 2D images, and auto-segment tools to reduce the time required to label datasets
- offers automatic data labeling which uses a machine learning model to label your data

[Clarify](https://aws.amazon.com/sagemaker/clarify/)

**Build**
[**Studio**](https://aws.amazon.com/sagemaker/studio/)
- first fully integrated development environment (IDE) for machine learning (ML)
- unifies at last all the tools needed for ML development
- developers can write code, track experiments, visualize data, and perform debugging and monitoring all within a single, integrated visual interface, significantly boosting developer productivity
- quickly move back and forth between steps, and also clone, tweak, and replay them.
- gives developers the ability to make changes quickly, observe outcomes, and iterate faster, reducing the time to market for high quality ML solutions

[**Autopilot**](https://aws.amazon.com/sagemaker/autopilot/)
- based on your data, automatically trains and tunes the best machine learning models for classification or regression 
- maintain full control and visibility
- provide a tabular dataset and select the target column to predict, which can be a number (such as a house price, called regression), or a category (such as spam/not spam, called classification)
- Autopilot will automatically explore different solutions to find the best model
- directly deploy the model to production with just one click, or iterate on the recommended solutions within Studio to further improve the model quality

[JumpStart](https://aws.amazon.com/sagemaker/getting-started/)

**Train & Tune**
[Debugger](https://aws.amazon.com/sagemaker/debugger/)
[Distributed Training](https://aws.amazon.com/sagemaker/distributed-training/)

**Deploy & Manage**
[Pipelines](https://aws.amazon.com/sagemaker/pipelines/)
[Model Monitor](https://aws.amazon.com/sagemaker/model-monitor/)
[Kubernetes Integration](https://aws.amazon.com/sagemaker/kubernetes/)
[Edge Manager](https://aws.amazon.com/sagemaker/edge-manager/)
[**Neo**](https://aws.amazon.com/sagemaker/neo/)
- enables developers to train machine learning models once and run them anywhere in the cloud and at the edge
- optimizes models to run up to twice as fast, with less than a tenth of the memory footprint, with no loss in accuracy
- automatically optimizes machine learning models to perform at up to twice the speed with no loss in accuracy

<br></br>
> The most commonly covered services on the exam are linked below, but others may be worthwhile to skim. However, the most important documentation to study regarding SageMaker is the [developer guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html). 
>
> [Ground Truth](https://aws.amazon.com/sagemaker/groundtruth/) 
> [Autopilot](https://aws.amazon.com/sagemaker/autopilot/)
> [Distributed Training](https://aws.amazon.com/sagemaker/distributed-training/)
> [Neo](https://aws.amazon.com/sagemaker/neo/)
{.is-info}

### Frameworks
[TensorFlow on AWS](https://aws.amazon.com/tensorflow/)
[PyTorch on AWS](https://aws.amazon.com/pytorch/)
[Apache MXNet on AWS](https://aws.amazon.com/mxnet/)
[Hugging Face on Amazon SageMaker](https://aws.amazon.com/machine-learning/hugging-face/)
<p></br>

### Infrastructure

[AWS DL AMIs](https://aws.amazon.com/machine-learning/amis/)
- AWS Deep Learning AMIs provide machine learning practitioners and researchers with the infrastructure and tools to accelerate deep learning in the cloud, at any scale. You can quickly launch Amazon EC2 instances pre-installed with popular deep learning frameworks and interfaces such as TensorFlow, PyTorch, Apache MXNet, Chainer, Gluon, Horovod, and Keras to train sophisticated, custom AI models, experiment with new algorithms, or to learn new skills and techniques.
- Whether you need Amazon EC2 GPU or CPU instances, there is no additional charge for the Deep Learning AMIs – you only pay for the AWS resources needed to store and run your applications.
<p></br>

[AWS DL Containers](https://aws.amazon.com/machine-learning/containers/)
- Docker images pre-installed with deep learning frameworks to make it easy to deploy custom machine learning (ML) environments quickly letting you skip the complicated process of building and optimizing your environments from scratch
- containers are available through Amazon Elastic Container Registry (Amazon ECR) and AWS Marketplace at no cost
- supports popular frameworks
<p></br>

[Elastic Container Service (ECS)](https://aws.amazon.com/ecs/)
- fully managed container orchestration service that helps you easily deploy, manage, and scale containerized appplications
- choose to run your ECS clusters using AWS Fargate, which is serverless compute for containers
- used to power services such as Amazon SageMaker, AWS Batch, Amazon Lex, and Amazon.com’s recommendation engine, ensuring ECS is tested extensively for security, reliability, and availability
<p></br>

[Elastic Inference](https://aws.amazon.com/machine-learning/elastic-inference/)
- attach low-cost GPU-powered acceleration to Amazon EC2 and Sagemaker instances or Amazon ECS tasks
- reduce the cost of running deep learning inference by up to 75%
- supports TensorFlow, Apache MXNet, PyTorch and ONNX models
- attach just the right amount of GPU-powered inference acceleration to any EC2 or SageMaker instance type or ECS task, with no code changes
- choose any CPU instance in AWS that is best suited to the overall compute and memory needs of your application, and then separately configure the right amount of GPU-powered inference acceleration, allowing you to efficiently utilize resources and reduce costs
<p></br>

[IoT Greengrass](https://aws.amazon.com/greengrass/)
- seamlessly extends AWS to edge devices so they can act locally on the data they generate, while still using the cloud for management, analytics, and durable storage
- connected devices can run AWS Lambda functions, Docker containers, or both, execute predictions based on machine learning models, keep device data in sync, and communicate with other devices securely – even when not connected to the Internet.
- use familiar languages and programming models to create and test your device software in the cloud, and then deploy it to your devices
<p></br>

[Inferentia](https://aws.amazon.com/machine-learning/inferentia/)
- custom silicon designed to accelerate deep learning workloads
- designed to provide high performance inference in the cloud, to drive down the total cost of inference, and to make it easy for developers to integrate machine learning into their business applications
- AWS Neuron software development kit (SDK), consisting of a compiler, run-time, and profiling tools that help optimize the performance of workloads for AWS Inferentia, enables complex neural net models, created and trained in popular frameworks such as Tensorflow, PyTorch, and MXNet, to be executed using AWS Inferentia-based Amazon EC2 Inf1 instances
<p></br>

[AWS Auto Scaling](https://aws.amazon.com/autoscaling/)
It monitors your applications and automatically adjusts capacity to maintain steady, predictable performance at the lowest possible cost. Using AWS Auto Scaling, it’s easy to setup application scaling for multiple resources across multiple services in minutes. The service provides a simple, powerful user interface that lets you build scaling plans for resources including Amazon EC2 instances and Spot Fleets, Amazon ECS tasks, Amazon DynamoDB tables and indexes, and Amazon Aurora Replicas. AWS Auto Scaling makes scaling simple with recommendations that allow you to optimize performance, costs, or balance between them. If you’re already using Amazon EC2 Auto Scaling to dynamically scale your Amazon EC2 instances, you can now combine it with AWS Auto Scaling to scale additional resources for other AWS services. With AWS Auto Scaling, your applications always have the right resources at the right time.

[Instance types](https://aws.amazon.com/ec2/instance-types/):
- **T:** most basic
- **M:** more memory, cores 
- **C:** compute optimized - ideal for compute bound applications that benefit from high performance processors. Instances belonging to this family are well suited for batch processing workloads, media transcoding, high performance web servers, high performance computing (HPC), scientific modeling, dedicated gaming servers and ad server engines, machine learning inference and other compute intensive applications
- **P:** GPU optimized - accelerated computing instances use hardware accelerators, or co-processors, to perform 		functions, such as floating point number calculations, graphics processing, or data pattern matching, more efficiently than is possible in software running on CPUs
<p></br>

## AWS Data Engineering Stack

| Concepts          | Description | 
| -----------       | ----------- | 
| **High Availability** | System will continue to function even when any component of the architecture stops working | 
| **Fault Tolerance** | Ensures that applications will continue to function without degradation in performance, despite complete failure of any component of the architecture | 
| **Loose Coupling** | In a distributed system, failure of one component can be managed in between application tiers so that the faults do not spread beyond that single point achieved by making sure application components are independent of each other. Read up on queues: [Amazon SQS](https://aws.amazon.com/sqs/) and [AWS Step Functions](https://aws.amazon.com/sqs/). | 
| **CI/CD** |  Bridges the gaps between development and operation activities and teams by enforcing automation in building, testing and deployment of applications. Modern day DevOps practices involve continuous development, continuous testing, continuous integration, continuous deployment and continuous monitoring of software applications throughout its development life cycle.  | 
| **Canary Deployment Methodology** | Pattern for rolling out releases to a subset of users or servers. The idea is to first deploy the change to a small subset of servers, test it, and then roll the change out to the rest of the servers. The canary deployment serves as an early warning indicator with less impact on downtime: if the canary deployment fails, the rest of the servers aren't impacted.| 

<br></br>
### Compiling
[Amazon Mechanical Turk](https://www.mturk.com/)
- crowdsourcing marketplace that makes it easier for individuals and businesses to outsource their processes and jobs to a distributed workforce who can perform these tasks virtually
- anything from conducting simple data validation and research to more subjective tasks like survey participation, content moderation, and more
- enables companies to harness the collective intelligence, skills, and insights from a global workforce to streamline business processes, augment data collection and analysis, and accelerate machine learning development

### Streaming
[**Amazon Kinesis**](https://aws.amazon.com/kinesis/)
Kinesis facilitates the streaming of data, making it easy to collect, process, and analyze data real-time for timely insights, speeding reaction time.  Video, audio, application logs, clickstreams, and IoT data can be ingested real-time.  
![kinesis_data_streams.png](/images/aws_ml_certification/kinesis_data_streams.png)
<p></br>

| Kinesis Capability   | Description | 
| -----------  | ----------- | 
| [**Amazon Kinesis Data Streams**](https://aws.amazon.com/kinesis/data-streams/) |  <ul><li>massively scalable and durable real-time data streaming service</li><li>continuously capture gigabytes of data per second from hundreds of thousands of sources such as website clickstreams, database event streams, financial transactions, social media feeds, IT logs, and location-tracking events</li> <li>data collected is available in milliseconds to enable real-time analytics use cases such as real-time dashboards, real-time anomaly detection, dynamic pricing, and more</li></ul>  <table>  <thead>  <tr>  <th>Component</th>  <th>Description</th>  </tr>  </thead>  <tbody>  <tr>  <td>[Kinesis Producer Library (KPL)](https://docs.aws.amazon.com/streams/latest/dev/developing-producers-with-kpl.html)</td> <td>intermediary between producer application code and KDS API data, to write to a Kinesis data stream</td> </tr>  <tr>  <td>[Kinesis Client Library (KCL)](https://docs.aws.amazon.com/streams/latest/dev/shared-throughput-kcl-consumers.html)</td>  <td>build own application to preprocess the streaming data as it arrives and emit data for generating incremental views and downstream analysis</td> </tr> </table> | 
  [**Amazon Kinesis Video Streams**](https://aws.amazon.com/kinesis/data-streams/) | <ul><li>securely stream video from connected devices to AWS for analytics, machine learning, playback, and other processing</li> <li>automatically provisions and elastically scales all the infrastructure needed to ingest streaming video data from millions of devices</li> <li>durably stores, encrypts, and indexes video data in your streams, and allows you to access your data through easy-to-use APIs</li></ul> | 
| [**Amazon Kinesis Data Firehose**](https://aws.amazon.com/kinesis/data-streams/) | <ul><li>reliably load streaming data into data lakes, data stores, and analytics services</li> <li>capture, transform ([lambda](https://aws.amazon.com/lambda/)) , and deliver streaming data to Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, generic HTTP endpoints, and service providers like Datadog, New Relic, MongoDB, and Splunk</li> <li>fully managed service that automatically scales to match the throughput of your data and requires no ongoing administration</li> <li>batch, compress, transform, and encrypt your data streams before loading, minimizing the amount of storage used and increasing security</li> <li>cannot ingest csv</li></ul> | 
| [**Amazon Kinesis Data Analytics**](https://aws.amazon.com/kinesis/data-streams/) | <ul><li>transform and analyze streaming data in real time with Apache Flink (SQL), an open source framework and engine for processing data streams</li> <li>reduces the complexity of building, managing, and integrating Apache Flink applications with other AWS services</li> <li>takes care of everything required to run streaming applications continuously, and scales automatically to match the volume and throughput of your incoming data; no servers to manage, no minimum fee or setup cost, and you only pay for the resources your streaming applications consume</li></ul> | 

[Apache Kafka](https://aws.amazon.com/kinesis/data-streams/)
- open-source platform for building real-time streaming data pipelines and applications
- used by thousands of companies for high-performance data pipelines, streaming analytics, data integration, and mission-critical applications

[Apache Kafka (MSK)](https://aws.amazon.com/msk/)
- fully managed service that makes it easy for you to build and run applications that use Apache Kafka to process streaming data
- use native Apache Kafka APIs to populate data lakes, stream changes to and from databases, and power machine learning and analytics applications
- build and run production applications on Apache Kafka without needing Apache Kafka infrastructure management expertise; spend less time managing infrastructure and more time building applications

### Storage
[AWS Lake Formation](https://aws.amazon.com/lake-formation/)
- data lake is a centralized, curated, and secured repository that stores all your data, both in its original form and prepared for analysis
- creating a data lake with Lake Formation is as simple as defining data sources and what data access and security policies you want to apply

[**Amazon Simple Storage Solution (S3)**](https://aws.amazon.com/s3/)
Storage for a data lake
![s3_bucket_access.png](/images/aws_ml_certification/s3_bucket_access.png)
| [Storage Class](https://aws.amazon.com/s3/storage-classes/) | Description |
| --------- | ----------- |
| Standard | frequent access of data |
| Intelligent-Tiering | automatically optimizes storage costs for data with changing access patterns |
| Standard-Infrequent Access | standard with less access needed |
| One Zone-Infrequent Access | lower cost with less access and stores in one-zone |
| Glacier | cheap for slow reada |
| Glacier Deep Archive | slowest method |
| Outpost | on prem |

[Amazon FSx for Lustre](https://aws.amazon.com/s3/)
- open-source highly scalable, highly distributed, and highly parallel file system for compute workloads like deep learning
- When your training data is already in Amazon S3 and you plan to run training jobs several times using different algorithms and parameters, consider using Amazon FSx for Lustre, a file system service
- speeds up your training jobs by serving your Amazon S3 data to Amazon SageMaker at high speeds
- first time you run a training job, FSx for Lustre automatically copies data from Amazon S3 and makes it available to Amazon SageMaker
- use the same Amazon FSx file system for subsequent iterations of training jobs, preventing repeated downloads of common Amazon S3 objects

[Amazon Elastic File System (EFS)](https://aws.amazon.com/efs/)
- serverless data storage without provisioning or managing storage
- useful when running batch processing on central locations
- directly launch your training jobs from the service without the need for data movement, resulting in faster training start times

[Amazon Elastic Block Storage (EBS)](https://aws.amazon.com/ebs/)
- persistent storage linked to a EC2 instance
- easy to use, high performance block storage service
- designed for use with Amazon Elastic Compute Cloud (EC2) for both throughput and transaction intensive workloads at any scale
- A broad range of workloads, such as relational and non-relational databases, enterprise applications, containerized applications, big data analytics engines, file systems, and media workflows are widely deployed on Amazon EBS

### ETL
[AWS Glue](https://aws.amazon.com/glue/)
- fully managed extract, transform, and load (ETL) service that makes it easy for customers to prepare and load their data for analytics
- point AWS Glue to your data stored on AWS, and AWS Glue discovers your data and stores the associated metadata (e.g. table definition and schema) in the AWS Glue Data Catalog
- Once cataloged, your data is immediately searchable, queryable, and available for ETL

### Distributed Computing
[Amazon Elastic Map Reduce (EMR)](https://aws.amazon.com/emr/)
- industry-leading cloud big data platform for processing vast amounts of data using open source tools such as Apache Spark, Apache Hive, Apache HBase, Apache Flink, Apache Hudi, and Presto
- run Petabyte-scale analysis at less than half of the cost of traditional on-premises solutions and over 3x faster than standard Apache Spark

[Apache Spark](https://spark.apache.org/)
- distributed processing framework and programming model that helps you do machine learning, stream processing, or graph analytics using Amazon EMR clusters
- open-source, distributed processing system commonly used for big data workloads 
- optimized directed acyclic graph (DAG) execution engine and actively caches data in-memory, which can boost performance, especially for certain algorithms and interactive queries (in contrast to Hadoop)

### Containers
[Amazon Elastic Container Service (ECS)](https://aws.amazon.com/ecs/)
- fully managed container orchestration service
- Duolingo, Samsung, GE, and Cookpad use ECS to run their most sensitive and mission critical applications because of its security, reliability, and scalability
- choose to run your ECS clusters using AWS Fargate, which is serverless compute for containers
- used to power services such as Amazon SageMaker, AWS Batch, Amazon Lex, and Amazon.com’s recommendation engine, ensuring ECS is tested extensively for security, reliability, and availability

[Amazon Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/)
Amazon Elastic Kubernetes Service (Amazon EKS) is a fully managed Kubernetes service. Customers such as Intel, Snap, Intuit, GoDaddy, and Autodesk trust EKS to run their most sensitive and mission critical applications because of its security, reliability, and scalability.

[Amazon Elastic Container Registry (ECR)](https://aws.amazon.com/ecr/)
- fully-managed Docker container registry that makes it easy for developers to store, manage, and deploy Docker container images
- integrated with Amazon Elastic Container Service (ECS), simplifying your development to production workflow
- eliminates the need to operate your own container repositories or worry about scaling the underlying infrastructure
- hosts your images in a highly available and scalable architecture, allowing you to reliably deploy containers for your applications

[AWS Fargate](https://aws.amazon.com/fargate/)
- serverless compute engine for containers that works with both Amazon Elastic Container Service (ECS) and Amazon Elastic Kubernetes Service (EKS)
- removes the need to provision and manage servers, lets you specify and pay for resources per application, and improves security through application isolation by design
- allocates the right amount of compute, eliminating the need to choose instances and scale cluster capacity
- runs each task or pod in its own kernel providing the tasks and pods their own isolated compute environment, enabling application to have workload isolation and improved security by design

### Orchestration
[AWS Step Functions](https://aws.amazon.com/sqs/)
- serverless function orchestrator that makes it easy to sequence AWS Lambda functions and multiple AWS services into business-critical applications
- create and run a series of checkpointed and event-driven workflows that maintain the application state
- output of one step acts as input into the next
- Each step in your application executes in order and as expected based on your defined business logic
- automatically manages error handling, retry logic, and state; with built-in operational controls, Step Functions manages sequencing

[AWS Lambda](https://aws.amazon.com/lambda/)
- run code without provisioning or managing servers; pay only for the compute time you consume
- upload your code and Lambda takes care of everything required to run and scale your code with high availability
- set up your code to automatically trigger from other AWS services or call it directly from any web or mobile app

[AWS Batch](https://aws.amazon.com/batch/)
- easily and efficiently run hundreds of thousands of batch computing jobs on AWS
- dynamically provisions the optimal quantity and type of compute resources (e.g., CPU or memory optimized instances) based on the volume and specific resource requirements of the batch jobs submitted
- no need to install and manage batch computing software or server clusters that you use to run your jobs, allowing you to focus on analyzing results and solving problems
- plans, schedules, and executes your batch computing workloads across the full range of AWS compute services and features, such as Amazon EC2 and Spot Instances


### Search
[ElasticSearch](https://aws.amazon.com/elasticsearch-service/)

- fully managed service that makes it easy for you to deploy, secure, and run Elasticsearch cost effectively at scale
- build, monitor, and troubleshoot your applications using the tools you love, at the scale you need
- service provides support for open source Elasticsearch APIs, managed Kibana, integration with Logstash and other AWS services, and built-in alerting and SQL querying

### Deployment
[SageMaker Model Deployment](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-deployment.html)


[Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/)
AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS.

[AWS DeepLens](https://aws.amazon.com/deeplens/)
Machine learning in the hands of developers, literally, with a fully programmable video camera, tutorials, code, and pre-trained models designed to expand deep learning skills

### Analytics
[Amazon Athena](https://aws.amazon.com/athena/)
- interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL
- serverless, no infrastructure to manage, and you pay only for the queries that you run
- point to your data in Amazon S3, define the schema, and start querying using standard SQL
- no need for complex ETL jobs to prepare your data for analysis
- out-of-the-box integrated with AWS Glue Data Catalog, allowing you to create a unified metadata repository across various services, crawl data sources to discover schemas and populate your Catalog with new and modified table and partition definitions, and maintain schema versioning


[Amazon QuickSight](https://aws.amazon.com/quicksight/)
- fast, cloud-powered business intelligence service that makes it easy to deliver insights to everyone in your organization.
- As a fully managed service, QuickSight lets you easily create and publish interactive dashboards that include ML Insights. Dashboards can then be accessed from any device, and embedded into your applications, portals, and websites.
- With the Pay-per-Session pricing, QuickSight allows you to give everyone access to the data they need, while only paying for what you use.

[Kibana](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-kibana.html)
- open-source data visualization and exploration tool used for log and time-series analytics, application monitoring, and operational intelligence use cases
- offers powerful and easy-to-use features such as histograms, line graphs, pie charts, heat maps, and built-in geospatial support
- provides tight integration with Elasticsearch, a popular analytics and search engine, which makes Kibana the default choice for visualizing data stored in Elasticsearch


### Monitoring
[Amazon Cloudwatch](https://aws.amazon.com/cloudwatch/)
- Monitor applications, including SageMaker
- collects monitoring and operational data in the form of logs, metrics, and events, providing you with a unified view of AWS resources, applications, and services that run on AWS and on-premises servers.
- detect anomalous behavior in your environments, set alarms, visualize logs and metrics side by side, take automated actions, troubleshoot issues, and discover insights to keep your applications running smoothly

[AWS CloudTrail](https://aws.amazon.com/cloudtrail/)
- captures API calls and related events
- enables governance, compliance, operational auditing, and risk auditing of your AWS account

[Amazon Simple Notification Service (SNS)](https://aws.amazon.com/sns/)
- fully managed messaging service for both system-to-system and app-to-person (A2P) communication
- enables communication between systems through publish/subscribe (pub/sub) patterns that enable messaging between decoupled microservice applications or to communicate directly to users via SMS, mobile push and email
- A2P messaging functionality enables you to send messages to users at scale using either a pub/sub pattern or direct-publish messages using a single API

### Security
[Identity Access and Management (IAM)](https://aws.amazon.com/iam/)
- manage access to AWS services and resources securely
- create and manage AWS users and groups, and use permissions to allow and deny their access to AWS resources

[Key Management Service (KMS)](https://aws.amazon.com/kms/)
- create and manage cryptographic keys and control their use across a wide range of AWS services and in your applications
- integrated with AWS CloudTrail to provide you with logs of all key usage to help meet your regulatory and compliance needs

[Virtual Private Cloud (VPC)](https://aws.amazon.com/vpc/)
- provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define
- You have complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways
- use both IPv4 and IPv6 in your VPC for secure and easy access to resources and applications

- [Security Groups:](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
	- acts as a virtual firewall for your instance to control inbound and outbound traffic
	- When you launch an instance in a VPC, you can assign up to five security groups to the instance
	- Security groups act at the instance level, not the subnet level; each instance in a subnet in your VPC can be assigned to a different set of security groups

- [Network Address Translation (NAT) gateways:](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
	- enable instances in a private subnet to connect to the internet or other AWS services, but prevent the internet from initiating a connection with those instances
  
- [Internet Gateways:](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
  - horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet
  - serves two purposes: 
  - provide a target in your VPC route tables for internet-routable traffic; and
  - perform network address translation (NAT) for instances that have been assigned public IPv4 addresses
  - supports IPv4 and IPv6 traffic; does not cause availability risks or bandwidth constraints on your network traffic
<p></br>

# Exam Material
## Data Engineering
### 1.1 Create data repositories for machine learning
> **For the exam you need to understand...**
>
> how to generate data with SageMaker Ground Truth, Mechanical Turk, Kinesis Data Analytics, and Kinesis Video Streams
> know tradeoffs between AWS storage solutions
{.is-info}

![storage_tradeoff.png](/images/aws_ml_certification/storage_tradeoff.png)
<br></br>
### 1.2 Identify and implement a data-ingestion solution
> **For the exam you need to understand...**
>
> the tradeoffs/capabilities between different ingestion solutions
>
> **Note:** DMS is specific to migration and legacy systems. Glue is generally best when you don't want to do much work, while step functions are good for flexibility and queing. For streaming, data Kafka is more work, but offers porting code. Kinesis is easy.
>
> *Tip: Much of this exam is finding the optimal solution and not just the right one for the specific business problem (simplest to implement, offer flexibility to port codebase, etc.)*
{.is-info}

**Batch Processing**
With batch processing, the ingestion layer periodically collects and groups source data and sends it to a destination like Amazon S3. You can process groups based on any logical ordering, the activation of certain conditions, or a simple schedule. Batch processing is typically used when there is no real need for real-time or near-real-time data, because it is generally easier and more affordably implemented than other ingestion options.

- AWS Glue 
- AWS DMS  
- AWS Step Functions


**Stream Processing**
Stream processing, which includes real-time processing, involves no grouping at all. Data is sourced, manipulated, and loaded as soon as it is created or recognized by the data ingestion layer. This kind of ingestion is less cost-effective, since it requires systems to constantly monitor sources and accept new information. But you might want to use it for real-time predictions using an Amazon SageMaker endpoint that you want to show your customers on your website or some real-time analytics that require continually refreshed data, like real-time dashboards 

- Kinesis
- Kafka


### 1.3 Identify and implement a data-transformation solution
The raw data ingested into a service like Amazon S3 is usually not ML ready as is. The data needs to be transformed and cleaned, which includes deduplication, incomplete data management, and attribute standardization. Data transformation can also involve changing the data structures, if necessary, usually into an OLAP model to facilitate easy querying of data.

Data transformation is often necessary to deal with huge amounts of data. Distributed computation frameworks like MapReduce and Apache Spark provide a protocol of data processing and node task distribution and management. They also use algorithms to split datasets into subsets and distribute them across nodes in a compute cluster.

Using Apache Spark on Amazon EMR provides a managed framework that can process massive quantities of data. Amazon EMR supports many instance types that have proportionally high CPU with increased network performance, which is well suited for HPC (high-performance computing) applications.

In the case of streaming data, Kinesis Firehose can transform data before storage using AWS Lambda. 

## Exploratory Data Analysis
### 2.1 Sanitize and prepare data for modeling
> **For the exam you need to understand...**
>
> what to do when you have imbalanced data
> what to do with variable that are highly correlated
> how to handle missing values and outliers
> how to clean text data
> how to handle scaling and normalizing tabular and image
> SageMaker specific practices (formats, labels, pipe vs file, local mode, etc.)
{.is-info}

<br></br>
### 2.2 Perform feature engineering
> **For the exam you need to understand...**
>
> why and how to augment data in both tabular, image, and text form
> how to handle scaling and normalizing tabular and image
> how to treat categorical features (one-hot encoding vs ordinality)
> techniques for dimensionality reduction (PCA and LDA)
{.is-info}

<br></br>
### 2.3 Analyze and visualize data for machine learning
**Scatter Plots**
![scatter_plots.png](/images/aws_ml_certification/scatter_plots.png =650x)
<br></br>

**Histograms**
![histogram.png](/images/aws_ml_certification/histogram.png =650x)
<br></br>

**Scatter Matrix**
![scatter_matrix.png](/images/aws_ml_certification/scatter_matrix.png =750x)
<br></br>

**Correlation Matrix**
![correlation_matrix.png](/images/aws_ml_certification/correlation_matrix.png =650x)
<br></br>

**Confusion Matrix**
![confusion_matrix.png](/images/aws_ml_certification/confusion_matrix.png =450x)
<br></br>

## Modeling
### 3.1 Frame business problems as machine learning problems
> **For the exam you need to understand...**
>
> all the built in SageMaker models
>
> [**Read:** Built-in SageMaker Models](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)
{.is-info}

| Problem Type | Definition |
| ------------ | -------- | ---------- |
| Supervised learning | making predictions on a labeled set | 
| Unsupervised learning | making predictions on an unlabeled set| 
| Regression | predicting a continous target  | 
| Classification | predicting a discrete target  | 
| Clustering | grouping points together to make predictions |  
| Anomaly detection | detecting outliers to make predictions| 

<br></br>
### 3.2 Select the appropriate model(s) for a given machine learning problem
**Anomaly Detection**
Random Cut Forest

**Classification Models**
KNN

**Regression/Classification Models**
Linear Learner
XGBoost

**Natural Language Processing**
BlazingText
Sequence2Sequence
Object2Vec
LSTM
BERT
RNN

**Computer Vision**
CNN
Semantic Segmentation
<br></br>

### 3.3 Train machine learning models
> **For the exam you need to understand...**
>
> how to train models with SageMaker
{.is-info}

<br></br>
### 3.4 Perform hyperparameter optimization
> **For the exam you need to understand...**
>
> how to optimize hyperparameters for neural networks and tree-based models
> what to do when the model isn't working (local minima, not coverging, etc)
{.is-info}

<br></br>
### 3.5 Evaluate machine learning models
> **For the exam you need to understand...**
>
> all evaluation metrics for classification and regressions (RSME, precision, accuracy, confusion matricies, etc.)
> all visualizations for evaluation (AUC ROC, AUPR, residual plots, etc)
> which metrics to use when you have an imbalanced dataset
{.is-info}

<br></br>
## ML Implementation and Operations
### 4.1 Build machine learning solutions for performance, availability, scalability, resiliency, and fault tolerance

### 4.2 Recommend and implement the appropriate machine learning services and features for a given problem
> **For the exam you need to understand...**
>
> and memorize all the entire AWS AI and ML stack (not joking)
{.is-info}

<br></br>
### 4.3 Apply basic AWS security practices to machine learning solutions
> **For the exam you need to understand...**
>
> VPCs and NAT/Internet gatewaya  in depth (this has increasing become more tested like almost 10% of the test)
> security practices involving KMS and IAM
{.is-info}

[S3 VPC Endpoints](https://tomgregory.com/when-to-use-an-aws-s3-vpc-endpoint/)
<br></br>
### 4.4 Deploy and operationalize machine learning solutions
# Appendix
## Links
**Vetted Practice Tests**
[Udemy Practice Test](https://www.udemy.com/course/aws-machine-learning-practice-exam/ "65 questions")
[Braincert Practice Test](https://www.braincert.com/course/22419-AWS-Certified-Machine-Learning-%E2%80%93-Specialty-Practice-Exams "3 tests with 50 questions each and 1 test with 37 questions")
[Official AWS Certified Machine Learning - Specialty Practice Test (MLS-P01)](https://www.certmetrics.com/amazon/)

**Important Nibbles of Info**
[Firehose Data Formats](https://docs.aws.amazon.com/firehose/latest/dev/record-format-conversion.html)
[SageMaker Endpoint Scaling](https://docs.aws.amazon.com/firehose/latest/dev/record-format-conversion.html)
[SageMaker and VPCs](https://docs.aws.amazon.com/sagemaker/latest/dg/appendix-notebook-and-internet-access.html)
[Scaling and Normalization of Data](https://towardsai.net/p/data-science/how-when-and-why-should-you-normalize-standardize-rescale-your-data-3f083def38ff)


**Additional study guides**
[Brian McMahon's Study Guide](https://docs.google.com/document/d/1B7m0sxJOVedQZeHNRdpB_y0sBEzBGVrb8nENuM07P_k/edit?usp=sharing "Great study guide with plenty of information that's not included in the AWS webpages. Much of the info here is pulled from there.")