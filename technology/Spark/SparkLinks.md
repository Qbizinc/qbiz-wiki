---
title: Spark Links
description: 
published: true
date: 2022-08-23T14:32:16.510Z
tags: 
editor: markdown
dateCreated: 2022-08-22T16:30:39.560Z
---

# Some interesting links on Spark and using GPUs with Dataframes
## Videos
- Youtube by Databricks regarding Spark 3.0 support for GPUs (2020): [Deep Dive into GPU Support in Apache Spark 3.x](https://www.youtube.com/watch?v=4MI_LYah900)
- Spark performance monitoring (2019): [Apache Spark Core—Deep Dive—Proper Optimization Daniel Tomes Databricks](https://www.youtube.com/watch?v=daXEp4HmS-E)
- [Deep Dive into the New Features of Apache Spark 3.2 and 3.3](https://www.youtube.com/watch?v=CZWYKRkXhy8) Data+AI Summit 2022.
- [Top 5 Mistakes When Writing Spark Applictons](https://www.youtube.com/watch?v=WyfHUNnMutg) Spark Summit 2016 (NYC).


## Articles and tutorials
- [Medium article on GPU parameters](https://medium.com/walmartglobaltech/getting-started-with-apache-spark-gpu-rapids-part-i-938664771092).
- [From Query Plan to Performance](https://www.youtube.com/watch?v=_Ne27JcLnEc).
    - @49:40, nice diagram of join decision tree.
- Article on memory parameters [Decoding Memory in Spark](https://medium.com/walmartglobaltech/decoding-memory-in-spark-parameters-that-are-often-confused-c11be7488a24).
- Another article about memory settings [Spark Performance Tuning: spill](https://selectfrom.dev/spark-performance-tuning-spill-7318363e18cb).
- S3 Committers (important when not using Yarn) [Improve Apache Spark performance with the S3 magic committer](https://spot.io/blog/improve-apache-spark-performance-with-the-s3-magic-committer/).  Warning you may get when not using Yarn: *[WARN AbstractS3ACommitterFactory: Using standard FileOutputCommitter to commit work. This is slow and potentially unsafe.]*.
- Delight, a free perormance monitor (Delight)[https://www.datamechanics.co/blog-post/delight-the-new-improved-spark-ui-spark-history-server-is-now-ga].
- [Spark by Example Blog](https://sparkbyexamples.com/).
### Databricks
Must register to access most material.
- [Databricks training](https://www.databricks.com/learn/training/lakehouse-fundamentals). 
- [Databricks ML Guide](https://docs.databricks.com/applications/machine-learning/index.html).
- [Databricks Big Book of ML Ops](https://www.databricks.com/p/ebook/the-big-book-of-mlops).
- [Databricks - how to automate your ML pipeline](https://www.databricks.com/p/ebook/automate-your-machine-learning-pipeline).
- [Databricks - ML Projs plan to prod](https://www.databricks.com/p/ebook/machine-learning-engineering-in-action).


## Not Spark specific
- A tutorial on using the RAPIDS library and cuDF (CUDA dataframes) for GPUs -- not Spark related.  This library supports GPUs for standalone Pandas and Dask: [10 Minutes to cuDF and Dask-cuDF](https://docs.rapids.ai/api/cudf/nightly/user_guide/10min.html#Object-Creation).
