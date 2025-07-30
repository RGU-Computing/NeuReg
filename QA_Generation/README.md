## 🤖 Zero-Shot QA Generation Prompt

In Zero-Shot QA generation, the model is not given any exemplars or sample QAs to imitate. Instead, it relies entirely on:
A structured task description,
Specific prompt instructions for the chosen question type,
Diversity constraints to ensure varied outputs,
A text chunk from the regulatory document, and
Its corresponding ontology-guided knowledge graph (KG) triples.
This approach evaluates the model's ability to generate policy-aligned questions without prior demonstrations, testing generalization and alignment to regulatory semantics purely from instruction and context.

----

### 🧠 Zero-Shot Prompt Template

ZERO-SHOT QA GENERATION TASK

TASK TYPE: {QUESTION_TYPE}

{TASK_DESCRIPTION}

DIVERSITY REQUIREMENTS:
- Each question must be UNIQUE and ask about DIFFERENT aspects
- Use VARIED question starters and phrasing patterns
- Focus on DIFFERENT entities, relationships, or information types
- Avoid repetitive structures or similar wordings
- Make each question distinctly different from others

INSTRUCTIONS:
{QUESTION_TYPE-SPECIFIC_PROMPT}

CONTEXT TEXT:
{context[:1200]}{"..." if len(context) > 1200 else ""}

ONTOLOGY-GUIDED KNOWLEDGE GRAPH TRIPLES:
{triple_text}

REQUIRED OUTPUT FORMAT:
[
  {{
    "id": "1",
    "question": "Your detailed question here?",
    "answer": "Your comprehensive answer here.",
    "type": "{question_type}",
    "qa_metadata": {{
      "mentioned_entities": ["entity1", "entity2"],
      "mentioned_relationships": ["relationship1", "relationship2"]
    }}
  }}
]

Generate exactly {N} DIVERSE, high-quality {question_type} questions.

###  🔍 Supported Question Types & Task Descriptions

| Type             | Description                                                                                                                                                                                                                                                                                                                                                                              |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Factual**      | Generate `{n}` detailed factual questions that require **specific** information from the context. <br> These questions should ask for concrete details, numbers, dates, names, or specific requirements mentioned in the text. <br> Focus on extracting **precise information** that can be directly answered from the provided context and **ontology-guided knowledge graph triples**. |
| **Relationship** | Generate `{n}` questions about **specific relationships or interactions** between entities in the context. <br> Focus on how different entities, organizations, processes, or concepts **connect, influence**, or **depend on each other** through the knowledge graph. <br> Each question should mention **at least two entities** and explore their connection via a relationship.     |
| **Comparative**  | Generate `{n}` questions comparing different **aspects, entities, or concepts** from the context. <br> These questions should highlight **differences**, **similarities**, or **contrasts** between funding types, eligibility requirements, processes, or organizational structures. <br> Use the knowledge graph to identify **comparable elements**.                                  |
| **Inferential**  | Generate `{n}` questions that require **reasoning, analysis, or synthesis** based on the context and the knowledge graph. <br> These questions combine multiple pieces of information to draw conclusions, predict outcomes, or identify implications. <br> They should involve **multiple knowledge graph triples** and go beyond surface-level retrieval.                              |

