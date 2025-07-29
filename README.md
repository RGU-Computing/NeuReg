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

NeuReg/
├── data/                       # Data processing & ontology construction
│   ├── chunks/                 # Text chunking pipeline
│   ├── ontology/               # Knowledge graph extraction
│   └── ablations/              # Ablation study datasets
├── qa_generation/              # QA dataset generation
│   ├── Zero-shot.ipynb         # Zero-shot prompting
│   ├── One-shot.ipynb          # One-shot prompting
│   ├── Few-shot.ipynb          # Few-shot prompting
│   └── outputs/                # Generated datasets & analysis reports
├── evaluation/                 # Comprehensive evaluation suite
│   ├── ontology_guided/        # KG-based evaluation
│   ├── llm_judges/             # 5 LLM evaluators
│   ├── humans/                 # Human evaluation
│   └── llm_vs_human/           # Correlation analysis
├── analysis/                   # Statistical analysis & insights
├── ablations/                  # Ablation studies (chunks_only/kg_only)
└── fine_tuning/                # T5 & FLAN-T5 fine-tuning experiments
