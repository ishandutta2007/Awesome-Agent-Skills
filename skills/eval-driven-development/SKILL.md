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
- "A few manual examples are enough." Manual checks miss regressions and cannot
  protect future model, prompt, or retrieval changes.
- "The model output is subjective." Subjective output still needs explicit
  quality dimensions, examples, and threshold decisions.
- "We can add evals after launch." Without a baseline, every later change is
  harder to compare and easier to rationalize.

## Red Flags
- No golden dataset exists before prompt or retrieval changes ship.
- The metric rewards exact formatting while ignoring task success.
- Failing examples are deleted instead of analyzed and preserved.
- CI does not run the evaluation or report a threshold.

## Verification
- Golden examples cover happy paths, edge cases, and known failures.
- Metrics are documented and aligned with user-visible quality.
- Baseline scores are recorded before the implementation changes.
- The eval command runs locally or in CI and blocks unacceptable regressions.
