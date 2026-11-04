# PromptShield: A Multilingual Prompt-Injection Detection Dataset

**English | Hindi | Hinglish | 11-Class Classification | LLM Security**

PromptShield is an academic study whose aim is the detection of prompt injection attacks in English, Hindi, and Hinglish. It investigates the classification of malicious instructions, jailbreaking attacks, requests for information extraction, and other types of attacks which could potentially cause deviation from the intended behavior of LLMs.

In this repository, you can find a set of 300 samples of annotated prompts, each language has 100 samples.

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

Prompt injection is a security issue where instructions provided by users or in external sources try to subvert the LLM’s intended instructions or acquire sensitive information.

Identifying such attacks becomes harder in multilingual or code-mixed contexts, since malicious intent can be communicated in different writing systems, via transliteration, misspellings, or mixed languages.

PromptShield researches this problem by developing an **11-class prompt taxonomy** that includes one Safe category and ten attacks.

The annotation process focuses on the desired behavior of a prompt rather than presence of specific keywords. It allows distinguishing between real attacks and non-threatening conversations regarding education, security analysis, translation, etc.

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

Equal number of examples for each language doesn’t mean that there is an equal representation of classes. You can refer to the research paper for further information on the whole dataset.

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

Terms like `ignore`, `password`, `system prompt`, `jailbreak`, for example, do not imply that the prompt is inherently malicious. The annotators have to go through the full prompt and see what behavior is required.

### 2. Safe Educational and Analytical Content

A question that requests an explanation, translation, summary, or classification of a harmful example cannot be considered an attack by default. It can be classified as Safe if it does not contain any elements of an attack behavior itself.

### 3. One Primary Class Per Prompt

One and only one primary class is identified for each prompt. In case there is more than one attacking behavior, a suggested precedence rule is included in the annotation guide.

### 4. Direct Versus Indirect Injection

User's direct commands are separated from malicious commands which have been planted within untrusted documents, web pages, email messages, transcripts or any other external material which the system is supposed to parse.

### 5. Specific Targets and Attack Mechanisms

Annotators differentiate attacks aimed at getting confidential system instructions from attacks that seek to acquire credentials or confidential data. Encoding/obfuscation is taken into account if it is the primary attack technique used.

### 6. Context and Multi-Turn Behavior

Only in the case where there is enough evidence from prior turns or a sequence that has been recorded is Multi-Turn Escalation assigned. Isolated prompts must never be classified as Multi-Turn attacks.

### 7. Language-Independent Interpretation

Definitions of classes remain constant irrespective of whether the language used is English, Hindi, or Hinglish. Transliteration, variations in spelling, intentional misspellings, and foreign script alone do not make an attack safe.

### 8. Ambiguity and Adjudication

Such examples that are truly ambiguous or not understandable should be marked for review and not labeled. Where the labeling can be justified by the annotation information, the original labels as well as the adjudicated labels should be retained.

For complete definitions of operation, decision rule, precedence policy, and the annotator checklist, please see the annotation guidelines accompanying this project.

---

## Dataset Structure

```text
PromptShield/
├── README.md
├── annotation_guidelines.pdf
└── data
```

The `data` file contains the public dataset sample, including examples in English, Hindi, and Hinglish.

## Dataset Format

The dataset is provided as a single file containing examples from all three language settings.

| **Field**       | **Description**                                     |
| --------------- | --------------------------------------------------- |
| `annotation_id` | Unique identifier for a prompt                      |
| `text`          | Prompt text                                         |
| `language`      | Language of the prompt: English, Hindi, or Hinglish |
| `binary_label`  | `0` for Safe, `1` for Attack                        |
| `primary_class` | Primary class name or canonical class ID            |

*Note: The column names above are illustrative. Refer to the actual dataset schema for the exact field names.*

---

## Usage

You can load the complete public sample using Python and pandas.

### 1. Load the Dataset

```python
import pandas as pd

# Load the complete dataset
dataset = pd.read_csv("data")

print("Total examples:", len(dataset))
print(dataset.head())
```

### 2. Explore Language Distribution

```python
print(dataset["language"].value_counts())
```

This displays the number of examples available for each language.

### 3. Filter Examples by Language

```python
# English examples
english = dataset[dataset["language"] == "English"]

# Hindi examples
hindi = dataset[dataset["language"] == "Hindi"]

# Hinglish examples
hinglish = dataset[dataset["language"] == "Hinglish"]

print("English:", len(english))
print("Hindi:", len(hindi))
print("Hinglish:", len(hinglish))
```

### 4. Explore Class Distribution

```python
print(dataset["primary_class"].value_counts())
```

To examine class distribution separately for each language:

```python
print(
    dataset.groupby(["language", "primary_class"])
    .size()
    .unstack(fill_value=0)
)
```

### 5. Explore Binary Labels

```python
print(dataset["binary_label"].value_counts())
```

The binary label uses `0` for Safe and `1` for Attack, consistent with the 11-class annotation scheme.

---

## Research Applications

The public sample can be used for the following purposes:

- For learning about PromptShield annotation scheme taxonomy.
- For examining multilingual prompt injection cases.
- For implementing binary & multiclass classification pipelines prototypes.
- For creating annotation tools and reviewing labeled data.
- For studying the differences between safe conversation & actionable attacks.
- For creating experiments prototypes for multilingual LLM security research.

It is meant for exploratory & prototyping work, and cannot be considered as an alternative to the full dataset for performing benchmarking & performance evaluation tasks.

## Limitations

- **Small public sample size:** A mere 100 prompts per language are used.
- **Imbalanced classes in public data:** Balanced number of samples in each language does not mean balanced representation of 11 classes.
- **Annotating issues:** Some prompts might lend themselves to several annotations or attack types.
- **Language limitation:** These three language configurations are just the example of all possible languages and codes.
- **Context-based detection methods:** Indirect Injection and Multi-Turn Escalation might need further contextual information for their proper understanding.
- **Detection and prevention difference:** A classifier's classification result does not provide protection of LLM from the prompt injection itself.

These results should be carefully analyzed due to the limited use of public dataset.

---

## Contributing

Discussion on the annotation taxonomy, multi-language support, and data usefulness is appreciated. 

In suggesting modifications for annotations, consider clear decision rules, consistency, and reproducibility. Modifying the classes themselves or their precedence rules cannot simply be done based on improving the rate of agreement.

## Acknowledgements

PromptShield is a research project exploring multilingual prompt-injection detection and annotation for LLM security.

For questions, suggestions, or research collaboration, please open an issue in this repository.
