# 🧠 Ontology-Guided QA Generation using Large Language Models

This repository contains prompting strategies to generate high-quality, semantically grounded question-answer (QA) pairs from educational funding regulation documents using domain-aligned knowledge graphs.

---

## 🤖 Zero-Shot QA Generation Prompt

In zero-shot QA generation, the model receives no prior examples or demonstrations to imitate. Instead, it relies entirely on:

- A structured task description specifying the question type  
- Prompt instructions tailored to that type (factual, relational, comparative, inferential)  
- Diversity constraints to encourage variation across generated questions  
- A regulatory text chunk  
- Its corresponding ontology-guided knowledge graph (KG) triples  

This strategy evaluates the model’s capacity to generate policy-aligned, diverse questions without prior examples, testing its generalization and semantic alignment abilities using only instructional guidance and contextual input.

---

### 🧠 Zero-Shot Prompt Template

**TASK TYPE:** `{QUESTION_TYPE}`

This task requires generating `{N}` diverse, high-quality questions aligned with the selected task type (`{QUESTION_TYPE}`), based on both the regulatory **context** and its corresponding **ontology-guided knowledge graph (KG) triples**.

#### 🎯 Task Description

`{TASK_DESCRIPTION}`

#### 🔁 Diversity Requirements

- Each question must be **unique** and ask about **different aspects**
- Use **varied** question starters and phrasing patterns
- Focus on **different entities, relationships, or information types**
- Avoid **repetitive structures** or similar wording
- Make each question **distinctly different** from others

#### 📝 Instructions

`{QUESTION_TYPE-SPECIFIC_PROMPT}`

#### 📄 Context Text

`{context}`

#### 🧠 Ontology-Guided Knowledge Graph Triples

`{triple_text}`

### 🧾 Required Output Format (JSON)

```json
[
  {
    "id": "1",
    "question": "Your detailed question here?",
    "answer": "Your comprehensive answer here.",
    "type": "{question_type}",
    "qa_metadata": {
      "mentioned_entities": ["entity1", "entity2"],
      "mentioned_relationships": ["relationship1", "relationship2"]
    }
  }
]
```

---

### 🔍 Supported Question Types & Task Descriptions

| Type             | Description                                                                                                                                                                                                                                                                                                                                                                              |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Factual**      | Generate `{n}` detailed factual questions that require **specific** information from the context. <br> These questions should ask for concrete details, numbers, dates, names, or specific requirements mentioned in the text. <br> Focus on extracting **precise information** that can be directly answered from the provided context and **ontology-guided knowledge graph triples**. |
| **Relationship** | Generate `{n}` questions about **specific relationships or interactions** between entities in the context. <br> Focus on how different entities, organizations, processes, or concepts **connect, influence**, or **depend on each other** through the knowledge graph. <br> Each question should mention **at least two entities** and explore their connection via a relationship.     |
| **Comparative**  | Generate `{n}` questions comparing different **aspects, entities, or concepts** from the context. <br> These questions should highlight **differences**, **similarities**, or **contrasts** between funding types, eligibility requirements, processes, or organizational structures. <br> Use the knowledge graph to identify **comparable elements**.                                  |
| **Inferential**  | Generate `{n}` questions that require **reasoning, analysis, or synthesis** based on the context and the knowledge graph. <br> These questions combine multiple pieces of information to draw conclusions, predict outcomes, or identify implications. <br> They should involve **multiple knowledge graph triples** and go beyond surface-level retrieval.                              |

---

## 🧠 One-Shot QA Generation Prompt

In One-Shot prompting, the model is provided with a single exemplar QA for each question type (Factual, Relationship, Comparative, Inferential). Each exemplar includes:

- A shared regulatory context  
- Ontology-guided KG triples extracted from that context  
- A well-formed QA pair aligned with the KG  

The exemplar is shown before the model is prompted with a target context and KG, encouraging it to generalize the QA generation task from a single in-context demonstration.

---

### 🧠 One-Shot Prompt Template

```text
ONE-SHOT QA GENERATION TASK

TASK TYPE: {QUESTION_TYPE}

{TASK_DESCRIPTION}

EXEMPLAR DEMONSTRATION:

EXAMPLE CONTEXT:
{EXEMPLAR_CONTEXT}

EXAMPLE KNOWLEDGE GRAPH TRIPLES:
{EXEMPLAR_KG}

EXAMPLE {QUESTION_TYPE} QUESTION:
{EXEMPLAR_QUESTION}

NOW GENERATE FOR TARGET CONTEXT:

TARGET CONTEXT:
{CONTEXT_TEXT}

TARGET KNOWLEDGE GRAPH TRIPLES:
{TRIPLE_TEXT}

GENERATION INSTRUCTIONS: {GENERATION_GUIDANCE}

DIVERSITY REQUIREMENTS:
- Each question must be UNIQUE and ask about DIFFERENT aspects  
- Use VARIED question starters and phrasing patterns  
- Focus on DIFFERENT entities, relationships, or information types  
- Avoid repetitive structures or similar wordings  
- Make each question distinctly different from others and from the exemplar  

REQUIRED OUTPUT FORMAT:
[ ... ]
```

---

## 🧠 Few-Shot QA Generation Prompt

In Few-Shot prompting, the model is provided with **multiple exemplars** across all question types. Each exemplar includes:

- A unique regulatory context chunk  
- Ontology-guided KG triples  
- QA pairs for each of the four question types  

This design helps internalize diverse reasoning strategies and enables the model to generate semantically rich, diverse QA pairs.

---

### 🧠 Few-Shot Prompt Template

```text
FEW-SHOT QA GENERATION TASK

TASK TYPE: {QUESTION_TYPE}

{TASK_DESCRIPTION}

MULTIPLE EXEMPLAR DEMONSTRATIONS:
{ALL_EXEMPLARS_SHOWN}

NOW GENERATE FOR TARGET CONTEXT:

TARGET CONTEXT:
{CONTEXT_TEXT}

TARGET KNOWLEDGE GRAPH TRIPLES:
{TRIPLE_TEXT}

GENERATION INSTRUCTIONS: {GENERATION_GUIDANCE}

ENHANCED DIVERSITY REQUIREMENTS:
- Study the patterns from ALL examples above
- Each question must be UNIQUE and ask about DIFFERENT aspects
- Use VARIED question starters and phrasing patterns inspired by the examples
- Focus on DIFFERENT entities, relationships, or information types
- Avoid repetitive structures or similar wordings
- Make each question distinctly different from others and from ALL exemplars

REQUIRED OUTPUT FORMAT:
[ ... ]
```
