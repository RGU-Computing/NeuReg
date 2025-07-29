# 🧠 NeuReg: Neuro-Symbolic QA Generation from Regulatory Compliance

**NeuReg** is a neuro-symbolic QA generation framework that transforms complex regulatory documents into intelligent and explainable question–answering systems. It integrates ontology-guided knowledge graphs (KGs) with large language models (LLMs) to generate high-quality, semantically grounded QA pairs—combining structured symbolic knowledge with generative language capabilities.

---

## 📚 Table of Contents
- [🔄 Pipeline Overview](#-pipeline-overview)
- [🧠 Model Architecture](#-model-architecture)
- [🚀 Motivation](#-motivation)
- [✨ Key Contributions](#-key-contributions)
- [🎯 QA Generation Types](#-qa-generation-types)
- [📂 Repository Structure](#-repository-structure)
- [⚙️ Installation](#️-installation)
- [▶️ Getting Started](#-getting-started)
- [📖 Citation](#-citation)
- [📄 License](#-license)

---

## 🔄 Pipeline Overview

NeuReg consists of two main stages: **Knowledge Extraction** and **Question Answer Generation**.

### 1️⃣ Ontology-Guided Knowledge Extraction
- Regulatory text is split into coherent **text chunks**.
- A domain-specific ontology (EFRO) is used to guide **schema extraction** and **triple generation** using GPT-4.
- Each output triple is structured as **(subject, predicate, object)** with post-processing and alignment.

### 2️⃣ Question Answer Generation
Each chunk is mapped to its corresponding KG (based on `chunk_id`). The QA generation follows four steps:

1. **Question Type Selection** – Factual, Relational, Comparative, Inferential  
2. **Prompt Augmentation** – Zero-shot, One-shot, Few-shot prompting strategies  
3. **QA Filtering** – Answer length, semantic similarity (cosine < 0.85), retry up to 3 times  
4. **Validation** – Human annotation and automatic scoring from LLMs

---

## 🧠 Model Architecture

![NeuReg Architecture](assets/neuReg_model_diagram.png)

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

## 📂 Repository Structure

```text
NeuReg/
├── README.md                          # Project overview and pipeline explanation
├── LICENSE                            # Project license (Apache 2.0)
├── requirements.txt                   # Python dependencies

├── data/                              # Preprocessing and KG construction
│   ├── chunks/                        # Extracting regulatory text chunks
│   ├── ontology/                      # EFRO ontology schema and KG triples

├── qa_generation/                     # QA dataset generation using prompting
│   ├── Zero-shot.ipynb, One-shot.ipynb, Few-shot.ipynb
│   └── Output QA datasets & analysis reports

├── evaluation/                        # QA dataset evaluation modules
│   ├── ontology_guided/               # Ontology-KG validation
│   ├── llm_judges/                    # 5 LLM evaluation models
│   ├── humans/                        # Human annotation & reports
│   └── llm_vs_human/                  # Correlation analysis

├── analysis/                          # Statistical & readability analyses

├── ablations/                         # KG-only and Chunks-only QA experiments

├── fine_tuning/                       # Fine-tuning T5/FLAN-T5 experiments
│   ├── t5_small/, t5_base/, flan_t5_large/, etc.
│   └── results/                       # Fine-tuned metrics
