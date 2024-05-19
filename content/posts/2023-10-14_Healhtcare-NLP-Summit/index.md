---
title: "Healthcare NLP Summit write-up 10/2023"
date: 2023-10-14

tags: ["NLP", "healthcare", "conference", "notes"]
---

The past year has been eventful in the fields of Machine Learning (ML) and Artificial Intelligence (AI). From October 3rd to 5th, 2023, John Snow Labs organized their second Natural Language Processing (NLP) Summit of the year. This event included a variety of informative presentations from both emerging start-ups and established players in the Language Model sector. This is the first part of a two-part conference write-up; it discusses trends and technologies, while the subsequent part will focus on applications in healthcare.

## Executive Summary
Companies and organizations constructed small to medium-sized proof of concepts (POCs) utilizing Large Language Models (LLMs) in 2023, achieving varying levels of success. As always, finding Product/Model-Market-Fit is essential. A common challenge arises from a misalignment between customer expectations and initial LLM capabilities. Multiple methods can be employed to tailor an LLM to a specific business use case, including Re-Training, Fine-Tuning, external Connectors, Prompt-Engineering, and Retrieval Augmented Generation (RAG). In practice, most successful implementations integrate an ensemble of "conventional" ML models, such as Classifiers or Regressions, with an LLM to preprocess data and reduce the volume of LLM queries. This approach primarily addresses the limitations associated with context length (the extent of text that can be input into an LLM prompt) and the cost-per-query for LLM inferences.

## How do I bring my POC into production?
M Waleed Kadous from Anyscale gave a very good talk on how to bring LLMs and different Models into production. My main takeaway was the concept of hybrid implementations of Retrieval Augmented Generation (RAG) with a Supervised Classifier to decide which which model to run and what context to provide to each prompt. Anyscale also open sourced code and documentation for their APIs and general concepts on github.
Of course GPT-4 performs better - but the bills are going to hunt you in your sleep!
Yuval Belfer from AI21 Labs gave a very entertaining talk on his experiences consulting clients on how to take initial LLM-POCs into production and why the largest model doesn't guarantee the best output. Unit-costs per inference are also a big issue. I've also learned a new type of bias from him, called "Recency Bias" which refers to prompt design and the fact, that most models place a lot of emphasis on the last sentences of the prompt. Therefore restructuring your prompts to include the most important instructions at the end can drastically improve your results.
He also shared his rule of thumb on when to fine-tune and when to use RAG in order to get the best results for a given use case:

- "The model should write like ..." --> Finetuning to change the style
- "The model should write about ..." --> RAG to decrease hallucinations

## RAG and Vector Databases for the win
To enable RAG, fast retrieval of embedding-vectors is a must. Bob van Luijt from Weaviate presented a demo on how to integrate LLM APIs with a vector database. This enables embeddings to improve over time by filling gaps with generated data in the background. Their write-up can be found here. Also, this blog post from Roie Schwaber-Cohen provides a good primer on vector databases.

## OK, so I need a Vector-DB. Now what?
Anton Troynikov, founder of Chroma, an open source Vector-DB, gave a great talk about how to conceptualize Vector-DBs as "programmable memory" for ML-models (he also had the best Mic-Setup!). This was easily one of the highlights of the first day, since he gave a no-BS overview of the current problems in information retrieval as well as research areas to watch for in the near future. I highly recommend to check out the Chroma blog and a one of his interviews.
Interestingly Veysel Kocaman from John Snow Labs also highlighted some of the same issues and proposed to treat the whole RAG process as a pipeline with many tunable parameters, starting at preprocessing and OCR. He summed it up in one sentence:
"I have seen many talks and tutorials where people are playing with 10 PDF documents. However, in production, selection of the Embedding-model and Vector-DB really matters"

As you've probably noticed by now - RAG is all the hype. Both because fine-tuning is expensive, but also because it allows production systems to integrate additional, up-to-date knowledge easily.

Itay Zitvar from Hyro also pointed out, that they prefer RAG over fine tuning, since use cases change frequently and training adapters for every customer seems infeasible. However, he shared some lessons learned during their hybrid approach to integrating LLMs into their production pipelines:

