# NeuReg QA Evaluation – Human Rating of Model-Generated QA Pairs

## Evaluator Instructions

You will evaluate each QA pair on five quality metrics, using a 1–5 scale, where:
- 5 = Excellent
- 4 = Good  
- 3 = Fair
- 2 = Poor
- 1 = Very Poor

Each QA pair includes:
- The model-generated question
- Its corresponding answer
- A source text chunk from which the QA is derived
- A set of Knowledge Graph (KG) triples extracted from the same chunk

## Metric Definitions

### 1. Relevance (1–5)
Does the question appropriately relate to the source text?

- **5**: Perfectly relevant to the source, clearly grounded in the text
- **4**: Mostly relevant, with minor off-topic elements
- **3**: Addresses the main question but misses some important points
- **2**: Loosely related, with significant tangents or irrelevance
- **1**: Entirely irrelevant or unrelated to the source content

### 2. Accuracy (1–5)
Is the answer factually correct based on the source text?

- **5**: All facts are accurate and fully verifiable in the context
- **4**: Mostly accurate; contains only minor factual issues
- **3**: Some factual inconsistencies or assumptions
- **2**: Several factual errors that affect reliability
- **1**: Mostly inaccurate or misleading information

### 3. Completeness (1–5)
Does the answer fully address the question?

- **5**: Thorough and complete response
- **4**: Covers most parts but misses minor aspects
- **3**: Addresses main part, omits some key details
- **2**: Partial answer with significant gaps
- **1**: Severely incomplete or off-topic

### 4. Fluency (1–5)
Is the answer well-written and grammatically correct?

- **5**: Excellent grammar and clarity; highly readable
- **4**: Minor grammatical or structural issues
- **3**: Understandable, but contains noticeable language errors
- **2**: Somewhat unclear due to poor grammar or phrasing
- **1**: Difficult to read or understand

### 5. KG Alignment (1–5)
How well does the answer reflect the relationships and facts expressed in the Knowledge Graph (KG) triples?

- **5**: Effectively uses KG relationships; no contradictions with KG facts; may appropriately include additional relevant information from the source text
- **4**: Uses most relevant KG information correctly; may omit minor KG details, but contains no contradictions
- **3**: Uses some KG information; may miss important relationships or entities, but is generally consistent with the KG
- **2**: Limited use of KG information; may contain minor contradictions or misinterpretations of KG facts
- **1**: Ignores KG information entirely or includes clear contradictions with KG relationships

## Question Types

### Factual
Asks for concrete information such as what, when, who, or how many
Example: "Who is responsible for funding post-16 education in the 2021/2022 academic year?"

### Relational
Focuses on connections between entities, such as institutions, policies, or statuses
Example: "How is the Secretary of State related to the ESFA's funding decisions?"

### Comparative
Involves comparing two or more items, highlighting similarities or differences
Example: "What are the differences between primary and secondary education funding?"

### Inferential
Requires reasoning across multiple facts
Example: "If a learner withdraws early, how might that affect the funding claim?"

## Shot Types

### Zero-Shot
The model receives only task instructions, the source text chunk, and associated KG triples without seeing any example questions. This tests the model's ability to generate relevant and accurate QA pairs based purely on instructions and content.

### One-Shot
The model is shown one example question before generating a new question for the given source context and KG triples. This allows the model to infer the question format and intent from a single exemplar.

### Few-Shot
The model is shown three example questions before generating a new one from the same kind of input. This helps the model learn a consistent style and structure from multiple exemplars.

## Knowledge Graph Format

A Knowledge Graph is a structured representation of information showing how entities relate to one another. Each piece of information in a KG is represented as a triple, consisting of:

- **Subject**: The entity being described
- **Predicate**: The relationship or property
- **Object**: The entity or value being connected

This structure follows the format:
```
Subject → Predicate → Object
```

Example Triple: Secretary of State for Education → provides_funding → Education Provision

This means: The Secretary of State for Education provides funding for education provision.
