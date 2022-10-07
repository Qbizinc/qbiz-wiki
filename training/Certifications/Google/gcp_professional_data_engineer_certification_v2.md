---
title: GCP Professional Data Engineer Certification v2
description: 
published: true
date: 2022-10-07T21:12:09.564Z
tags: 
editor: markdown
dateCreated: 2022-02-28T23:09:08.207Z
---



# The exam itself



* The exam requires in-depth knowledge of a wide variety of products, some of which are unique to Google and are not directly analogous to other cloud platforms such as AWS. If you've never used GCP and are starting completely from scratch, it may take **at least 1-2 months** to master the material well enough to pass the exam.
* The exam consists of 50 multiple choice questions. You have 2 hours to complete it. The passing criteria is widely thought to be in the 70-80% range.
* You'll have the option to "flag and move on" to other questions and then loop back and/or change your answers to questions you've already answered.
* The remote proctored exam has stringent environmental requirements (e.g., no breaks, no food or drink, no background noise, etc). One that I found especially challenging is that you're not permitted to read the questions/answers out loud. It may be helpful to rehearse not speaking when taking the practice exams.
* The questions are very much in the spirit of "If you were faced with this problem, what combinations of products and services would you recommend?" There will be some technical questions about storage limits, costs, command-line options, etc, but these questions aren't likely to be the majority of the exam.


# Products and topics

The majority of questions will relate to the following:.



