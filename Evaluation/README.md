# 🧪 Evaluation Module

This folder contains all scripts, evaluation results, and analysis reports used to assess the quality of the QA pairs generated in the **NeuReg** framework. Evaluation spans four core areas:
-  **Ontology-guided triple validation** using the  textaul chunks and EFRO 
-  **LLM-based scoring** using five state-of-the-art models
-  **Human-based annotation** with expert reviewers and quality rubrics
- **LLM–Human agreement analysis** using alignment metrics such as Exact Match and F1 Score

Each type of evaluation is organized into its own subfolder, with dedicated notebooks and result files.

---

## 1. Ontology-Guided KG Evaluation

To assess KG quality, we implement a two-layered LLM-based validation framework:
The first layer utilises LLM to compare triples to the source text, identifying false positives and false negatives.
The second layer verifies ontology conformity, assigning Pass/Fail labels and confidence scores. 
These layers yield precision, recall, F1-score, and semantic validity metrics

📂 **Folder**: `Ontology-Guided_KG_Evaluation/`  
📄 **Files**:
- `Evaluation.ipynb` – Triple validation notebook
- `evaluation_results.csv` – Triple-level validation results
- `evaluation_report.json` – Aggregated compliance statistics

📘 See `Ontology-Guided_KG_Evaluation/README.md` for more details.

---

## 2. LLM-Based Evaluation (LLM-as-a-Judge)

This module evaluates QA quality using five powerful language models as automated judges. Each LLM scores QA pairs on five metrics:

- **Relevance**
- **Accuracy**
- **Completeness**
- **Fluency**
- **KG Alignment**

📂 **Folder**: `LLM-as-a-Judge/`  
📁 **Subfolders** (one per model):
- `DeepSeek-R1-Distill-Llama-70B/`
- `LLaMA 3.3 70B/`
- `Mixtral-8x22B/`
- `Gemma-2 Instruct 27B/`
- `Qwen3-32B/`

Each subfolder includes:
- Evaluation notebook (e.g., `LLaMA 3.3 70B.ipynb`)
- Result CSVs for each prompting strategy: Zero-Shot, One-Shot, Few-Shot

📘 See `LLM-as-a-Judge/README.md` for model-specific details and structure.

---

## 3. LLM Results Aggregated Analysis on complete QA datasets (zero, one and few shots)

This folder contains statistical analyses comparisons across all LLMs, identifying score patterns and LLMs majority voting agreement.

📂 **Folder**: `llms results analysis/`  
📄 **Files**:
- `LLM results analysis.ipynb` –  metrics, comparisons, majority voting 
- `comprehensive_analysis_report.json` – Summary of LLM performance across metrics

📘 See `llms results analysis/README.md` for further insights.

---

## 4. Human-Based Evaluation

Human experts manually rated a representative 5% sample of QA pairs (160) using the same five metrics as LLMs. A stratified sampling approach ensured balanced coverage across:

- Prompting strategies (Zero-Shot, One-Shot, Few-Shot)
- Question types (Factual, Relational, Comparative, Inferential)

📂 **Folder**: `Human Judgements/`  
📄 **Files**:
- `Evaluation_Template.md` – Annotator instructions and scoring rubric
- `QA_Human_Eval_Stratified_5percent.csv` – Sampled QA pairs for annotation
- `QA_Sampling_Summary_Statistics.csv` – Sampling breakdown by category
- `QA_Stratified_Sampling_Visualization.png` – Visual distribution overview
- `stratified sampling method.ipynb` – Script for balanced stratified sampling
- `human results analysis.ipynb` – Human score analysis and statistics
- `human_evaluation_analysis_report.json` – Final metrics from human reviewers

📘 See `Human Judgements/README.md` for full details on annotation setup.

---

## 5. LLM vs Human Agreement

This module analyzes agreement between LLM-generated scores and human evaluations using quantitative measures:

- Exact Match %
- F1 Score

📂 **Folder**: `LLM vs Human/`  
📄 **Files**:
- `LLM vs Human.ipynb` – Analysis of score alignment between humans and LLMs
- `human_llm_comparison_results.csv` – Output metrics for each model

📘 See `LLM vs Human/README.md` for agreement methodology.

---

## 📌 Evaluation Summary

All evaluations are aligned with the same underlying QA pairs generated using:
- Zero-Shot prompting
- One-Shot prompting
- Few-Shot prompting

This modular structure allows:
- Method-wise performance comparison
- Cross-rater agreement analysis
- Semantic consistency checks via ontology

Together, these modules ensure transparency, reproducibility, and alignment with FAIR data principles.

---

📎 For prompting strategies, dataset generation, and full methodology, refer to the [project root README](../README.md).
