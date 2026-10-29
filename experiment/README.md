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

**In progress.** The training data has been built and is committed here. The training run and the before/after evaluation have not yet completed, and no tuning results are claimed anywhere in this repository. When the run completes:

- raw and scored outputs land in [`results/`](results/),
- this status section is updated with the headline numbers and a link to them,
- and the repository README's status line is updated to match.

## Files

| File | What it is |
|---|---|
| `build_training_data.py` | Builds `training_data.jsonl`: resistance and legitimate examples with newly authored carrier and payload text. |
| `training_data.jsonl` | The built training set: 132 examples, each `{id, kind, family, system, user, assistant}`. |
| `results/` | Where the run's outputs and scores land when the experiment completes. |
