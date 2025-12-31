# SemEval-2026 Task 12 Dataset  
**Abductive Event Reasoning (AER)**

This repository contains the dataset for **SemEval-2026 Task 12: Abductive Event Reasoning (AER)**, a shared task focused on evaluating language models’ ability to perform real-world event causal inference.  
The task requires systems to identify the most plausible direct cause of a given event based on retrieved contextual documents.

SemEval is an international semantic evaluation workshop series that provides standardized tasks and shared datasets for NLP research. Task 12 is part of the SemEval-2026 workshop lineup. :contentReference[oaicite:0]{index=0}

---

## 📂 Repository Structure
```
.
├── dev_data
├── README.md
├── sample_data
├── test_data
└── train_data
```
Each data split consists of two files:
- `questions.jsonl`: event descriptions and multiple-choice options  
- `docs.json`: retrieved contextual documents for each event

---

## 📌 Data Format
### Questions File (`questions.jsonl`)

Each line in `questions.jsonl` is a JSON object representing one multiple-choice reasoning instance.

```json
{
  "topic_id": 11,
  "id": "q-101",
  "target_event": "Short description of an observed event.",
  "option_A": "Candidate explanation A.",
  "option_B": "Candidate explanation B.",
  "option_C": "Candidate explanation C.",
  "option_D": "Candidate explanation D.",
  "golden_answer": "A,B"
}
```

Fields:
- `id`: Unique identifier of the instance (used for evaluation and submission).
- `topic_id`: Identifier linking the instance to its contextual documents.
- `target_event`: A short textual description of the observed real-world event.
- `option_A – option_D`: Four candidate explanations written in natural language.
- `golden_answer:`
    - Present in sample, train, and dev splits.
    - Contains one or more correct option labels, comma-separated (e.g., "A,B").
    - Removed in the test split.

### Context File (docs.json)
Each record in the docs file contains background context for an event:
```json
{
  "topic_id": 11,
  "topic": "OpenAI releases ChatGPT.",
  "docs": [
    {
      "title": "Article title",
      "id": "doc-001",
      "link": "https://example.com",
      "snippet": "Short summary of the document.",
      "source": "News source",
      "imageUrl": "Base64imageUrl",
      "content": "Full document text."
    }
  ]
}
```

Fields:
- `topic_id`: Topic identifier shared with questions.jsonl.
- `topic`: A short description of the topic or event cluster.
- `docs`: A list of retrieved documents, which may include distractors.
    - `title`: Document title.
    - `id`: Document identifier.
    - `link`: URL of the original source.
    - `snippet`: Short excerpt or summary.
    - `source`: Name of the data source.
    - `imageUrl`: Optional image URL associated with the document.
    - `content`: Full textual content of the document.

## 🧠 Dataset Usage
### Training / Validation (Train & Dev)
All instances include answer (the gold labels).
Used for training and tuning models.

### Evaluation (Eval)
The answer field is removed.
Models must produce predictions in the format required by the evaluation script.

## 📏 Evaluation Metric
System performance is evaluated at the **instance level** using an exact and partial matching scheme over the predicted answer options.

Let **G** denote the set of gold-standard correct options for an instance, and **P** denote the set of options predicted by the system.

Each instance is scored as follows:

- **1.0 (Full Match)**: if *P = G*  
- **0.5 (Partial Match)**: if *P* is a non-empty proper subset of *G* (i.e., the prediction covers at least one correct option and contains no incorrect options)  
- **0.0 (Incorrect)**: otherwise, including cases where the prediction contains any incorrect option or is empty  

The final system score is computed as the **average score across all evaluation instances**.

## 🆚 Codabench Competition
The official evaluation for SemEval-2026 Task 12 is hosted on Codabench.

🔗 Competition page:
https://www.codabench.org/competitions/12440/

Participants are required to submit prediction files following the competition guidelines.