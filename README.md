# 🧠 NeuReg: Neuro-Symbolic QA Generation from Regulatory Compliance

**NeuReg** is a neuro-symbolic QA generation framework that transforms complex regulatory documents into intelligent and explainable question–answering systems. It integrates ontology-guided knowledge graphs (KGs) with large language models (LLMs) to generate high-quality, semantically grounded QA pairs—combining structured symbolic knowledge with generative language capabilities.

---

## 🔄 Pipeline Overview

NeuReg consists of two main stages: **Knowledge Extraction** and **Question Answer Generation**.

### 1️⃣ Ontology-Guided Knowledge Extraction
- Regulatory text is split into coherent **text chunks**.
- A domain-specific ontology (EFRO) is used to guide **schema extraction** and **triple generation** using GPT-4.
- Each output triple is structured as **(subject, predicate, object)** with post-processing and alignment.

### 2️⃣ Question Answer Generation
- Each chunk is mapped to its corresponding ontology-guided KG (based on `chunk_id`).
- QA generation follows four steps:
  
  **(a) Question Type Selection**  
  Questions are categorized into:
  - **FactQ**: Factual lookups
  - **RelQ**: Relational reasoning
  - **CompQ**: Comparative analysis
  - **InferQ**: Inferential/multi-hop logic

  **(b) Prompt Augmentation**  
  Questions are generated using:
  - **Zero-Shot Prompting**  
  - **One-Shot Prompting**  
  - **Few-Shot Prompting**  

  **(c) QA Filtering**  
  Candidate QAs are filtered using:
  - Answer length check
  - Semantic similarity (cosine similarity < 0.85 with existing questions)
  - Rejection and retry up to 3 times

  **(d) Validation**  
  Final QA pairs are validated using:
  - Human expert annotators  
  - Automatic scoring from multiple LLM judges

---

## 🧠 Model Architecture

<img width="2708" height="2324" alt="github" src="https://github.com/user-attachments/assets/ab3e2137-b665-43ef-88ee-2f13bb3c6817" />


**Figure 1**: *NeuReg: Neuro-symbolic framework for regulatory QA generation using ZS, OS, and FS prompting with ontology-guided KG extraction.*

---

## 🚀 Motivation

Access to education funding is governed by complex and evolving regulations. These policies are often communicated through lengthy documents that are difficult for students and institutional staff to interpret. NeuReg addresses this challenge by transforming unstructured regulatory guidance into structured and explainable QA datasets—bridging the gap between dense policy language and actionable decision support.

---

## ✨ Key Contributions

### 🧠 A Novel Neuro-Symbolic Framework  
NeuReg combines symbolic reasoning from ontology-guided knowledge graphs with the generative power of LLMs. This enables accurate, semantically aligned, and explainable QA generation in high-stakes regulatory domains.

### 📊 A Domain-Specific Regulatory QA Dataset  
NeuReg introduces a QA dataset with four well-defined question types—**Factual**, **Relational**, **Comparative**, and **Inferential**—validated through expert human annotation and multi-model LLM judgment.

### 🔬 Systematic Evaluation & Ablation Studies  
The framework includes:
- Empirical comparison of prompting strategies (ZS, OS, FS)
- Isolation of KG-only and chunk-only contributions
- Evaluation of QA generation quality with both fine-tuned T5/FLAN-T5 models and SOTA LLM scoring

---

## 🎯 QA Generation Types

| Type      | Description |
|-----------|-------------|
| **FactQ**   | Direct factual lookup (e.g., eligibility dates, age thresholds) |
| **RelQ**    | Questions requiring entity–entity relationships from the KG |
| **CompQ**   | Comparisons between multiple entities or regulations |
| **InferQ**  | Multi-hop reasoning and semantic inference across chunk+KG |

---

NeuReg offers a reproducible, modular pipeline for ontology-grounded QA generation—enhancing transparency, interpretability, and compliance support in complex regulatory domains.

🔗 Explore the full repository for code, datasets, evaluation metrics, and detailed ablation studies.


## 📂 Repository Structure

