## 🤖 Zero-Shot QA Generation Prompt
In zero-shot QA generation, the model is not provided with any exemplar QAs or demonstrations. Instead, it relies entirely on:

A structured task description specifying the question type
Prompt instructions tailored to that type (factual, relational, comparative, inferential)
Diversity constraints to encourage variation across generated questions
A regulatory text chunk 
Its corresponding ontology-guided knowledge graph (KG) triples)
This strategy evaluates the model’s capacity to generate policy-aligned, diverse questions without prior examples, testing its generalization and semantic alignment abilities using only instructional guidance and contextual input.

---
### 🧠 Zero-Shot Prompt Template

#### 🤖 ZERO-SHOT QA GENERATION TASK

**TASK TYPE:** `{QUESTION_TYPE}`

This task requires generating `{N}` diverse, high-quality questions aligned with the selected task type (`{QUESTION_TYPE}`), based on both the regulatory **context** and its corresponding **ontology-guided knowledge graph (KG) triples**.

---

#### 🎯 Task Description

{TASK_DESCRIPTION}

---

#### 🔁 Diversity Requirements

- Each question must be **unique** and ask about **different aspects**
- Use **varied** question starters and phrasing patterns
- Focus on **different entities, relationships, or information types**
- Avoid **repetitive structures** or similar wording
- Make each question **distinctly different** from others

---

#### 📝 Instructions

{QUESTION_TYPE-SPECIFIC_PROMPT}

---

#### 📄 Context Text

---

#### 🧠 Ontology-Guided Knowledge Graph Triples

---

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

Please generate exactly {N} diverse, high-quality {question_type} questions using the provided instructions, context, and triples.
```
---

### 🔍 Supported Question Types & Task Descriptions

| Type             | Description                                                                                                                                                                                                                                                                                                                                                                              |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Factual**      | Generate `{n}` detailed factual questions that require **specific** information from the context. <br> These questions should ask for concrete details, numbers, dates, names, or specific requirements mentioned in the text. <br> Focus on extracting **precise information** that can be directly answered from the provided context and **ontology-guided knowledge graph triples**. |
| **Relationship** | Generate `{n}` questions about **specific relationships or interactions** between entities in the context. <br> Focus on how different entities, organizations, processes, or concepts **connect, influence**, or **depend on each other** through the knowledge graph. <br> Each question should mention **at least two entities** and explore their connection via a relationship.     |
| **Comparative**  | Generate `{n}` questions comparing different **aspects, entities, or concepts** from the context. <br> These questions should highlight **differences**, **similarities**, or **contrasts** between funding types, eligibility requirements, processes, or organizational structures. <br> Use the knowledge graph to identify **comparable elements**.                                  |
| **Inferential**  | Generate `{n}` questions that require **reasoning, analysis, or synthesis** based on the context and the knowledge graph. <br> These questions combine multiple pieces of information to draw conclusions, predict outcomes, or identify implications. <br> They should involve **multiple knowledge graph triples** and go beyond surface-level retrieval.                              |
