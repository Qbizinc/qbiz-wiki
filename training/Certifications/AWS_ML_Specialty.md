---
title: AWS ML Specialty
description: AWS has a certification for Machine Learning specialty. 
published: true
date: 2021-08-19T22:40:05.275Z
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
  
# Exam Material 
## AWS ML Stack
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


> The most commonly covered services on the exam are linked below, but others may be worthwhile to skim. However, the most important documentation to study regarding SageMaker is the [developer guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html). 
>
> [Ground Truth](https://aws.amazon.com/sagemaker/groundtruth/) 
> [Autopilot](https://aws.amazon.com/sagemaker/autopilot/)
> [Distributed Training](https://aws.amazon.com/sagemaker/distributed-training/)
> [Neo](https://aws.amazon.com/sagemaker/neo/)
<p></br>

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

## Data Engineering
### 1.1 Create data repositories for machine learning
### 1.2 Identify and implement a data-ingestion solution
### 1.3 Identify and implement a data-transformation solution
## Exploratory Data Analysis
### 2.1 Sanitize and prepare data for modeling
### 2.2 Perform feature engineering
### 2.3 Analyze and visualize data for machine learning
## Modeling
### 3.1 Frame business problems as machine learning problems
### 3.2 Select the appropriate model(s) for a given machine learning problem
**Classification Models**
KNN

**Regression/Classification Models**
Linear Learner
XGBoost

**Natural Language Processing**
BlazingText
Sequence2Sequence
Object2Vec

**Computer Vision**
Semantic Segmentation
<p></br>

### 3.3 Train machine learning models
### 3.4 Perform hyperparameter optimization
### 3.5 Evaluate machine learning models
## ML Implementation and Operations
### 4.1 Build machine learning solutions for performance, availability, scalability, resiliency, and fault tolerance
### 4.2 Recommend and implement the appropriate machine learning services and features for a given problem
### 4.3 Apply basic AWS security practices to machine learning solutions
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


**Additional study guides**
[Brian McMahon's Study Guide](https://docs.google.com/document/d/1B7m0sxJOVedQZeHNRdpB_y0sBEzBGVrb8nENuM07P_k/edit?usp=sharing "Great study guide with plenty of information that's not included in the AWS webpages. Much of the info here is pulled from there.")