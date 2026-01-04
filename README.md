# End-to-End Arabic Summarization — Synthetic Data + SLM Fine-Tuning (Qwen2.5-3B)

This project builds an Arabic summarization Small Language Model (SLM) by:
1) creating a synthetic (text, summary) dataset from raw Arabic articles via LLM API calls, then  
2) fine-tuning a lightweight model using parameter-efficient fine-tuning (LoRA/QLoRA) with Unsloth.

---

## Goal
Fine-tune an Arabic-capable SLM to summarize long-form Arabic news/articles.

---

## Dataset
- Source corpus: RightNow Arabic LLM corpus (subset of **~5,000 documents**).
- Output: Synthetic summaries generated via **Gemma-3-27B-IT (Google AI Studio)** to create (Text, Summary) pairs.

---

## Pipeline
### 1) Synthetic data generation (LLM API)
- Implemented traffic shaping to respect free-tier constraints (RPM / TPM / RPD)
- Truncation + dynamic throttling + retries/backoff + key rotation for reliability

### 2) Fine-tuning (PEFT)
- Model: **Qwen2.5-3B-Instruct**
- Framework: **Unsloth** (memory-efficient, faster training)
- Method: **QLoRA 4-bit + LoRA (r=16, alpha=16)** targeting linear projection modules
- Train context: 2048 tokens; inference context: up to 8192 tokens

### 3) Evaluation
Evaluated on a held-out test set using:
- **ROUGE** (lexical overlap)
- **BERTScore** (semantic similarity)
- **LLM-as-a-Judge** (qualitative scoring)

---

## Results
- ROUGE-1: **0.2967** | ROUGE-2: **0.1272** | ROUGE-L: **0.2828**
- BERTScore F1: **0.7497**
- LLM-as-a-Judge (30 samples): **8.44/10**

---

## Tech Stack
Python, Hugging Face, Unsloth, LoRA/QLoRA (4-bit), Google AI Studio (Gemma-3-27B-IT), ROUGE, BERTScore

---

## How to Run
1. Open the notebook: `Arabic_Summarization.ipynb`
2. Run data synthesis (API step) to build (text, summary) pairs
3. Run fine-tuning with Unsloth + QLoRA
4. Run evaluation cells (ROUGE/BERTScore/Judge)

---

## Repo Structure
.
├── Arabic_Summarization.ipynb
├── Rapport.pdf
└── README.md
