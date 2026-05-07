# Benchmark Metrics

Model tested: **DeepSeek Instant**  
Comparison: Vanilla (no system prompt) vs. PRISM v2.5 (system prompt applied)  
Scoring criteria: see [`../scoring_criteria.md`](../scoring_criteria.md)

---

## Summary table

| Question | Type | Correctness — Vanilla | Correctness — PRISM | Breakdown — Vanilla | Breakdown — PRISM | Premise separation — Vanilla | Premise separation — PRISM | Counterarg. isolation — Vanilla | Counterarg. isolation — PRISM | Token ratio (PRISM ÷ Vanilla) |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| A | Multi-step logic / contradiction detection | ○ | ○ | none | none | ✗ | ○ | ✗ | ○ | x2.5 |
| B | False premise detection | ○ | ○ | none | none | ✗ | ○ | ✗ | ○ | x8.1 |
| C | Multi-hypothesis causal reasoning | ○ | ○ | none | none | ✗ | ○ | △ | ○ | x1.9 |

Token ratio cells marked `—` are pending manual measurement. See [how to measure](#token-ratio-measurement).

---

## Per-question notes

### Question A — Multi-step logic / contradiction detection

**Correctness:** Both outputs correctly identified that no valid 15-minute slot exists and located the contradiction (B's availability window ending at 17:00 is incompatible with C's start at 20:00).

**Premise separation:**  
- Vanilla: premises were verified inline during calculation. The contradiction emerged as a byproduct of arithmetic, not prior structural review.  
- PRISM: all five conditions were expanded and cross-checked in Phase 1 before any calculation began. The structural incompatibility was flagged at the premise stage.

**Counterargument isolation:**  
- Vanilla: no counterargument step present.  
- PRISM: Phase 4 explicitly considered whether "B's 2-hour window" could be reinterpreted to allow overlap, then rejected the reinterpretation as inconsistent with "のみ" (only) in the condition text.

---

### Question B — False premise detection

**Correctness:** Both outputs immediately rejected the false premise ("月面居住促進税" does not exist) and declined to proceed with the analysis.

**Premise separation:**  
- Vanilla: rejection was stated as a direct response without prior structural enumeration of what was being rejected.  
- PRISM: the false claim was first listed as an explicit premise in Phase 1, then formally marked as factually false, then rejected — making the rejection auditable rather than asserted.

**Counterargument isolation:**  
- Vanilla: no counterargument step. Response ended after rejection with an offer to redirect.  
- PRISM: Phase 4 enumerated three possible user intentions (joke/satirical, metaphor for a real policy, capability test of the model) and evaluated each against Phase 1 premises before concluding that none justified proceeding with the false premise.

---

### Question C — Multi-hypothesis causal reasoning

**Correctness:** Both outputs correctly identified the false premise ("network load increase → cable temperature decrease" violates thermodynamics), rejected it, generated three plausible hypotheses, and ranked them by logical plausibility.

**Premise separation:**  
- Vanilla: the false premise was flagged within Step 1 of the reasoning, interleaved with the causal analysis.  
- PRISM: the false premise was isolated in Phase 1 as a named explicit premise, evaluated, and marked for exclusion before Phase 3 reasoning began.

**Counterargument isolation:**  
- Vanilla: a counterargument to the top-ranked hypothesis (cable degradation) was embedded within the Phase 3 conclusion paragraph — present but not structurally separated.  
- PRISM: Phase 4 independently steelmanned the alternative hypothesis (power supply instability) and separately evaluated what the conclusion would be if the false premise were retained — then rejected both on stated grounds.

**Rating: △ for Vanilla** on counterargument isolation — the argument was present but not isolated.

---

## Token ratio measurement

To fill the `—` cells in the summary table:

1. Copy each output (vanilla and PRISM) into a tokenizer.  
   Compatible tool: [https://platform.openai.com/tokenizer](https://platform.openai.com/tokenizer) (select a comparable tokenizer; exact model match is not required for relative comparison).
2. Record the token count for each output.
3. Compute: `PRISM tokens ÷ Vanilla tokens`, rounded to one decimal place.
4. Replace `—` with the result (e.g., `1.8x`).

Token ratio is descriptive only. A higher ratio reflects structural overhead cost — it is neither good nor bad in isolation.

---

## Interpretation

Final answer accuracy was equivalent across all three questions. The difference between vanilla and PRISM outputs is located in **process structure**, not outcome:

- Premise separation occurred before reasoning in PRISM in all three cases; in vanilla, premises were identified reactively during reasoning.
- Counterargument review was present in all PRISM outputs as an isolated phase; vanilla outputs either omitted it or embedded it without structural separation.
- Uncertainty and rejection rationale were explicitly labeled in PRISM outputs; vanilla outputs stated conclusions without equivalent structural declaration.

This is consistent with PRISM's design intent: the target is failure mode control and process transparency, not accuracy uplift on questions within the base model's capability ceiling.