* **BigQuery**: columnar-store ANSI SQL compliant database with a browser interface (similar to Snowflake) that supports both batch and streaming data, as well as machine learning
* **BigTable**: a highly performative NoSQL data store, based on HBase
* **Dataflow**: an ETL tool based on Apache Beam that supports both batch and streaming data
* **Dataproc**: runs the Hadoop family of services including Spark
* **Cloud** **Storage**: an object store, pretty much identical to Amazon's S3
* **Machine learning** is a significant component (up to 30%) of the exam, with neural networks/deep learning and remedying overfitting/underfitting being important topics.
* Other products touched on include **Cloud SQL** (MySQL/Postgres in the cloud), **Cloud Spanner** (a highly available transactional database that can span multiple regions across the globe and handle vast amounts of data), and **Compute Engine** (virtual machines that act like servers, nearly identical to Amazon's EC2 instances)
* Many scenarios involve the most effective and secure ways to move on-premise workloads and data to the cloud. As you learn about GCP services, it can be helpful to think about them from this perspective.


# Some high-level guidelines

Often questions will have several viable answers and you'll need to choose the best according to the problem requirements. Here are some rules of thumb that can help you land on the best option.



* Google strongly prefers products/solutions that separate compute and storage. If there's an option to use local, persistent storage (e.g., a Compute Engine instance with a persistent drive for use with HDFS), it's almost always wrong. That said, the answer isn't always "use Cloud Storage" because many products already separate compute and storage under the hood (e.g., BigQuery and BigTable).
* Managed, ephemeral, and largely serveless services are almost always preferred to open-source/DIY solutions. E.g., if you see an option like "launch an instance and install Kafka", it's almost always wrong.
* SSD vs HDD comes up fairly frequently with regard to certain services. SSD is almost always preferred, though HDD can be appropriate for low-cost, very large, batch-heavy workloads and data stores that don't require low latency reads.
* Co-locating services in the same zone ensures the lowest latency and is generally preferred unless you're optimizing for high database resiliency and availability – in which case replicas and mirrors in multiple zones within the same region can be utilized for failover.
* There may be several questions about remedying overfitting and underfitting in machine learning models. In general, reducing features/complexity and/or increasing the volume of training data is good for overfitting problems, and increasing features/complexity is good for underfitting problems.


# Learning resources


## Where to start?
QBiz employees now have access to the partner kickstart program which includes a 6-week prep course and an exam voucher if the program is completed within 10 weeks of enrollment. More details here: [Kickstart](/training/Certifications/Google/Kickstart)

You may find the training options complex and confusing. As of this writing, Google sponsors two sets of courses, the [Data Engineering & Analytics Courses | Google Cloud Training](https://cloud.google.com/training/data-engineering-and-analytics#data-engineer-learning-path) path and the [Data Engineer Learning Path | Google Cloud Skills Boost](https://www.cloudskillsboost.google/paths/16) path (recommended as it's more thorough), which is largely an expansion of the former. The latter currently offers unlimited learning and lab credits for $29/month (though you can purchase individual credits if you'd rather do that).

A starter page, [Professional Data Engineer Certification | Google Cloud](https://cloud.google.com/certification/data-engineer), contains links to the first path mentioned above, practice questions, and the official exam registration.

Depending on your learning style and cloud experience, you may want to start with the [Preparing for the Google Cloud Professional Data Engineer Exam](https://www.cloudskillsboost.google/course_templates/72) course for a top-down approach that you can use to identify knowledge gaps, or you may want to work your way up from introductory courses such as [Google Cloud Big Data and Machine Learning Fundamentals](https://www.cloudskillsboost.google/course_templates/3). In either case, if you've had no experience with GCP, the [A Tour of Google Cloud Hands-on Labs](https://www.cloudskillsboost.google/focuses/2794?parent=catalog) course will help familiarize you with both the UI and command-line interactions.


## Additional course options

Both [A Cloud Guru](https://acloudguru.com/) and [Cloud Academy](https://cloudacademy.com/) offer an extensive set of similar courses and practice exams. I didn't dig deep into their materials but they appeared to be high quality from what I did see. Coursera appeared to offer Google's course, and Udemy's offerings appeared lower quality to me.


## Courses and Quests

The training materials consist of **courses**, which are lecture-heavy, and **quests**, which consist primarily of hands-on lab challenges. Because the exam focuses more on use cases than on the actual mechanics of using the GCP services, I generally skipped the quests (with the exception of the first one, which introduces you using the GCP GUI and command line).


## Modules, Lectures, and Labs

Courses consist of multiple **modules** (I found most were in the 4-6 module range), and each module consists of a series of video lectures, hands-on labs (usually one per module), and links to PDF versions of the lectures.

I found many of the lectures spent more time than I would have liked introducing topics. If you're already familiar with key concepts in cloud computing such as separating compute from storage, map/reduce and Hadoop-related technologies, object stores, etc., feel free to skip ahead (or listen at a faster rate).

The labs all follow a similar format:



1. The labs are timed and cannot be paused. That said, I found the time allotted was usually far more than what was required to complete the labs, especially as I got more familiar with the GCP console.
2. Open the GCP web console using the supplied student account ID and password. It's highly recommended that you use Chrome and open the console in an incognito window so Chrome doesn't attempt to associate the student account with your personal one.
3. Launch a service using the navigation bar to the left and/or open a shell session.
4. Follow the step-by-step instructions for the specific lab (vs the setup instructions, which are generic). In almost all cases where code is required, you'll be able to copy the code directly and paste it in the shell.
5. Some labs will check your work by confirming that you've completed a certain step. I found this helpful to prevent me from rushing through and overlooking important steps
6. End the lab. Any artifacts/environments/infrastructure you create during your session will be destroyed when the session is over, so that they no longer incur costs.

Be aware that you may find that some of the labs reference UI elements that have changed since the labs were created. This can be frustrating, especially when you're just getting used to the console, but the majority of changes aren't radical departures. Don't be afraid to poke around to find the right element, option, etc.


## Documentation

Unfortunately, the lectures don't cover everything that can appear on the exam. While a module or course is still fresh in memory, it's a good idea to at least scan through the [Google Cloud documentation](https://cloud.google.com/docs) to see if there are any important topics not mentioned in the module or to deepen your understanding on areas that you may have found confusing.


## Practice Exams

As you gain confidence with the GCP products that are covered by the exam you'll want to practice with as many questions as possible. There are a number of realistic practice tests and sets of questions available:



* [Google's own practice questions](https://docs.google.com/forms/d/e/1FAIpQLSfkWEzBCP0wQ09ZuFm7G2_4qtkYbfmk_0getojdnPdCYmq37Q/viewform): there are ~~40~~ 22 questions and they're the same every time; no grade is given
* [Cloud Academy Cert Prep Exam](https://cloudacademy.com/quiz/35356/): draws 50 randomly from a larger set; this felt the closest to the actual exam to me (Jay) and its grading is more rigorous than others, requiring a minimum score for each major subject area.  Requires regestration (Free Trial available).
* [A Cloud Guru Practice Exam](https://practice-exam.acloud.guru/gcp-certified-professional-data-engineer): draws 50 randomly from a larger set; simple pass/fail grading
* [A Cloud Guru Final Exam](https://practice-exam.acloud.guru/fd871722-ab09-450b-8a3a-eac2852a84d5): similar to the above, but incorporates some of Google's own questions
* [Exam Topics practice questions](https://www.examtopics.com/exams/google/professional-data-engineer/view/1/): while this site has a lot of questions, **the answers are often incorrect**, with the correct answers being debated in the question discussions. Use with caution!


# Product notes and documentation


## Dataflow

There will be numerous questions related to Dataflow so it's worth spending extra time understanding this product, especially considering how unique and complex it is.

Dataflow is a serverless ETL service that handles both streaming and batch data. It uses [Apache Beam](https://beam.apache.org/) as its execution engine and provides both GUI interfaces and Python/Java APIs for workflow creation. At a high level, Apache Beam can be thought of as a framework that abstracts Spark-like concepts, and workflows are often set up in a mapreduce-like structure.

It's highly recommended that you familiarize yourself with some of the [Basics of the Beam model](https://beam.apache.org/documentation/basics/), even before you take your first Dataflow courses.

As you get more comfortable with the basics, you may want to take a deeper dive into [transformations](https://beam.apache.org/documentation/programming-guide/#transforms), which are the building blocks of workflows. I personally had a difficult time following some of the course modules until I read this section.

Another crucial topic, especially for streaming data, is [windowing](https://beam.apache.org/documentation/programming-guide/#windowing). Windowing allows you to set up complex rules to implement requirements like sessionization and handling late arriving data.

Finally, some notes about the Dataflow labs:



* There are numerous command line and programmatic configuration options; one of these is a "runner" option, which you can use to specify whether to run locally (for testing) or in the cloud. I found the labs didn't do a great job of explaining this clearly; you may want to read some documentation on [configuring pipeline options](https://beam.apache.org/documentation/programming-guide/#configuring-pipeline-options) 
* While workflows run very quickly locally, they can take a long time to start up in the cloud (e.g., 10-15 minutes) and may appear frozen during this time
* It's worth learning where to find the log output in the Dataflow console; if there's an error, your job may continue silently retrying for awhile (this can be another cause of a job appearing frozen)
* Sometimes your first job run in the cloud fails almost immediately, due to a permission issue. If this happens, submit it again or disable then enable the Dataflow API (you'll learn how to do this in the labs)

Lastly, I've come across an inconsistency between the Videos and Documentation on the subject of Windowing.  In the Labs, the diffferent types of Windowing are knows as *Fixed*, *Sliding* and *Session* (these are the terms used by Apache Beam); However, in [Google's Dataproc Streaming documenation](https://cloud.google.com/dataflow/docs/concepts/streaming-pipelines), calls these modes *Tumbeling*, *Hopping* and *Session*.  This absolutely tripped me up on the pracice exam because the exam used terms not presented in the labs or lessons.

## BigQuery

BigQuery is a flagship GCP product and is featured in numerous questions. At a high level, it's a serverless columnar store SQL database whose features include:



* The ability to do streaming and batch inserts
* Separation of compute and storage (it has its own very efficient file system under the hood but can also point to Cloud Storage)
* High resiliency. Because of the separation of compute and storage, data isn't lost if a node goes down. Additionally, shuffling data (e.g., during a resize) is fast because it's just shuffling pointers, not the data itself.
* GIS SQL extensions
* The ability to perform machine learning via SQL extensions
* The ability to read from and write to Cloud Storage

There is a wide variety of topics to dive into, including:
* [Access control](https://cloud.google.com/bigquery/docs/access-control) to tables and views, especially Authorized Views
* [Column](https://cloud.google.com/bigquery/docs/column-level-security) and [row](https://cloud.google.com/bigquery/docs/row-level-security-intro) level security
* [Big Query ML](https://cloud.google.com/bigquery-ml/docs/introduction)
* [Denormalizing](https://cloud.google.com/bigquery/docs/loading-data#loading_denormalized_nested_and_repeated_data) by [specifying nested and repeated](https://cloud.google.com/bigquery/docs/nested-repeated) fields

## Vertex AI
Vertex AI is a suite of services designed to make ML more turnkey.  It has a models ready-to-use (no training required) in areas of NLP and image processing.  An example is translating one language to another, just like Google Translate; another is Optical Character Recognition.  With these pre-traineed models, you just upload you documents; models have been trianed with text and images avialble to Google.

- Has pre-trained models.
- Has built-in feature store.
- Can build custome models using Tensorflow.
- Has Workbench (Jupyter Notebook) for doing development and EDA.
- A manual training option, through the UI, that allows for the labeling of your data to be outsourced.

## Transfer Appliance

[Transfer Appliance](https://cloud.google.com/transfer-appliance/docs/4.0/overview) is a suitcase-sized or rack-sized storage device used to physically move data from on-prem to Cloud Storage.  It comes in 7, 40 and 300 TB sizes (see [pricing](https://cloud.google.com/transfer-appliance/pricing)).  On the Practice Exam, I got the following question:

- Low-cost one-way one-time migration of two 100-TB file servers to Google Cloud; data will be frequently accessed and only from Germany.  *Note the question mentiones "one-time" and the total capacity required fits within a 300TB appliance.*  Be careful: multiple answers can include Transfer Appliance, the difference between the correct and wrong answers may be in the kind of storage the data is transfered to.

