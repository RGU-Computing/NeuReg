# 🧠 Ontology-Guided Knowledge Graph Construction

This project begins by using GPT-4 Turbo to extract triples from educational funding regulatory (EFR) documents, guided by a domain-specific ontology. The extracted knowledge graph is structured, compact, and aligned with the Educational funding regulation ontology (**EFRO**).

🔗 **EFR Ontology File**:  [EFRO.rdf on GitHub](https://github.com/RGU-Computing/EFRO/tree/main)

   **EFR Ontology URL**:  (http://www.w3id.org/efro/efro)
   
---

## 💬 Prompt for EFRO guided KG Extraction

> You are a knowledge extraction model that converts EFR text into triples.  
>
> Below is a text chunk from an EFR document:  
>
> ```
> {text_chunk}
> ```
>
> Use the ontology schema below to guide your extraction:
>
> ```
> {ontology_schema}
> ```
>
> ---
>
> **Example:**
>
> Text:  
> Institutions must assume that EEA students have the right to remain in the UK. Once a student is enrolled, the institution is expected to take all reasonable steps to ensure that the student can complete their programme.
>
> Triples:
> ```
> (institution, has_assumption, eea_students_right_to_remain)  
> (student, is_enrolled_in, study_programme)  
> (institution, has_legal_duty, ensure_student_completion)  
> (institution, provides_support, student)  
> ```
>
> ---
>
> Now, extract all triples from the provided chunk in the format:  
> `(subject, predicate, object)`
>
> **Guidelines:**
> - Do not use full sentences or long descriptions as object values.  
> - Normalize object values into short, meaningful phrases or identifiers  
>   (e.g., `GDPR_compliance`, `funding_16_19`, `right_to_remain`).

---

# 📈 Chunk & Triple Statistics
The following statistics summarise the structure and textual complexity of the extracted knowledge graph:

## 🧩 Chunk-Level Statistics
These reflect how triples are distributed across document segments:

| Metric                 | Value |
| ---------------------- | ----- |
| Total Chunks           | 154   |
| Mean Triples per Chunk | 12.43 |
| Std. Deviation         | 4.10  |
| Minimum                | 5     |
| 25th Percentile        | 10    |
| Median                 | 12    |
| 75th Percentile        | 14    |
| Maximum                | 29    |

---

## 📊 Knowledge Graph Summary

The dataset has been processed into a structured knowledge graph extracted from educational funding regulations. Below is a summary of key statistics:

## 📏 Triple Component Lengths (Avg. Character Count)
These values describe the average character length of each triple component:

| Component | Average Length |
| --------- | -------------- |
| Subject   | 17.16 chars    |
| Predicate | 14.73 chars    |
| Object    | 22.50 chars    |

## 🧩 General Graph Statistics

- **Total Triples**: 1,865  
- **Unique Chunks**: 150  
- **Unique Subjects**: 784  
- **Unique Predicates**: 533  
- **Unique Objects**: 1,475  
- **Average Triples per Chunk**: 12.43  
- **Min Triples per Chunk**: 5  
- **Max Triples per Chunk**: 29  

---

## 📚 Frequent Subjects (Top Entities)

| Subject              | Count |
| -------------------- | ----- |
| `institution`        | 141   |
| `student`            | 104   |
| `institutions`       | 63    |
| `esfa`               | 60    |
| `study_programme`    | 25    |
| `full_time_student`  | 20    |
| `learning_agreement` | 17    |
| `planned_hours`      | 14    |
| `provider`           | 14    |
| `funding_element`    | 13    |

---

## 🔁 Frequent Predicates (Top Relations)

| Predicate                     | Count |
| ----------------------------- | ----- |
| `includes`                    | 159   |
| `has_legal_duty`              | 105   |
| `requires`                    | 49    |
| `has_description`             | 45    |
| `is_funded_by`                | 40    |
| `participates_in`             | 40    |
| `provides_assistance`         | 37    |
| `enrolled_in`                 | 27    |
| `corresponds_to_learning_aim` | 26    |
| `is_member_of`                | 26    |

---

## 🧵 Frequent Objects (Top Values)

| Object                        | Count |
| ----------------------------- | ----- |
| `true`                        | 32    |
| `european_union`              | 28    |
| `esfa`                        | 19    |
| `study_programme`             | 14    |
| `eu_member_state_territories` | 12    |
| `institution`                 | 11    |
| `uk_education_funding`        | 9     |
| `educational_funding`         | 9     |
| `institutions`                | 8     |
| `false`                       | 8     |

---

## 📐 Statistical Summary of Triples per Chunk

| Metric        | Value |
|---------------|-------|
| Count         | 150   |
| Mean          | 12.43 |
| Std. Dev.     | 4.10  |
| Min           | 5     |
| 25% Percentile| 10    |
| Median (50%)  | 12    |
| 75% Percentile| 14    |
| Max           | 29    |

---

## 🔠 Token Length (Avg. Character Count)

- **Subject Length**: 17.16  
- **Predicate Length**: 14.73  
- **Object Length**: 22.50
