## 🔬 Ablation Studies

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
#### The main prompt structure for chunks-only QA generation:

TASK TYPE: {question_type.value.upper()}
{task_description}

EXEMPLAR DEMONSTRATION:

EXAMPLE CONTEXT:
{CHUNKS_ONLY_EXEMPLAR}

EXAMPLE {question_type.value.upper()} QUESTION:
{exemplar_question}

NOW GENERATE FOR NEW CONTEXT:

TARGET CONTEXT:
{context}

#### GENERATION INSTRUCTIONS: {generation_guidance}

IMPORTANT: Generate questions and answers based ONLY on the text context provided. 
Do not reference or require knowledge graph information.

#### 🔁 Diversity Requirements:
- Each question must be UNIQUE and ask about DIFFERENT aspects
- Use VARIED question starters and phrasing patterns  
- Focus on DIFFERENT concepts, relationships, or information types mentioned in the text
- Avoid repetitive structures or similar wordings
- Make each question distinctly different from others and from the exemplar

#### REQUIRED OUTPUT FORMAT:
```json
[
  {
    "id": "1",
    "question": "Your detailed question here?",
    "answer": "Your comprehensive answer here.",
    "type": "{question_type.value}"
  }
]
```

| Type             | Description                                                                                                                                                                                                                                                                                                                                                                              |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Factual**      | Generate factual questions that require **specific** information from the context. <br> These questions should ask for concrete details, numbers, dates, names, or specific requirements mentioned directly in the provided text. <br> Focus on extracting **precise information** that can be directly answered from the context **without external knowledge**. |
| **Relationship** | Generate questions about **relationships or interactions** between concepts, processes, or entities mentioned in the context. <br> Focus on how different elements **connect, influence**, or **interact with each other** based solely on the information provided in the text. <br> Each question should explore **connections between at least two concepts** from the context.     |
| **Comparative**  | Generate questions comparing different **aspects, concepts, or scenarios** from the context. <br> These questions should highlight **differences**, **similarities**, or **contrasts** between multiple items such as funding types, requirements, processes, or organizational structures mentioned in the text. <br> Use only information **available in the provided context**.                                  |
| **Inferential**  | Generate questions that require **analysis, reasoning, or inference** based on the context. <br> These questions should combine multiple pieces of information from the text to draw conclusions, identify implications, or predict outcomes. <br> They require **synthesizing information** from different parts of the context **without external knowledge**.                              |

----
  ####  Exemplar Context (Excerpt):

 "ESFA funds children who are currently electively home educated (EHE) who attend general further education (Further Education) and sixth form colleges. EHE children who attend schools and academies are not eligible for ESFA young people's funding. You can find more information in the Funding rates and formula guide. Colleges may claim ESFA young people's funding for children of compulsory school age who have completed their statutory education, have achieved qualifications at least equivalent to a full level 2, and who want to enrol on a level 3 course. Colleges do not need to seek approval from ESFA, as we will count these students for lagged funding purposes. This advice also applies to schools and academies placing students in their sixth forms earlier than usual. In exceptional circumstances, for example, students arriving in the UK for the first time during school year 11, ESFA will consider provision for individual students of compulsory school age to be eligible for ESFA young people's funding in colleges. Groups of students would not be eligible for funding, since by inference such circumstances are unlikely to be exceptional."

 Exemplar_Questions:
```json

  "FACTUAL": {
    "question": "question": "What type of students are not eligible for ESFA young people's funding if they attend schools and academies?"
  },
  "RELATIONSHIP": {
    "question": "How is a student's eligibility for ESFA funding connected to completing statutory education and enrolling in a level 3 course?"
  },
  "COMPARATIVE": {
    "question": "What is the difference in ESFA funding eligibility between students arriving in the UK individually versus in groups during school year 11?"
  },
  "INFERENTIAL": {
    "question": "Why might ESFA be more willing to fund individual students rather than groups of students who arrive in the UK during year 11?"
  }


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



