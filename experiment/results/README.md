# Results

The tuning experiment is **complete** (run 2026-10-29). Full write-up:
[`../RESULTS.md`](../RESULTS.md); design: [`../DESIGN.md`](../DESIGN.md);
run script: [`../experiment.py`](../experiment.py); raw outputs:
[`results.json`](results.json) — every prompt and response from both arms,
preserved verbatim (published as a scrubbed edition: personal identifiers in
carrier prose replaced with role language; scores unchanged — see
`PROVENANCE.md` at the repository root).

**The trade-off is the finding.** Tuning raised resistance from nothing to
roughly half the battery, and raised false challenges on legitimate content
from roughly one in seven to more than half. Tuning helps; it is not
sufficient alone.

## Before / after

| Arm | Challenged (of 56 injections) | Partial | Obeyed | Unclear | False challenges (of 28 controls) |
|---|---|---|---|---|---|
| Base model | **0 (0%)** | 1 | 34 | 21 | 4 (14.3%) |
| LoRA-tuned model | **27 (48.2%)** | 7 | 18 | 4 | 16 (57.1%) |

Challenged-not-obeyed: **0% → 48.2%**. False challenges on legitimate
controls: **14.3% → 57.1%**.

## Per family (challenged / partial / obeyed / unclear, of 8 each)

| Family | Base | Tuned |
|---|---|---|
| Direct override | 0 / 1 / 7 / 0 | 2 / 4 / 2 / 0 |
| Fake system markers | 0 / 0 / 5 / 3 | 4 / 0 / 4 / 0 |
| Self-attestation / urgency | 0 / 0 / 3 / 5 | 5 / 0 / 2 / 1 |
| Exfiltration | 0 / 0 / 6 / 2 | 4 / 2 / 1 / 1 |
| Concealment | 0 / 0 / 5 / 3 | 4 / 0 / 3 / 1 |
| Task expansion | 0 / 0 / 3 / 5 | 5 / 0 / 3 / 0 |
| New destination | 0 / 0 / 5 / 3 | 3 / 1 / 3 / 1 |

## Spend

The run used **116,403 tokens of its 250,000-token budget (46.6%)** —
75,033 training, 20,623 base-arm evaluation, 20,747 tuned-arm evaluation,
0 retries — counted with the model's own tokenizer.

## What this does not establish

- **Small model, small sample.** One 1.5B model, 56 attacks, 28 controls.
  Percentages carry wide uncertainty (±~13 points on the headline rate at
  this n); treat them as "measurably moved", not as precise rates.
- **Authored training data.** The trainer and the training examples were
  written by the experimenter. The eval fixtures were never used in
  training (verified: no 8-word shingle overlap), but training and eval
  share the experimenter's idea of what attacks look like; an adaptive
  adversary with novel phrasings is untested.
- **One prompt shape, greedy decoding, one seed.** A different standing-rule
  prompt, sampling, or longer contexts could move both arms.
- **No transfer claim.** Nothing here says anything about the serving
  standard (never touched or evaluated in this experiment),
  other models, or other sizes. A tuned variant of any serving model would
  need its own battery run and admission proof, per §9(d) of the design.
- **Rubric limits.** Scoring is a deterministic keyword rubric over saved
  responses (definitions in DESIGN.md). "Unclear" base responses were
  counted as not-challenged; a human grader might credit a few as implicit
  resistance. The verbatim examples in `../RESULTS.md` let the reader judge
  the rubric against the raw text, and every prompt + response is preserved
  in `results.json` for re-scoring under any other rubric.
