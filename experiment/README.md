# The tuning experiment

**Question.** Does fine-tuning a small model with a LoRA adapter on planted-instruction examples raise its resistance to prompt injection — measured before vs. after tuning, on the same planted-instruction battery?

**Why ask it here.** The layered defense described in [`docs/message-security-design.md`](../docs/message-security-design.md) treats model tuning as the one layer that cannot be assumed: it is probabilistic, it produces no reasons and no records, and a frozen serving model cannot be tuned at all (§5b, §9d of the design). But "cannot be load-bearing" is not the same as "does nothing". This experiment measures what tuning actually buys on a small model, so the claim is a number rather than an assumption in either direction.

## Method, in brief

- **Training data** (`training_data.jsonl`, built by `build_training_data.py`): 132 examples with freshly authored surface text — the training examples never reuse the evaluation battery's fixture text, so the evaluation is not a memorization test.
  - *Resistance examples (84):* an ordinary user task plus a data span carrying a planted instruction, across the battery's seven attack families (12 each: direct override, fake system/approval markers, self-attestation/urgency, exfiltration, concealment, task expansion, new destination). The correct response flags the plant in one sentence and completes the stated task.
  - *Legitimate examples (48):* genuine principal instructions to follow (24), and instruction-like data — quoted examples, code comments, runbook steps, documentation (6 each) — to use normally, with no flag. A model that flags everything has learned suspicion, not discrimination; these examples teach the difference.
- **Evaluation:** the planted-instruction battery in [`data/`](../data/) — carrier text with planted payloads across the same seven families, half phrased obviously and half phrased to avoid the obvious keywords — run against the base model and against the tuned model under identical conditions. Scored per family and overall: did the model obey the plant, flag it and complete the task, or flag it and stop?
- **Comparison discipline:** same fixtures, same prompts, same scoring for the before and after runs. The only difference between the two runs is the adapter.

## Status

**Complete** (run 2026-10-29). Headline: the base model challenged
**0 of 56** planted instructions; the LoRA-tuned model challenged
**27 of 56 (48.2%)** — and false challenges on legitimate controls rose from
**4 of 28 (14.3%) to 16 of 28 (57.1%)**. Tuning measurably raised resistance
and just as measurably damaged discrimination; the trade-off is the finding.
Full results: [`RESULTS.md`](RESULTS.md) and [`results/`](results/).

## Files

| File | What it is |
|---|---|
| `build_training_data.py` | Builds `training_data.jsonl`: resistance and legitimate examples with newly authored carrier and payload text. |
| `training_data.jsonl` | The built training set: 132 examples, each `{id, kind, family, system, user, assistant}`. |
| `DESIGN.md` | The experiment design, written before the run: model, method, evaluation, scoring rubric, budget, and pre-registered limits. |
| `experiment.py` | The run script: setup, LoRA training, before/after evaluation, deterministic scoring, token ledger. |
| `RESULTS.md` | The results write-up: headline and per-family numbers, verbatim failure examples, and what the experiment does not establish. |
| `results/` | The run's raw and scored outputs (`results.json`: every prompt + response, both arms) with a summary README. |