## Quality of Inference, Risk and Bias
Unsurprisingly, a lot of presenters highlighted that they encountered biases that they didn't encounter during initial validation. John Snow Labs released their new tooling "langtest" which is an evolution of last-years "nlp-test" library, that can be used to generate and execute automated tests for language models. I've used nlp-test in the past and really liked the platform-agnostic approach that integrates with all the popular NLP libs.
For high-risk applications (HealthCare, Chemical Engineering, Finance and so on) all presenters also showed Human-In-The-Loop (HITL) steps in their pipelines towards the end - so all "automatic validation" and "ML-Judges" still seem to exist mostly in academic settings and low-risk applications.

During Medical School, we used to joke that "when in doubt - always choose answer C" in our multiple-choice tests. Turns out LLMs do the same thing 😅
Additionally, biases also emerge when evaluating LLM output with LLMs. Davis Blalock from Databricks Mosaic Research gave a great talk on how "the big boys" train LLMs and why our experiences with scikit-learn do not translate into the scale of LLM training. I urge you to read this statement and draw your own conclusion from "this is so cool! Accuracy 1.0!!! Let's publish / put it into production". Here's a more comprehensive write-up of the incidence: read me.
Random, valuable tips and tricks

# So where are the killer apps?

Most applications for ML in HealthCare are still in early stages of development. Medical Imaging is the notable exception - but this is not due to LLMs. Data availability and standardization remain challenging and knowledge extraction is primarily done with "traditional" NLP methods like Named Entity Recognition (NER). Most of these issues aren't new and even the biggest players (like Google) have them. Clinical language models still outperform general-use LLMs for reasoning tasks, therefore use of LLMs is mostly limited to user interfaces at the moment (think: "medical chatbots" and "chat with your documents"). Multimodal LLMs might change this outlook, but as of now, there is simply no "killer app" using LLMs in HealthCare.

## Multi-modality is always right around the corner
One school of thought argues, that models need multi-modal inputs to be successful in medical reasoning. Intuitively this makes sense - after all, a physician also takes several modalities (e.g. Text, Images, Sensor-Data) and their changes over time into account, when diagnosing and treating patients. Both Durga S. Borkar, MD, MMCi hinted at this, and Zain Hasan gave an introduction on how this could be facilitated with Vector-DBs.
While the concept of multi-modality isn't specific to LLMs (see slide below), this is one development to watch out for in the next 12 months with recent announcements from OpenAI.

## Cohort Definition / Precision Medicine
Shaayaan Sayed from ClosedLoop demonstrated their natural language interface to their risk stratification platform. Users can ask questions like "show all patients that are at high risk to be admitted to the ED in the next 6 weeks" and the LLM generates custom SQL queries to retrieve this patient cohort. While this sounds straight forward, they also struggled with input size limitations and had to couple their inference models with additional classifiers to reduce the number of columns that would be relevant to such a generated query. So context length limits strike again (see pt. 1).
Dr. Richard "Spencer" Schaefer from the US VA showed their initial results on their quest to extract knowledge from large amounts of Electronic Medical Records (EMR) by using LLMs to query and summarize findings to the users specific questions. Unfortunately he wasn't at liberty to discuss detailed results. From their evaluation of self-hosted models, they seem to lean toward Llama-2 for their domain-specific tasks. However, the project still seems to be in early stage, maybe they'll release a more detailed report in the future.

## Knowledge Extraction
Itay Zitvar and Yizhen Zhong both reported on their experiences with using GPT architectures / LLMs for domain-specific NLP tasks and noted that they underperformed compared with hybrid approaches or BERT models.

## Where will all the medical text for training come from?
De-Identification is at the top of my current interests, because high quality de-id is always the first step to enable knowledge extraction for unstructured text. In relation to our recent poster on using on-premise LLMs for de-id (find the abstract here), David Talby from John Snow Labs showed some interesting numbers and facts that really question the economic feasibility of using LLMs for de-identification tasks:

While ETL and de-identification isn't specific to medical applications, there was one talk that really ticked a lot of boxes for me in terms of daily needs and pain points. Matt Robinson from unstructured.io demoed their ETL library. I think this can be really valuable to speed up POC / MVP development and I will probably try their open-source lib in the future, and see how it scales from there.
