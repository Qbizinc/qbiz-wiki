---
title: AWS ML Specialty
description: AWS has a certification for Machine Learning specialty. 
published: true
date: 2021-08-19T17:44:49.090Z
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
The AWS ML stack is made up of three key layers of abstraction.  When building a machine learning solution, key priorities to balance may consist of:
- speed to market
- level of customization
- machine learning skill level

At the top layer, you have AI Services, which are “fully managed services for quickly adding machine learning capabilities to workloads using API calls”; ML knowledge is not required to utilize these.  

The middle layer, ML Services, is primarily made up of SageMaker related tools.  SageMaker spans the machine learning pipeline by offering solutions for labeling data as well as building, training and deploying machine learning models while largely abstracting away infrastructure maintenance requirements.  

The bottom layer, ML Frameworks + Infrastructure, contains the “closest to the metal” solutions, allowing machine learning practitioners to build machine learning solutions from the ground up.  With deep learning containers and images, AWS supports many common machine learning frameworks such as TensorFlow, PyTorch and Apache MXNet.  

### AI Services
#### [Amazon Augmented AI](https://aws.amazon.com/augmented-ai/)
#### [Amazon CodeGuru](https://aws.amazon.com/codeguru/)
#### [Amazon Comprehend](https://aws.amazon.com/comprehend/)
#### [Amazon Comprehend Medical](https://aws.amazon.com/comprehend/medical/)
#### [Amazon Forecast](https://aws.amazon.com/forecast/)
#### [Amazon Fraud Detector](https://aws.amazon.com/fraud-detector/)
#### [Amazon HealthLake](https://aws.amazon.com/healthlake/)
#### [Amazon Kendra](https://aws.amazon.com/kendra/)
#### [Amazon Lex](https://aws.amazon.com/lex/)
#### [Amazon Lookout for Equipment](https://aws.amazon.com/lookout-for-equipment/)
#### [Amazon Lookout for Vision](https://aws.amazon.com/lookout-for-vision/)
#### [Amazon Monitron](https://aws.amazon.com/monitron/)
#### [Amazon Personalize](https://aws.amazon.com/personalize/)
#### [Amazon Polly](https://aws.amazon.com/polly/)

#### [Amazon Rekognition](https://aws.amazon.com/rekognition/)
- add image and video analysis to your applications using proven, highly scalable, deep learning technology
- requires no machine learning expertise
- identify objects, people, text, scenes, and activities in images and videos, as well as detect any inappropriate content
- highly accurate facial analysis and facial search capabilities that you can use to detect, analyze, and compare faces for a wide variety of user verification, people counting, and public safety use cases

#### [Amazon Textract](https://aws.amazon.com/textract/)
- automatically extracts text and data from scanned documents to identify, understand, and extract data from forms and tables
- fully managed machine learning service
goes beyond simple optical character recognition (OCR) 
- instantly read and process any type of document, accurately extracting text, forms, tables and, other data without the need for any manual effort or custom code

#### [Amazon Transcribe](https://aws.amazon.com/transcribe/)
- deep learning process called automatic speech recognition (ASR) to convert speech to text quickly and accurately
- add speech to text capability to applications
- recorded speech often needs to be converted to text before it can be used in applications
- transcribe customer service calls, automate closed captioning and subtitling, and to generate metadata for media assets to create a fully searchable archive
- Amazon Transcribe Medical to add medical speech to text capabilities to clinical documentation applications

#### [Amazon Translate](https://aws.amazon.com/translate/)
#### [AWS Panorama](https://aws.amazon.com/panorama/)

### ML Services
### Frameworks
### Infrastructure
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
[Official AWS Certified Machine Learning - Specialty Practice (MLS-P01)](https://www.certmetrics.com/amazon/)

**Important Nibbles of Info**
[Firehose Data Formats](https://docs.aws.amazon.com/firehose/latest/dev/record-format-conversion.html)
[SageMaker Endpoint Scaling](https://docs.aws.amazon.com/firehose/latest/dev/record-format-conversion.html)
[SageMaker and VPCs](https://docs.aws.amazon.com/sagemaker/latest/dg/appendix-notebook-and-internet-access.html)


**Additional study guides**
[Brian McMahon's Study Guide](https://docs.google.com/document/d/1B7m0sxJOVedQZeHNRdpB_y0sBEzBGVrb8nENuM07P_k/edit?usp=sharing "Great study guide with plenty of information that's not included in the AWS webpages. Much of the info here is pulled from there.")