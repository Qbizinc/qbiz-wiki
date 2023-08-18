---
title: Generative AI at QBiz
description: Knowledge repository for GenAI
published: true
date: 2023-08-18T17:52:10.829Z
tags: llm, generative ai
editor: markdown
dateCreated: 2023-08-01T23:28:56.886Z
---

# Generative AI


### Main AI players
🤖 [OpenAI](https://openai.com): Research company that aims to promote and develop “friendly” AI. [ChatGPT](https://chat.openai.com) and DALLE-2 managed to take AI mainstream. Their models are not open source although they are available through API and Web playground with a paid subscription. 

⚙️ [Google AI](https://ai.google/discover/generativeai): Bard is their alternative to ChatGPT. Google provides API usage access and Vertex AI deployment using their main model PaLM. 

Ⓜ️ [Microsoft AI](https://www.microsoft.com/es-mx/ai/ai-platform): BingChat is their ChatGPT alternative. Azure cloud provides services, infrastructure and tools for developing and deploying AI applications. The easiest way to use OpenAI models as they are partners. 

🤗 [HuggingFace Hub](https://huggingface.co/docs/hub/index): The Hugging Face Hub is a platform with over 120k models, 20k datasets, and 50k demo apps (Spaces), all open source and publicly available, in an online platform where people can easily collaborate and build ML together. HuggingChat is their ChatGPT alternative.


### Getting Started
[**What's the HuggingFace Hub?**](https://huggingface.co/docs/hub/index)
Is a platform with over 120k models, 20k datasets, and 50k demos in which people can easily collaborate in their ML workflows. The Hub works as a central place where anyone can share, explore, discover, and experiment with **open-source** Machine Learning.

**What can you find at the hub?**
1. [Models](https://huggingface.co/models)
2. [Datasets](https://huggingface.co/datasets)
3. [Spaces](https://huggingface.co/spaces)

---
**Basic Usage**
1. Define the task to solve. https://huggingface.co/tasks contains the list of available tasks to solve with HuggingFace.
2. Choose a model from the proper list based on the ask to solve. (i.e. *facebook/bart-large-cnn* for "Text Summarization")
3. Install the transformers library `pip install transformers` 
4. Run model inference using the pipeline object
```python
from transformers import pipeline

# This model is a `zero-shot-classification` model.
# It will classify text, except you are free to choose any label you might imagine
classifier = pipeline(model="facebook/bart-large-mnli")
classifier(
    "I have a problem with my iphone that needs to be resolved asap!!",
    candidate_labels=["urgent", "not urgent", "phone", "tablet", "computer"],
)
```
`Output:`

```
{'sequence': 'I have a problem with my iphone that needs to be resolved asap!!', 'labels': ['urgent', 'phone', 'computer', 'not urgent', 'tablet'], 'scores': [0.504, 0.479, 0.013, 0.003, 0.002]}
```

## Cloud Warehouses
### Databricks: *Model inference using Hugging Face Transformers*

**Requirements:**
- MLflow 2.3
- Any cluster with the Hugging Face `transformers` library installed can be used for batch inference. The `transformers` library comes preinstalled on Databricks Runtime 10.4 LTS ML and above.

**Use a pipeline as a Pandas UDFs to distribute model computation on a Spark cluster:**
1. Check device and create pipeline:
```python
from transformers import pipeline
import torch
device = 0 if torch.cuda.is_available() else -1
summarizer = pipeline("summarization", model="facebook/bart-large-cnn", device=device)
```

2. Apply the pipeline as a PandasUDF
 ```python
import pandas as pd
from pyspark.sql.functions import pandas_udf
from tqdm.auto import tqdm

@pandas_udf('string')
def summarize_batch_udf(texts: pd.Series) -> pd.Series:
  pipe = tqdm(summarizer(texts.to_list(), truncation=True, batch_size=8), total=len(texts), miniters=10)
  summaries = [summary['summary_text'] for summary in pipe]
  return pd.Series(summaries)
  ```
Using the UDF is identical to using other UDFs on Spark. For example, you can use it in a select statement to create a column with the results of the model inference.
 ```python
spark.sql(f"CREATE SCHEMA IF NOT EXISTS {output_schema}")
summaries = sample.select(sample.title, sample.text, summarize_batch_udf(sample.text).alias("summary"))
summaries.write.saveAsTable(f"{output_schema}.{output_table}", mode="overwrite")
 ```
### Snowflake: *Deploying models using Snowpark* 
1. Create a Snowflake stage to store the vectorized UDF. Let's call this stage ZERO_SHOT_CLASSIFICATION.
```sql
CREATE STAGE IF NOT EXISTS {your db}.{your schema}.ZERO_SHOT_CLASSIFICATION;
```
2. Compress the model locally for transfering it to Snowflake
```python
import joblib
joblib.dump(classifier, 'bart-large-mnli.joblib')
```
3. Use Snowpark put command to transfer the model to the stage
```python
from snowflake.snowpark import Session
session = Session.builder.configs({your connection parameters}).create()
session.file.put(
   'bart-large-mnli.joblib',
   stage_location = f'@{your db}.{your schema}.ZERO_SHOT_CLASSIFICATION',
   overwrite=True,
   auto_compress=False
)
```
NOTE
When we need to use the model, we must first decompress it, which can be time-consuming. To speed up this process, we can utilize the cachetools Python library. This library stores the outcomes of function calls in a cache and retrieves them when the same inputs are used again. We can create a function using this library in the following way:

```python
# Caching the model
import cachetools
Import sys
@cachetools.cached(cache={})
def read_model():
   import_dir = sys._xoptions.get("snowflake_import_directory")
   if import_dir:
       # Load the model
       return joblib.load(f'{import_dir}/bart-large-mnli.joblib'
```
4. Write the vectorized UDF function to use the model
```python
from snowflake.snowpark.functions import pandas_udf
from snowflake.snowpark.types import StringType, PandasSeriesType
@pandas_udf(  
       name='{your db}.{your schema}.get_review_classification',
       session=session,
       is_permanent=True,
       replace=True,
       imports=[
           f'@{your db}.{your schema}.ZERO_SHOT_CLASSIFICATION/bart-large-mnli.joblib'
       ],
       input_types=[PandasSeriesType(StringType())],
       return_type=PandasSeriesType(StringType()),
       stage_location='@{your db}.{your schema}.ZERO_SHOT_CLASSIFICATION',
       packages=['cachetools==4.2.2', 'transformers==4.14.1']
   )
def get_review_classification(sentences: pd.Series) -> pd.Series:
   # Classify using the available categories
   candidate_labels = ['customer support', 'product experience', 'account issues']
   classifier = read_model()

    # Apply the model
   predictions = []
   for sentence in sentences:
       result = classifier(sentence, candidate_labels)
       if 'scores' in result and 'labels' in result:
           category_idx = pd.Series(result['scores']).idxmax()
           predictions.append(result['labels'][category_idx])
       else:
           predictions.append(None)
   return pd.Series(predictions)
```
5. Call the function using SQL
```sql
WITH cs AS (
   SELECT value AS customer_review
   FROM (
   VALUES
       ('Nobody was able to solve my issues with the system'),
       ('The interface gets frozen very often'),
       ('I was charged two in the same period')
   ) AS t(value)
)
SELECT
   cs.customer_review,
  {your database}.{your schema}.get_review_classification(
       customer_review::VARCHAR
   ) AS category_prediction
FROM cs;
```

Please check the documentation or revelant tutorials for updates on best practices. This tutorial is based on the article [**Deploying pre-trained LLMs in Snowflake.**](https://medium.com/snowflake/deploying-pre-trained-llms-in-snowflake-75a0d07ef03d)