```text
NeuReg/
├── README.md                          # Overview of the project, contributions, pipeline, and structure
├── LICENSE                            # Project license (e.g., MIT, Apache 2.0)
├── requirements.txt                   # Python dependencies for reproducing the results

├── data/                              # Preprocessing and knowledge graph construction
│   ├── chunks/                        # Extracting regulatory text chunks
│   │   ├── chunks.csv                 # Final cleaned chunk dataset
│   │   └── chunks.ipynb               # Chunk extraction notebook
│   ├── ontology/                      # Ontology schema and KG triples
│   │   ├── ontology_schema.json       # Extracted domain ontology in JSON
│   │   ├── Ontology_Guided_Triples.csv           # Ontology-guided KG triples
│   │   ├── Ontology_Guided_Triples_statistics.json  # Stats on generated triples
│   │   ├── EFRO_Schema_Extraction.ipynb           # Extract ontology schema from guidance
│   │   └── KG_Extraction.ipynb                    # Generate KG using ontology + chunks

├── qa_generation/                     # QA dataset generation using prompting
│   ├── Zero-shot.ipynb                # Zero-shot QA generation
│   ├── One-shot.ipynb                 # One-shot QA generation
│   ├── Few-shot.ipynb                 # Few-shot QA generation
│   ├── Zero-Shot_qa_dataset.json      # Output QA dataset (zero-shot)
│   ├── One-Shot_qa_dataset.json       # Output QA dataset (one-shot)
│   ├── Few-Shot_qa_dataset.json       # Output QA dataset (few-shot)
│   ├── Zero_Shot_QA_analysis_report.json  # Analysis report (zero-shot)
│   ├── One_Shot_QA_analysis_report.json   # Analysis report (one-shot)
│   └── Few_Shot_QA_analysis_report.json   # Analysis report (few-shot)

├── evaluation/                        # Evaluation modules for QA datasets
│   ├── ontology_guided/               # KG triples validation
│   │   ├── Evaluation.ipynb
│   │   ├── evaluation_results.csv
│   │   └── evaluation_report.json
│   ├── llm_judges/                    # LLM-based QA scoring (5 models)
│   │   ├── DeepSeek-R1-Distill-Llama-70B/
│   │   │   ├── DeepSeek-R1-Distill-Llama-70B.ipynb
│   │   │   ├── *_zeroshot_*.csv
│   │   │   └── *_fewshot_*.csv
│   │   ├── Gemma-2-27B/
│   │   ├── LLaMA-3.3-70B/
│   │   ├── Mixtral-8x22B/
│   │   └── Qwen3-32B/
│   ├── humans/                        # Human-based QA evaluation
│   │   ├── Evaluation_Template.pdf (or .csv)      # Annotation template
│   │   ├── Human_based_results_analysis.ipynb
│   │   └── humans_Analysis_report.csv
│   └── llm_vs_human/                 # Correlation between LLM and human scores
│       ├── llm_vs_human_Analysis_results_analysis.ipynb
│       └── Correlation_llm_vs_human.csv

├── analysis/                          # Statistical analysis & insights
│   ├── Statistical_Analysis.ipynb
│   ├── Readability_Analysis.csv       # FKGL, Flesch, etc.
│   ├── Vocabulary_Diversity_Analysis.csv
│   ├── Length_Distribution_Analysis.csv
│   ├── LLMs_based_results_analysis.ipynb
│   └── LLMs_Analysis_report.csv

├── ablations/                         # Ablation studies (chunks_only/kg_only)
│   ├── chunks_only/                   # QA from chunks only (no KG)
│   │   ├── chunks_only_qa_dataset.ipynb
│   │   ├── evaluation/
│   │   │   ├── chunks_only_evaluation_DeepSeekR1.ipynb
│   │   │   ├── *.csv
│   ├── kg_only/                       # QA from KG only (no chunks)
│   │   ├── KG_only_qa_dataset.ipynb
│   │   ├── evaluation/
│   │   │   ├── KG_only_evaluation_DeepSeekR1.ipynb
│   │   │   ├── *.csv
│   ├── Ablation_1_chunks_only_qa_dataset.json      # Chunks-only QA JSON
│   ├── Ablation_1_chunks_only_analysis_report.json # Evaluation report
│   ├── Ablation_2_kg_only_qa_dataset.json          # KG-only QA JSON
│   ├── Ablation_2_kg_only_analysis_report.json     # Evaluation report

├── fine_tuning/                       # Fine-tuning experiments on QA datasets
│   ├── t5_small/
│   │   ├── t5_small_zero.ipynb
│   │   ├── t5_small_one.ipynb
│   │   └── t5_small_few.ipynb
│   ├── t5_base/
│   ├── t5_large/
│   ├── flan_t5_small/
│   ├── flan_t5_base/
│   ├── flan_t5_large/
│   └── results/                       # Final results and logs
```
