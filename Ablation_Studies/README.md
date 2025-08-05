## 🔬 Ablation Studies – NeuReg QA Framework

This module contains the implementation, datasets, evaluation, and analysis of the **Ablation Studies** designed to isolate the contributions of **unstructured** (text-only) and **structured** (KG-only) inputs in QA generation. Both experiments use the One-Shot (OS) prompting strategy and follow the same architecture as the OS  NeuReg pipeline.

---
### 📁 Folder Structure

```text
Ablation Studies/
│
├── Ablation Study 1/
│ ├── chunks_only_qa_dataset.ipynb
│ ├── Ablation_1_chunks_only_analysis_report.json
│ └── Ablation_1_chunks_only_qa_dataset.json
│
├── Ablation Study 2/
│ ├── KG_only_qa_dataset.ipynb
│ ├── Ablation_2_kg_only_analysis_report.json
│ └── Ablation_2_kg_only_qa_dataset.json
│
├── Evaluation/
│ ├── chunks_only_Evaluation/
│ │ ├── DeepSeek-R1-Distill-Llama-70B/
│ │ ├── Gemma-2 Instruct (27B)/
│ │ ├── Llama 3.3 70B/
│ │ ├── mixtral-8x22b-instruct-v0.1/
│ │ └── Qwen3-32B/
│ │
│ └── KG_only_Evaluation/
│  ├── DeepSeek-R1-Distill-Llama-70B/
│  ├── Gemma-2 Instruct (27B)/
│  ├── Llama 3.3 70B/
│  ├── mixtral-8x22b-instruct-v0.1/
│  └── Qwen3-32B/
│
└── Results Analysis/
├── Chunks Only/
│ └── Chunks Only Evaluation Analysis.ipynb
└── KG Only/
 └── KG Only Evaluation Analysis.ipynb
```

---


---

###  🧪 Experiment Overview

To quantify the separate contributions of text and KG information, we conducted two controlled ablation studies:

- **Chunks-Only:** Uses only regulatory text chunks. All KG triples are removed.
- **KG-Only:** Uses only subject–predicate–object triples. Text chunks are excluded.

Both datasets were generated using **GPT-4 Turbo** with minor prompt adjustments specific to each input modality.

Each QA dataset contains questions generated using the same OS prompt structure and is evaluated by five state-of-the-art LLM judges.

---

## 📊 Results Summary

The table below presents the average QA quality scores (1–5 scale) across 5 evaluation metrics: Relevance, Accuracy, Completeness, Fluency, and KG Alignment.

| Model               | Chunks-Only (Ov) | KG-Only (Ov) | Agreement (C/K) |
|--------------------|------------------|--------------|-----------------|
| Mixtral-8x22B      | 4.91             | 4.69         | 90.8 / 75.4     |
| Qwen3-32B          | 4.78             | 4.21         | 87.4 / 71.1     |
| DeepSeek-R1        | 4.81             | 4.75         | 87.7 / 72.1     |
| Gemma-2-27B-IT     | 4.76             | 3.87         | 87.0 / 43.6     |
| LLaMA-3.3-70B      | 4.47             | 4.10         | 65.6 / 58.6     |
| **Average**        | **4.75**         | **4.33**     | **82.2 / 64.2** |

> KG Alignment is excluded from Chunks-Only as it contains no structured input.

---

###  📈 Key Insights

- **Chunks-Only** setting outperformed **KG-Only** across all metrics:
  - **Overall**: 4.75 vs. 4.33
  - **Relevance**: 4.91 vs. 4.54
  - **Accuracy**: 4.80 vs. 4.47
  - **Completeness**: 4.33 vs. 3.79
  - **Fluency**: 4.94 vs. 4.60
- Higher agreement among LLM judges for Chunks-Only (82.2%) vs. KG-Only (64.2%), indicating better interpretability and consistency when grounded in text.

---

###  🔁 Comparison to Full NeuReg QA

When compared to the full NeuReg QA system using both **Text + KG**, performance further improved:

| Metric        | Chunks-Only | KG-Only | NeuReg (Text + KG) |
|---------------|-------------|---------|---------------------|
| Relevance     | 4.91        | 4.54    | **4.912**           |
| Accuracy      | 4.80        | 4.47    | **4.803**           |
| Completeness  | 4.33        | 3.79    | **4.433**           |
| Fluency       | 4.94        | 4.60    | **4.963**           |
| KG Alignment  | N/A         | 4.26    | **4.747**           |
| **Overall**   | 4.75        | 4.33    | **4.772**           |

This confirms that combining symbolic knowledge with unstructured context results in the highest quality QA generation.

---



