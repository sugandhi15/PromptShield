# PromptShield: A Multilingual Prompt-Injection Detection Dataset

**English | Hindi | Hinglish | 11-Class Classification | LLM Security**

PromptShield is a research project focused on detecting prompt-injection attacks across English, Hindi, and Hinglish. It explores the classification of malicious instructions, jailbreak attempts, information-extraction requests, and other attack behaviors that may compromise the intended behavior of large language models (LLMs).

This repository provides a **public sample of 300 annotated prompts**, with 100 examples per language, to support transparency, reproducibility, and further research in multilingual prompt-injection detection.

> **Note:** This repository contains a public sample of the dataset, not the complete research dataset.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Dataset Details](#dataset-details)
- [Label Schema](#label-schema)
- [Annotation Guidelines](#annotation-guidelines)
- [Dataset Structure](#dataset-structure)
- [Dataset Format](#dataset-format)
- [Languages and Code-Mixed Text](#languages-and-code-mixed-text)
- [Usage](#usage)
- [Research Applications](#research-applications)
- [Limitations](#limitations)
- [Contributing](#contributing)

---

## Overview

Prompt injection is a security challenge in which instructions supplied by users or embedded in external content attempt to override an LLM's intended instructions, bypass safeguards, or obtain protected information.

Detecting these attacks becomes more challenging in multilingual and code-mixed settings, where malicious intent may be expressed through different scripts, transliteration, spelling variations, or a mixture of languages.

PromptShield investigates this problem through an **11-class prompt taxonomy** comprising one Safe class and ten attack categories.

The annotation framework emphasizes the intended behavior of a prompt rather than the presence of individual keywords. This helps distinguish genuine attacks from benign educational discussions, security analyses, translations, and quoted examples.

## Key Features

- **Multilingual coverage:** English, Hindi, and Hinglish.
- **11-class taxonomy:** One Safe class and ten attack categories.
- **Binary and multiclass labels:** Supports safe-versus-attack classification and fine-grained attack categorization.
- **Intent-based annotation:** Labels are assigned according to the behavior requested or enacted, rather than keywords alone.
- **Context-aware decisions:** Considers quoted instructions, untrusted external content, false authority claims, and documented multi-turn context.
- **Public sample:** Includes 100 annotated examples per language.
- **Research-oriented design:** Intended to support exploration of multilingual prompt-injection detection and annotation practices.

## Dataset Details

| Property | Description |
|---|---|
| Project | PromptShield |
| Task | Prompt-injection detection and classification |
| Languages | English, Hindi, Hinglish |
| Public sample size | 300 prompts |
| Samples per language | 100 |
| Number of classes | 11 |
| Classification types | Binary and multiclass |
| Annotation approach | Guideline-based, intent-focused labeling |

The public sample demonstrates the dataset structure, language coverage, label taxonomy, and annotation conventions.

The equal number of examples per language does not imply that all classes are equally represented. Refer to the associated research paper for details about the complete dataset and experimental setup.

---

## Label Schema

PromptShield uses one primary class per prompt. The following table summarizes the 11 categories.

| ID* | Class | Description |
|---:|---|---|
| 0 | Safe | No actionable prompt-injection or attack behavior is being performed or requested. |
| 1 | Instruction Override | Attempts to ignore, replace, supersede, or disobey prior instructions. |
| 2 | System Prompt Exfiltration | Attempts to reveal hidden system/developer instructions or confidential configuration text. |
| 3 | Jailbreak/Roleplay Bypass | Uses an unrestricted persona, role-play, or bypass framing to evade safeguards. |
| 4 | Indirect Injection | Embeds malicious instructions in untrusted external content intended for the model to process. |
| 5 | Encoding/Obfuscation | Conceals malicious instructions using encoding, character substitution, spacing, or similar techniques. |
| 6 | Multi-Turn Escalation | Progressively builds an attack across documented conversational turns. |
| 7 | Data/Credential Exfiltration | Attempts to obtain passwords, API keys, tokens, private records, or other protected data. |
| 8 | Role Impersonation | Uses a false claim of trusted identity or authority to induce restricted actions. |
| 9 | Context Manipulation | Uses misleading contextual claims or false authorization to influence interpretation. |
| 10 | DoS/Prompt Flooding | Uses excessive or repetitive prompts to overload the model or disrupt normal operation. |

**\*Important:** Verify that the numeric IDs match the canonical label mapping used in the released dataset.

### Binary Classification

The binary label is derived from the primary class:

| Binary Label | Meaning |
|---:|---|
| 0 | Safe |
| 1 | Attack — any of the ten attack classes |

The binary label must remain consistent with the 11-class label. Classification should be based on intent and behavior, not keywords alone.

---

## Annotation Guidelines

PromptShield follows a structured annotation guideline designed to improve consistency and reproducibility across English, Hindi, and Hinglish.

### 1. Intent Over Keywords

Words such as `ignore`, `password`, `system prompt`, and `jailbreak` do not automatically make a prompt malicious. Annotators must read the complete prompt and determine what behavior it requests or enacts.

### 2. Safe Educational and Analytical Content

A prompt that asks for an explanation, translation, summary, or classification of a malicious example is not automatically an attack. It should be labeled Safe when it does not itself request or enact an attack behavior.

### 3. One Primary Class Per Prompt

Each prompt receives exactly one primary class. When multiple attack behaviors are present, the annotation guideline provides proposed precedence rules for selecting the primary class.

### 4. Direct Versus Indirect Injection

Direct instructions from a user are distinguished from malicious instructions embedded in untrusted documents, webpages, emails, transcripts, or other external content that the model is expected to process.

### 5. Specific Targets and Attack Mechanisms

Annotators distinguish attempts to extract hidden system instructions from attempts to obtain credentials or private data. Encoding and obfuscation are considered according to whether concealment is the defining attack mechanism.

### 6. Context and Multi-Turn Behavior

Multi-Turn Escalation is assigned only when preceding turns or a documented sequence provide sufficient evidence. An isolated prompt should not be labeled as a multi-turn attack without supporting context.

### 7. Language-Independent Interpretation

The same class definitions apply across English, Hindi, and Hinglish. Transliteration, spelling variations, deliberate typos, and unfamiliar scripts do not automatically make an attack Safe.

### 8. Ambiguity and Adjudication

Genuinely ambiguous or unintelligible examples should be flagged for review rather than assigned a speculative label. Where supported by the annotation records, original labels and adjudicated labels should be preserved separately.

For complete operational definitions, decision rules, precedence policies, and the annotator checklist, refer to the annotation guidelines provided with this project.

---

## Dataset Structure

The repository can be organized as follows:

```text
PromptShield/
├── README.md
├── annotation_guidelines.pdf
└── data
```

The filenames above illustrate a suggested structure. Actual filenames may differ depending on how the repository is organized.

## Dataset Format

The public sample is provided in CSV format, with one file per language.

### Recommended Fields

| Field | Description |
|---|---|
| `annotation_id` | Unique identifier for a prompt |
| `text` | Prompt text |
| `language` | English, Hindi, or Hinglish |
| `binary_label` | `0` for Safe, `1` for Attack |
| `primary_class` | Primary class name or canonical class ID |

The annotation guidelines additionally recommend fields for annotator identity, confidence, rationale, adjudication status, adjudicated class, adjudication reason, and guideline version where applicable.

Only fields actually included in the released files should be documented as present in the dataset.

---

## Languages and Code-Mixed Text

PromptShield covers three language settings:

- **English:** Prompts written in English.
- **Hindi:** Prompts expressed in Hindi, including the scripts represented in the dataset.
- **Hinglish:** Prompts mixing Hindi and English, including common transliteration and code-switching patterns.

The annotation framework focuses on semantic intent rather than language or script. The same underlying behavior should receive a consistent class even when expressed through different linguistic forms.

The public sample offers an initial view of this multilingual setting. It does not, by itself, establish coverage across all Hindi dialects, scripts, transliteration patterns, or code-mixed expressions.

---

## Usage

You can inspect the public sample using Python and pandas.

### Load the Dataset

```python
import pandas as pd

english = pd.read_csv("data/english.csv")
hindi = pd.read_csv("data/hindi.csv")
hinglish = pd.read_csv("data/hinglish.csv")

dataset = pd.concat(
    [english, hindi, hinglish],
    ignore_index=True
)

print("Total public examples:", len(dataset))
print(dataset.head())
```

### Inspect Class Distribution

```python
print(dataset["primary_class"].value_counts())
```

### Inspect Language Distribution

```python
print(dataset["language"].value_counts())
```

These examples assume the files and columns follow the illustrated structure. Update the file paths or column names if your actual schema differs.

---

## Research Applications

The public sample may be useful for:

- Understanding the PromptShield annotation taxonomy.
- Exploring multilingual prompt-injection examples.
- Prototyping binary and multiclass classification pipelines.
- Developing annotation tools and reviewing labeling decisions.
- Studying distinctions between safe discussion and actionable attack behavior.
- Designing preliminary experiments for multilingual LLM security research.

The sample is intended for exploration and prototyping. It should not be treated as a substitute for the complete dataset when conducting benchmark comparisons or drawing broad performance conclusions.

## Limitations

- **Limited public sample:** Only 100 prompts per language are included.
- **Potential class imbalance:** Equal sample sizes across languages do not imply equal representation of all 11 classes.
- **Annotation ambiguity:** Some prompts may support multiple interpretations or attack categories.
- **Language coverage:** The three language settings do not represent every dialect, script, or code-mixing pattern.
- **Context dependence:** Indirect Injection and Multi-Turn Escalation may require additional context to be interpreted correctly.
- **Detection is not prevention:** A classifier's prediction alone cannot guarantee that an LLM is protected against prompt injection.

Results obtained using this small public subset should be interpreted cautiously.

---

## Contributing

Suggestions, issue reports, and research discussions related to the annotation taxonomy, multilingual coverage, and dataset usability are welcome.

When proposing annotation changes, prioritize explicit decision rules, consistency, and reproducibility. Changes to class definitions or precedence rules should be documented and versioned rather than introduced solely to increase inter-annotator agreement.

## Acknowledgements

PromptShield is a research project exploring multilingual prompt-injection detection and annotation for LLM security.

For questions, suggestions, or research collaboration, please open an issue in this repository.
