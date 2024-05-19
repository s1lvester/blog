---
title: "Healthcare NLP Summit write-up"
date: 2023-04-06

tags: ["NLP", "healthcare", "conference", "notes"]
---
I spent the last two evenings attending John Snow Labs' virtual "Healthcare NLP Summit," and I found it to be a stimulating event. If you couldn't attend, here are my notes:

## LLMs are all the hype
Large-language-models (LLMs) were the overarching topic of the conference, and some speakers (particularly from marketing and sales) were excited about the possibilities. However, there wasn't a clear vision of what these possibilities entailed.
Senior researchers and executives had a different sentiment. Elliot Bolton from Stanford CRFM discussed his experiences benchmarking BioMedLM against Med-PaLM and GPT-4. He noted that it is almost impossible for smaller research labs and startups to compete with larger players in the space of LLMs because of the resources required for training LLMs at the scale of GPT-3 / 4.

My Opinion: Healthcare professionals should avoid using hosted LLM APIs, even for research projects. Veysel Kocaman from John Snow Labs emphasized the importance of hosting models on-premise for training, specialization, and inference. This is especially true when working with patient data. In Germany, for instance, physicians are strictly prohibited from sharing even pseudonymized data with third parties without patients' consent. Even with consent, physicians could be criminally liable if unwanted re-identification occurs.

![line go up](./line-go-up.png "Line go up")

## Meaningless accuracy scores
Measuring accuracy has become ridiculous in some cases. Presenters often showcased accuracy scores for clinically meaningless tests, such as "their LLM passing the USMLE practice exam," PubMedQA, and MedQA. Some implied that these models would generalize to real-world data from electronic health records (EHRs) without requiring much specialization. This is like thinking a first-year resident in internal medicine is capable of diagnosing and treating Ewing-Sarkomas because they passed the USMLE and read the relevant literature.

An incomplete list of notable exceptions contains Paulo Pinho, Sudhakar Velamoor, Brooke Gruman, Natalia Vassilieva and Nasser Ghadiri who presented real-world use cases for NLP and accuracies within their development process or different evolutions of their models/ensembles. These were the insights I was looking for.

## Inference needs quality control
Several presenters invested substantial efforts into autonomous (rules-based) quality-control frameworks, which verify inference results before inclusion in their products. Saira Kazmi Ph. D. gave an excellent presentation on the importance of wrapping an NLP model into an API and creating processes and monitoring to capture the entire life cycle of the product.

John Snow Labs released an early-stage library to automate testing of NER and text-classification models called "nlptest" (https://nlptest.org), which looks promising. I’m definitely going to try it, especially since it’s api-compatible with Hugging Face and spaCy. Their proposed development cycle, which first augments the training data, and then tests for bias, robustness, accuracy and fairness seems pretty slick.

## Few-Shot Learning is definitely worth a try
Moshe Wasserblat from Intel presented their paper on "Efficient Few-Shot Learning Without Prompts," which introduced SetFit. This looks really interesting and I plan to try SetFit on one of my problems where annotated data is difficult to obtain. Especially since one of the authors (Luke Bates) currently resides in Darmstadt. ;-) Link to the Paper: https://arxiv.org/abs/2209.11055

## Summary
Overall, the conference lacked substance in some presentations, but it provided great insights into the development of real-world products. I would recommend attending next time.