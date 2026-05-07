# PRISM — Premises Reasoning Inference Structured Methodology

**v2.5 · Single-prompt · Single-LLM · Single-pass**

> A reasoning scaffold that mimics the structure of human critical thinking within the constraints of a forward-pass language model.

---

## What is PRISM?

PRISM is a system prompt designed for single-LLM, single-pass operation. It imposes a five-phase structure that mirrors the human critical thinking process:

```
Phase 1 — Premises        : Separate explicit and implicit assumptions. Surface conflicts.
Phase 2 — Core Question   : Restate the real problem. Abstract one level up.
Phase 3 — Decompose       : Break into steps. Reason via Cause → Mechanism → Outcome.
Phase 4 — Counterarguments: Steelman the opposition. Check consistency with Phase 1.
Phase 5 — Conclusion      : State the final answer. Declare uncertainty if it exists.
```

The goal is not to increase raw accuracy, but to **control failure modes** — error propagation, uncritical premise acceptance, and structural drift — within the hard constraints of a single forward pass.

---

## Design Principles

**Constraint-aware design.**
Single-pass LLMs tend to silently drop instructions as output length grows. PRISM is calibrated for the balance between phase count, instruction density, and total token volume. Adding more phases would increase structural integrity on paper while reducing it in practice.

**Process transparency over outcome.**
PRISM makes the reasoning chain auditable. The difference between a PRISM output and a vanilla output is not always the final answer — it is whether the path to that answer can be inspected, challenged, and reproduced.

**Forced counterargument.**
Phase 4 is non-optional. A response that skips counterargument review is structurally incomplete, not merely less thorough. This is the primary mechanism by which confirmation bias is surfaced.

---

## Benchmark

Three test questions were run on DeepSeek Instant, comparing vanilla vs. PRISM outputs.

| Question | Type | Vanilla correct | PRISM correct |
|---|---|---|---|
| A | Multi-step logic / contradiction detection | ✓ | ✓ |
| B | False premise detection | ✓ | ✓ |
| C | Multi-hypothesis causal reasoning | ✓ | ✓ |

Final answer accuracy was equivalent. The measurable difference was in **process quality**:

| Metric | Vanilla | PRISM |
|---|---|---|
| Premises separated before reasoning | ✗ | ✓ |
| Counterargument in isolated step | ✗ (embedded in conclusion) | ✓ |
| Uncertainty explicitly stated | △ | ✓ |

→ Full results: [`benchmark/metrics.md`](benchmark/metrics.md)  
→ Questions (full text): [`benchmark/questions.md`](benchmark/questions.md)  
→ Scoring criteria: [`scoring_criteria.md`](scoring_criteria.md)

---

## Limitations (read before use)

PRISM v2.5 is an attempt to mitigate the fundamental constraints of LLMs — next-token prediction, absence of causal models, absence of metacognition — through prompt structure.

It shows measurable improvement in symbolic contradiction detection and goal tracking across reasoning chains. However, it **cannot** provide:

- True reasoning or causal manipulation
- Probabilistically calibrated confidence
- Reliable performance on chains of 5+ inferential steps
- Correct counterfactual causal judgments

Error propagation becomes significant in chains of 5 or more steps. Use outputs accordingly.

---

## Usage

Copy the full prompt from [`PRISM_v2.5.md`](PRISM_v2.5.md) into your system prompt.

PRISM is model-agnostic. It has been tested on DeepSeek Instant. Performance varies by model capability — stronger base models benefit more from structural scaffolding.

---

## Language note

This README is in English. A Japanese version is available at [`README_ja.md`](README_ja.md).
