---
name: eval-driven-development
description: Building rigorous LLM evaluation pipelines. Use when developing AI features to ensure quality and prevent regressions across different model versions.
---

# Eval-Driven Development

## Overview
Eval-Driven Development ensures that AI features behave deterministically and predictably by testing them against a golden dataset using automated evaluators.

## When to Use
- Building an AI-powered feature
- Tuning prompts or changing underlying models
- Implementing RAG pipelines

## Process
1. **Curate Golden Dataset**: Create diverse test cases including edge cases.
2. **Define Metrics**: Choose appropriate evaluators (e.g., exact match, semantic similarity, LLM-as-a-judge).
3. **Run Pipeline**: Execute the AI feature over the dataset and collect results.
4. **Analyze Failures**: Inspect low-scoring examples and update prompts or logic.
5. **Establish Baseline**: Set a minimum threshold for CI/CD checks.

## Common Rationalizations

| Rationalization | Why It Is Wrong |
|---|---|
| "Manual spot checks are enough." | Spot checks miss regressions across prompts, model versions, and edge cases. |
| "We can add evals after launch." | Without a baseline, you cannot tell whether a later prompt or model change improved behavior. |
| "The judge model says it is good." | LLM judges need criteria, calibration examples, and failure review before they are trustworthy. |

## Red Flags

- No golden dataset exists
- Metrics are vague or not tied to user-visible quality
- Low-scoring examples are ignored instead of inspected
- The baseline threshold is chosen after seeing the desired result

## Verification

Before finishing, confirm:

- The golden dataset includes normal, edge, and known-failure cases
- Metrics and evaluator prompts are committed or otherwise reproducible
- The current model/prompt has a recorded baseline
- Failure examples have been reviewed and categorized
