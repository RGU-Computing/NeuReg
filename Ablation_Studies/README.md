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
ABLATION STUDY: CHUNKS-ONLY QA GENERATION TASK

TASK TYPE: {question_type}
{task_description}

EXEMPLAR DEMONSTRATION:

EXAMPLE CONTEXT:
{CHUNKS_ONLY_EXEMPLAR["context"]}

EXAMPLE {question_type.value.upper()} QUESTION:
{exemplar_question}

NOW GENERATE FOR NEW CONTEXT:

TARGET CONTEXT:
{context}

GENERATION INSTRUCTIONS: {generation_guidance}

IMPORTANT: Generate questions and answers based ONLY on the text context provided. 
Do not reference or require knowledge graph information.

DIVERSITY REQUIREMENTS:
- Each question must be UNIQUE and ask about DIFFERENT aspects
- Use VARIED question starters and phrasing patterns  
- Focus on DIFFERENT concepts, relationships, or information types mentioned in the text
- Avoid repetitive structures or similar wordings
- Make each question distinctly different from others and from the exemplar

REQUIRED OUTPUT FORMAT:
[
  {{
    "id": "1",
    "question": "Your detailed question here?",
    "answer": "Your comprehensive answer here.",
    "type": "{question_type.value}"
  }}
]

Chunks-Only Exemplar
Context:
ESFA funds children who are currently electively home educated (EHE) who attend general further education (Further Education) and sixth form colleges. EHE children who attend schools and academies are not eligible for ESFA young people's funding. You can find more information in the Funding rates and formula guide. Colleges may claim ESFA young people's funding for children of compulsory school age who have completed their statutory education, have achieved qualifications at least equivalent to a full level 2, and who want to enrol on a level 3 course. Colleges do not need to seek approval from ESFA, as we will count these students for lagged funding purposes. This advice also applies to schools and academies placing students in their sixth forms earlier than usual. In exceptional circumstances, for example, students arriving in the UK for the first time during school year 11, ESFA will consider provision for individual students of compulsory school age to be eligible for ESFA young people's funding in colleges. Groups of students would not be eligible for funding, since by inference such circumstances are unlikely to be exceptional.

Exemplar Questions by Type:
Factual Question:
What type of students are not eligible for ESFA young people's funding if they attend schools and academies?
Relationship Question:
How is a student's eligibility for ESFA funding connected to completing statutory education and enrolling in a level 3 course?
Comparative Question:
What is the difference in ESFA funding eligibility between students arriving in the UK individually versus in groups during school year 11?
Inferential Question:
Why might ESFA be more willing to fund individual students rather than groups of students who arrive in the UK during year 11?
Question Type Definitions
Factual Questions
Factual questions seek specific, concrete information that can be directly extracted from the text context. They typically start with 'What', 'When', 'Where', 'Who', or 'How much/many' and ask for precise details mentioned in the text.
Generation Guidance: Generate factual questions that require specific information from the context. These questions should ask for concrete details, numbers, dates, names, or specific requirements mentioned directly in the provided text. Focus on extracting precise information that can be directly answered from the context.
Relationship Questions
Relationship questions explore connections between concepts, actions, or items mentioned in the text. They examine how different elements interact, depend on each other, or influence one another based on the information provided in the context.
Generation Guidance: Generate questions about relationships or interactions between concepts, processes, or entities mentioned in the context. Focus on how different elements connect, influence, or interact with each other based solely on the information provided in the text.
Comparative Questions
Comparative questions examine differences and similarities between concepts, processes, or categories mentioned in the text. They help understand distinctions in requirements, procedures, amounts, or characteristics across different scenarios or instances.
Generation Guidance: Generate questions comparing different aspects, concepts, or scenarios from the context. These questions should highlight differences, similarities, or contrasts between multiple items such as funding types, requirements, processes, or organizational structures mentioned in the text.
Inferential Questions
Inferential questions require reasoning and synthesis of multiple pieces of information from the text. They ask for conclusions, implications, or predictions that must be derived by combining various facts and statements from the context.
Generation Guidance: Generate questions that require analysis, reasoning, or inference based on the context. These questions should combine multiple pieces of information from the text to draw conclusions, identify implications, or predict outcomes. They require synthesizing information from different parts of the context.


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



