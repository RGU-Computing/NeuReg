# 🧠 NeuReg: Neuro-Symbolic QA Generation from Regulatory Compliance

NeuReg is a neuro-symbolic QA generation framework that transforms complex regulatory documents into intelligent, explainable QA systems. It integrates ontology-guided knowledge graphs (KGs) with large language models (LLMs) to generate high-quality, semantically grounded question–answer (QA) pairs.

---

## 🔄 Pipeline Overview

1. **Ontology-Guided Triple Extraction**  
   Structured triples are extracted from Education Funding Regulatory (EFR) documents using a domain-specific ontology.

2. **KG–Text Pair Formation**  
   Each triple is aligned with its corresponding text segment to provide contextual grounding.

3. **Multi-Strategy Prompting**  
   QA pairs are generated using Zero-Shot (ZS), One-Shot (OS), and Few-Shot (FS) prompting strategies.

4. **Quality Validation**  
   QA quality is assessed through expert human annotation and automatic evaluation using state-of-the-art LLMs.

---

## 🚀 Motivation

Access to education funding is governed by complex and evolving policies. These are often communicated in lengthy documents that are difficult for students and staff to interpret. NeuReg addresses this challenge by converting these regulatory documents into structured and queryable QA datasets—bridging the gap between unstructured policy language and structured decision support.

---

## ✨ Key Contributions

### 🧠 A Novel Neuro-Symbolic Framework

NeuReg combines LLM-based generative reasoning with symbolic structure from ontology-guided KGs, enabling semantically rich and accurate QA generation in high-stakes regulatory domains.

### 📊 First-of-Its-Kind Regulatory QA Dataset

We present a domain-specific QA dataset covering four distinct question types. It is validated via expert human judgments and automatic scoring by multiple SOTA LLMs.

### 🔬 Empirical Validation & Ablation Studies

Our evaluations isolate the unique contributions of KG and text inputs, demonstrate the effectiveness of different prompting strategies, and quantify model performance through fine-tuned T5/Flan-T5 variants.

---

## 🎯 QA Generation Types

| Type      | Description |
|-----------|-------------|
| **FactQ**   | Direct factual retrieval (e.g., definitions, thresholds, dates) |
| **RelQ**    | Relational reasoning over KG (e.g., entity-to-entity links) |
| **CompQ**   | Comparison of policies, programs, or eligibility rules |
| **InferQ**  | Multi-hop reasoning and inference across KG and text |

---

NeuReg offers a reproducible, ontology-grounded QA pipeline designed to improve transparency and interpretability in complex regulatory environments. For code, datasets, evaluation results, and ablation studies, explore the full repository.

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
