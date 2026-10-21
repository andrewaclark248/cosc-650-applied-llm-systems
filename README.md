# Applied LLM Systems

## Purpose

The purpose of this repository is to serve as my lab notebook and portfolio for the duration of the course.

This is an Applied LLM Systems course that covers tokenization, prompt engineering, function calling, retrieval-augmented generation (RAG), fine-tuning, and production readiness. The course focuses on understanding LLMs from a systems perspective and applying those concepts through hands-on projects and experiments. It also covers LLM evaluation, testing, and analysis.

## Repository Organization

* `week-01/` through `week-08/` — Weekly assignments, experiments, and notebooks.
* `notes/` — Notes from course readings and other useful information.
* `CLAUDE.md` — Instructions and context for AI coding assistants.
* `AGENTS.md` — Repository guidance for AI coding assistants and disclosure of AI assistance.
* `README.md` — Overview of the course and repository.

## Technologies

* **Python**
* **Jupyter Notebook / Google Colab**
* **GitHub**
* **tiktoken**
* **Gemini API**
* **OpenAI-compatible SDKs**

Additional tools and libraries will be added as the course progresses.


## Eight-Week Project Progression

- **Week 1 — Tokenization:** Compared English and Spanish tokenization using two encodings, measuring differences in token counts, cost, and context-window usage.
- **Week 2 — Inference and Sampling:** Traced a local model’s forward pass and explored how temperature, top-k, and top-p change next-token probabilities.
- **Week 3 — Prompt Engineering:** Compared versioned software-incident classification prompts using exact-match accuracy and semantic similarity, documenting improvements and regressions.
- **Week 4 — Tool Use:** Built a basketball assistant with player-stat lookup, player comparison, and a guarded calculator, including schema validation, multi-step tool calls, and failure recovery.
- **Week 5 — RAG Foundations:** Built a Spring documentation RAG pipeline and compared three chunking strategies using Precision@3 and Recall@3.
- **Week 6 — Advanced Retrieval:** Extended Week 5 with pseudo-relevance feedback and cross-encoder reranking. Mean Precision@4 increased from 0.825 to 0.850, with no additional improvement from reranking.
- **Week 7 — Adaptation Decisions:** Compared zero-shot and dynamic few-shot classification for home-care requests. Accuracy increased from 94% to 98% on 50 synthetic test requests, followed by an analysis of complexity, failures, and whether fine-tuning was justified.
- **Week 8 — Integrated Capstone:** Combined retrieval, grounded prompting, and adapted basketball tools into a Jordan–LeBron comparison system. Evaluated baseline and category-based retrieval using RAGAS and an LLM judge, then traced a retrieval failure and proposed mitigations.

The repository progresses from understanding model inputs and generation to building, evaluating, and integrating LLM application components.


# Caspstone

## Jordan vs. LeBron: Integrated RAG System

This project compares Michael Jordan and LeBron James across 5 categories:
defense, scoring, longevity, playoffs, and accolades using
retrieval, grounded prompting, and basketball lookup tools.

## Architecture

Ten source documents → overlapping chunks → Sentence Transformer
embeddings → FAISS retrieval → Gemini with optional tools → cited answer.

Baseline retrieval selects chunks by semantic similarity.
Category retrieval uses question keywords to allocate chunks
among labeled categories.

Tools provide  lookups for player stats, accolades,
and stat comparisons.

## Reused Course Techniques

- RAG: adapted from my earlier retrieval and chunking work.
- Tool use: adapted Week 4 schemas, validation, dispatch,
  and the tool-calling loop.
- Grounded prompting: instructions require evidence-based answers,
  citations, and acknowledgment of missing information.

## Running the Notebook

1. Set `GEMINI_API_KEY` and `GEMINI_MODEL` in a local `.env` file.
2. Run `week8.ipynb` from top to bottom.

The notebook installs its dependencies. Cached API responses
are stored in `llm_cache/`; RAGAS results are saved in
`ragas_scores.json`. Cache misses make live API requests.
Do not commit `.env`.

## Evaluation and Results

Ten questions compare baseline and category retrieval.
RAGAS evaluates answers without tools; a custom LLM judge
evaluates answers with tools.

| Metric | Baseline | Category |
|---|---:|---:|
| Faithfulness | 0.837 | 0.687 |
| Context recall | 0.423 | 0.253 |
| Judge accuracy | 4.8 | 4.7 |
| Judge citation support | 5.0 | 5.0 |
| Judge reasoning quality | 5.0 | 5.0 |

Baseline performed better on this test set. The LLM as a judge
scores were similar; tool assistance is one possible explanation.

## Failure and Proposed Mitigation

Category retrieval did not include a LeBron playoff chunk that
baseline retreival retrieved. The tools lacked playoff statistics and
could not fill the gap. The notebook includes answers, retrieved
chunks, and tool logs tracing this failure.

Proposed mitigations include broader retrieval fallback,
table-aware chunking, and adding verified playoff tool data.

## Research Note

[Research note](https://github.com/andrewaclark248/cosc-650-applied-llm-systems/issues/16)

## AI Assistance

See the AI-use disclosure in AGENTS.md.