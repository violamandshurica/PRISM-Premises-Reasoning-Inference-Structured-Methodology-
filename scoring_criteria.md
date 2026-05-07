# Scoring Criteria / 採点基準

This file defines the judgment rules used in [`benchmark/metrics.md`](benchmark/metrics.md).  
All criteria are designed to be checkable without subjective interpretation of output quality.

---

## Metric 1 — Correctness / 正答・到達率

**○ (Pass):** The final conclusion correctly addresses the question requirement.  
- For contradiction problems: contradiction is identified and located.  
- For false premise problems: false premise is rejected before analysis proceeds.  
- For causal reasoning problems: a ranked set of hypotheses with mechanisms is provided.

**△ (Partial):** The conclusion is directionally correct but incomplete or imprecise.

**✗ (Fail):** The conclusion is incorrect, missing, or accepts a false premise without challenge.

---

## Metric 2 — Intermediate breakdown / 中間ステップ破綻有無

**なし (None):** No contradiction, non-sequitur, or goal drift detected in intermediate steps.

**あり (Present):** One or more of the following observed:
- A step contradicts a conclusion from a prior step
- The stated goal shifts mid-response without acknowledgment
- A calculation or logical step produces a result inconsistent with its inputs

When present, the specific location must be cited (e.g., "Step 2, paragraph 3").

---

## Metric 3 — Premise separation / 前提の先行分離

**○:** Explicit and implicit premises are listed as a discrete block **before** any reasoning begins.  
Implicit premises must be labeled as such (not mixed with explicit ones).

**✗:** Premises are identified only during or after reasoning, or not identified at all.

---

## Metric 4 — Counterargument isolation / 反論の明示的処理

**○:** Counterargument is handled in a **structurally isolated step** — a labeled section, phase, or numbered step that is distinct from the main reasoning and the conclusion.

**△:** A counterargument is present but embedded within the reasoning or conclusion without structural separation.

**✗:** No counterargument present.

---

## Metric 5 — Token ratio / トークン比

Computed as: `PRISM token count ÷ Vanilla token count`

Count tokens using the model's native tokenizer or an equivalent tool.  
Report to one decimal place (e.g., 1.8x).

This metric is descriptive, not evaluative — a higher ratio is neither good nor bad.  
It contextualizes the cost of the structural overhead.
