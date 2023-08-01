---
title: Generative AI at QBiz
description: Knowledge repository for GenAI
published: true
date: 2023-08-01T23:27:18.717Z
tags: llm, generative ai
editor: markdown
dateCreated: 2023-07-26T21:18:23.021Z
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
### Databricks