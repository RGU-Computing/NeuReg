# 🤖 QA Generation Framework Overview

This contains prompting strategies to generate high-quality, semantically grounded question-answer (QA) pairs from educational funding regulation documents using Ontology guided knowledge graphs.

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
- A well-formed  question aligned with the chunk + KG  

---
 ### Shared Exemplar

 Exemplar Context (Excerpt):

 "ESFA funds children who are currently electively home educated (EHE) who attend general further education (Further Education) and sixth form colleges. EHE children who attend schools and academies are not eligible for ESFA young people's funding. You can find more information in the Funding rates and formula guide. Colleges may claim ESFA young people's funding for children of compulsory school age who have completed their statutory education, have achieved qualifications at least equivalent to a full level 2, and who want to enrol on a level 3 course. Colleges do not need to seek approval from ESFA, as we will count these students for lagged funding purposes. This advice also applies to schools and academies placing students in their sixth forms earlier than usual. In exceptional circumstances, for example, students arriving in the UK for the first time during school year 11, ESFA will consider provision for individual students of compulsory school age to be eligible for ESFA young people's funding in colleges. Groups of students would not be eligible for funding, since by inference such circumstances are unlikely to be exceptional."

Exemplar KG Triples:

        ("esfa", "funds", "ehe_children_further_education"),
        ("ehe_children_further_education", "enrolled_in", "further_education_colleges"),
        ("ehe_children_further_education", "enrolled_in", "sixth_form_colleges"),
        ("ehe_children_schools_academies", "not_eligible_for", "esfa_young_people_funding"),
        ("funding_rates_and_formula_guide", "provides_information_on", "esfa_funding_details"),
        ("colleges", "can_claim", "esfa_young_people_funding"),
        ("esfa_young_people_funding", "for_programme", "level_3_course"),
        ("children_compulsory_school_age", "has_achievement_status", "full_level_2_qualification"),
        ("esfa", "does_not_require_approval_from", "colleges_for_lagged_funding"),
        ("schools_and_academies", "applies_same_advice_as", "colleges_for_early_sixth_form_placement"),
        ("esfa", "considers_funding_eligibility_for", "individual_students_compulsory_school_age"),
        ("individual_students_compulsory_school_age", "has_reason", "arriving_in_uk_during_school_year_11"),
        ("groups_of_students", "not_eligible_for", "esfa_young_people_funding_due_to_non_exceptional_circumstances")

Exemplar_Questions:
```json
{
  "Factual": {
    "question": "What types of institutions can claim ESFA young people's funding for electively home educated (EHE) children?"
  },
  "Relationship": {
    "question": "How is a full Level 2 qualification related to eligibility for Level 3 ESFA-funded programmes for compulsory school-age students?"
  },
  "Comparative": {
    "question": "How does ESFA funding eligibility differ between individual and groups of students arriving in the UK during school year 11?"
  },
  "Inferential": {
    "question": "Based on the information provided, what is the underlying reason ESFA distinguishes between individual and groups of students when considering funding for those arriving in the UK during school year 11?"
  }
}

```
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
[ ... ] 

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

 ### Shared Exemplar

1: Exemplar Context (Excerpt):

"In determining student eligibility, institutions must also satisfy themselves that there is a reasonable likelihood that the student will be able to complete their study programme before seeking funding for the student. This should include the practicality of providing a place for a student who may be unable to complete their programme if they are likely to leave the country permanently during their study programme. For the purposes of this paragraph, institutions must assume that all European Economic Area students resident in the UK before 1 January 2022 have the legal right to remain in the UK for the duration of their study programme. Once a student is enrolled, the institution is expected to take all reasonable steps to ensure that the student can complete their programme."

Exemplar KG Triples:

            ("institution", "has_legal_duty", "verify_student_eligibility"),
            ("institution", "is_expected_to_ensure", "student_program_completion"),
            ("student", "enrolled_in", "study_programme"),
            ("institution", "must_evaluate", "student_completion_likelihood"),
            ("student", "potential_withdrawal_reason", "likely_permanent_departure"),
            ("eea_student", "has_legal_right", "remain_in_uk_during_study"),
            ("eea_student", "has_status", "resident_in_uk_before_20220101"),
            ("institution", "provides_assistance", "student_completion_support")

Exemplar_Questions:
```json
QuestionType.FACTUAL: {
                "question": "What must institutions assume about EEA students who were resident in the UK before 1 January 2022?"
            },
            QuestionType.RELATIONSHIP: {
                "question": "How is a student's likelihood of permanent departure related to their eligibility for funding?"
            },
            QuestionType.COMPARATIVE: {
                "question": "How does the institution's responsibility differ before and after a student is enrolled in a study programme?"
            },
            QuestionType.INFERENTIAL: {
                "question": "Why might institutions be discouraged from enrolling students who are likely to leave the UK permanently during their study programme?"
            }
```

2: Exemplar Context (Excerpt):

"Students who are attending programmes of more than one term's duration, and are eligible for funding at the start of their programme, will usually be eligible for funding for the whole duration of their study programme as well as subsequent funded study programmes studied immediately end-on to their initial funded programme. This includes students studying consecutive study programmes with no break in studies other than normal holiday periods. Similarly, students who are not eligible for funding at the start of their study programme are very unlikely to become eligible for funding during the period of their study programme.",

Exemplar KG Triples:

            ("student", "has_funding_start_status", "eligible_at_programme_start"),
            ("student", "enrolled_in", "study_programme"),
            ("study_programme", "has_duration_value", "more_than_one_term"),
            ("study_programme", "has_funding", "funded_for_duration"),
            ("funded_for_duration", "includes", "subsequent_funded_programmes"),
            ("subsequent_funded_programmes", "has_temporal_value", "immediately_end_on"),
            ("student", "participates_in", "consecutive_study_programmes"),
            ("consecutive_study_programmes", "has_time_period", "no_break_other_than_holidays"),
            ("student", "has_funding_start_status", "not_eligible_at_programme_start"),
            ("student", "related_to_funding_status", "unlikely_to_become_eligible_during_programme")

Exemplar_Questions:

3: Exemplar Context (Excerpt): 

"context": "For the Prince's Trust Team Programme, the institution overhead rate (management fee) should be no more than a maximum of 15 per cent of the total ESFA funding. Any figure above 15 per cent will require prior approval from ESFA in collaboration with the Prince's Trust. For the purpose of the condition of funding, ESFA recognise that the Team Programme will support young people to progress towards General Certificate of Secondary Education standard and has been approved as a stepping stone towards a General Certificate of Secondary Education in these subjects.",

Exemplar KG Triples:

            ("esfa", "provides_funding", "princes_trust_team_programme"),
            ("princes_trust_team_programme", "has_funding_condition", "maximum_management_fee_15_percent"),
            ("maximum_management_fee_15_percent", "requires", "prior_approval_from_esfa_above_15_percent"),
            ("prior_approval_from_esfa_above_15_percent", "involves", "collaboration_with_princes_trust"),
            ("esfa", "recognizes", "princes_trust_team_programme_as_stepping_stone"),
            ("princes_trust_team_programme", "supports_learning", "general_certificate_of_secondary_education_standard"),
            ("princes_trust_team_programme", "has_progression", "general_certificate_of_secondary_education")

Exemplar_Questions:

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

#### 🔁 Diversity Requirements

[ ... ]

REQUIRED OUTPUT FORMAT:
[ ... ]
```
