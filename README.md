# Project 3: Context-Anchored Answering (RAG Basics)

**DecodeLabs Prompt Engineering Internship – Batch 2026**

## Overview

AI models can sound confident while answering from memory instead of from the document in front of them. In business settings such as HR, legal or finance, that is a risk. This project builds a **"closed-book" prompt**: the AI may answer only from a document supplied inside the prompt, must cite the exact paragraph it used, and must reply `Information Not Found` when the document does not contain the answer.

The prompt is in [`prompt.txt`](prompt3.txt).

## Objectives

Based on the Project 3 assignment, the goals were to:

- Write a prompt that restricts the AI to the provided reference text only.
- Add strict **negative constraints**, so the AI replies `Information Not Found` when the answer is not in the text.
- Require **exact paragraph citations** (such as `[Para 2]`) for every claim.

## Techniques Used

| Technique | What it does |
|---|---|
| **System role** | Sets the AI up as a "strict, objective analyst" instead of a general assistant. |
| **XML delimiters** | `<context>` holds the document and `<query>` holds the question, keeping instructions separate from data. |
| **Context locking** | Tells the AI to use only facts from the `<context>` block, even if it knows the answer from memory. |
| **Deflection protocol** | If the answer is not fully supported by the text, the AI must reply exactly `Information Not Found`, with no apology or filler. |
| **Source attribution** | Every sentence must end with its paragraph tag, such as `[Para 2]`. |
| **Instruction-injection guard** | Text inside `<context>` is treated as reference data, and instructions found there must be ignored. |

## Methodology

### 1. The reference document

A short, fictional company policy was written for testing, with each paragraph tagged:

```
[Para 1] Northwind Co. employees work a standard 40-hour week, Monday to Friday.
[Para 2] Employees are eligible for 20 days of paid time off (PTO) annually after their first year of employment.
[Para 3] Employees may work remotely up to two days per week with manager approval.
[Para 4] Expense reports must be submitted within 30 days of the purchase date.
[Para 5] The company offers health insurance to all full-time employees after 60 days of employment.
```

### 2. The prompt structure

The full prompt is in [`prompt.txt`](prompt3.txt). It has four parts: a `<system>` role, the `<context>` document, the `<query>` question, and six numbered rules covering context locking, the `Information Not Found` fallback, `[Para X]` citations, and output format.

### 3. How the tests were run

The same prompt and document were used each time, with only the text inside `<query>` changed.

## Results

| Test | Question | AI reply | Followed the rules? |
|---|---|---|---|
| 1 – Answer is in the document | How much PTO do I get after one year? | 20 days of paid time off (PTO) annually after your first year of employment. [Para 2] | Yes: answered from the document and cited `[Para 2]` |
| 2 – Answer is not in the document | [YOUR QUESTION] | Information Not Found | Yes: used the exact fallback phrase with no extra text |

### Screenshots

**Test 1: a valid question with a paragraph citation**

The AI answers from the document and ends with `[Para 2]`.

![Test 1 – answer with paragraph citation](1st-ss-p3.png)

**Test 2: a question the document cannot answer**

The AI replies with exactly `Information Not Found`.

![Test 2 – Information Not Found reply](3rd-ss-p3.png)

## Observations

- With the prompt, the AI answered an in-document question using only the document's wording and attached the correct paragraph tag.
- For a question the document cannot answer, the AI used the exact fallback phrase and added nothing else.
- Only two tests are shown here, with one run each, so this does not prove the prompt is reliable in every case. Further tests, such as questions where the AI knows the answer from memory or documents with misleading details, would be the next step.

## Project Structure

```
Project-3/
├── README.md       # Project documentation (this file)
├── prompt.txt      # The closed-book RAG prompt
├── 1st-ss-p3.png   #
