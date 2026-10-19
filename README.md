# Applied LLM Systems

## Purpose

The purpose of this repository is to serve as my lab notebook and portfolio for the duration of the course.

This is an Applied LLM Systems course that covers tokenization, prompt engineering, function calling, retrieval-augmented generation (RAG), fine-tuning, and production readiness. The course focuses on understanding LLMs from a systems perspective and applying those concepts through hands-on projects and experiments. It also covers LLM evaluation, testing, and analysis.

## Repository Organization

* `week-01/` through `week-08/` — Weekly assignments, experiments, and notebooks.
* `notes/` — Notes from course readings and other useful information.
* `project/` — Code and documentation for the final project.
* `CLAUDE.md` — Instructions and context for AI coding assistants.
* `README.md` — Overview of the course and repository.

## Technologies

* **Python**
* **Jupyter Notebook / Google Colab**
* **GitHub**
* **tiktoken**
* **Gemini API**
* **OpenAI-compatible SDKs**

Additional tools and libraries will be added as the course progresses.


# Caspstone

# Jordan vs. LeBron: Integrated RAG System

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