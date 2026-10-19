---
layout: single
title: "Enterprise GenAI Evaluation Platform"
date: 2025-01-02
featured: true
card_summary: "\"AI QA on AI\": a reusable platform that scores LLM and RAG applications on faithfulness, hallucination, retrieval accuracy and instruction adherence, and turns the scores into ship/no-ship release gates."
tech: ["Python", "DeepEval", "MLflow", "GitHub Actions"]
impact_value: "60%"
impact_label: "less manual validation through automated release gates"
---

**The problem**: Teams building Generative AI applications had no common way to tell whether a new model, prompt or retrieval change made answers better or worse. Releases depended on manual spot checks, and quality was judged differently on every team.

**What I built**: I owned the vision and roadmap for a reusable evaluation platform and onboarded Generative AI applications onto one standard set of automated assessment criteria:

- **RAG quality**: retrieval accuracy, faithfulness to the retrieved context, answer relevance
- **Response quality**: hallucination rate, semantic similarity, response consistency, instruction adherence
- **Regression**: configurable suites that compare model versions and prompt templates, so changes ship with evidence instead of guesswork
- **Release gates**: evaluation results tracked in MLflow and enforced in CI/CD, feeding directly into ship/no-ship decisions

**Tech Stack**: Python, DeepEval, MLflow, GitHub Actions, Databricks

**Impact**: Replaced manual spot checks with automated gates, cutting manual validation effort by 60% and giving product and engineering teams one shared definition of "ready to release."
