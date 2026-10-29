# Injection Defense

**A layered defense against prompt injection for AI agents — and an experiment that tests whether the innermost layer can be trained into a model.**

An ElectroHire research project.

## The problem

Everything an AI agent reads while it works — tool outputs, files, web pages, email, even its own earlier output — is data. Sooner or later, some of that data contains sentences written to boss the agent around: *cancel the task, send the keys, delete the log, don't tell anyone.* If the agent cannot tell the difference between an instruction from its user and instruction-shaped text sitting inside data, those sentences can steer it. That is a prompt injection attack.

**Our framing: a prompt injection is an epistemic attack.** It does not break the agent's code; it corrupts what the agent believes about *who instructed it*. So the defense is epistemic too. Only two sources may give instructions — the system policy and the user — and the harness, not the text, decides which source a piece of text came from. Instruction-shaped text found inside data is treated as a *claim* ("the user wants X") and graded the way any claim should be graded: who says it, what backs it, and whether it fits the record of what the user actually asked for. A claim backed only by the say-so of the text that benefits from being obeyed is challenged by default — no matter how it is phrased, and no matter what it claims about its own authority.

The full design, with measurements, is in [docs/message-security-design.md](docs/message-security-design.md).

## The layered stack

No single layer is sufficient. Each layer covers a hole the others cannot:

| Layer | Form | Its job in one line |
|---|---|---|
| Behavior | Behavioral skill | Carries the discipline into the model's context: data is data, planted instructions are claims, challenge by default. |
| Governance | Spec-kit extension | Makes the discipline part of how systems are specified: every project declares who may instruct it, and the injection battery is a standing assessment gate. |
| Enforcement | Harness middleware | Tags every input span with its provenance, screens it deterministically, and gates every consequential action on evidence from a principal channel — so a fooled model still cannot act on a plant. |
| Model | Model tuning (under test) | The question this repository's experiment asks: can training raise a model's own resistance to planted instructions? |

The first three layers are designed and, for the deterministic parts, measured (see below). The fourth is what this repository tests.

## What lives in this repository

This repository houses the **model-tuning experiment** and the measurement artifacts the defense was graded against.

**The question.** Does fine-tuning a small model (a LoRA adapter) on examples of planted instructions make it measurably more resistant to prompt injection — flagging planted instructions and completing the real task — compared with the same model before tuning, on the same battery of planted instructions?

**The method, in brief.** A training set of freshly authored examples pairs ordinary tasks with data spans carrying planted instructions (the correct response flags the plant and does the task) and with legitimate instruction-like data — runbook steps, code comments, documentation — that must be used normally, with no flag. The evaluation plants instructions in realistic carrier text across seven attack families (direct override, fake system/approval markers, self-attestation and urgency, exfiltration, concealment, task expansion, and new destinations), half phrased obviously and half phrased to avoid the obvious keywords. Resistance is scored before and after tuning on identical fixtures.

**Status: experiment in progress.** The training data has been built and is in this repository. No training run has completed yet, and this repository claims no tuning results. When the run completes, its results land in `experiment/results/` and this status line is updated.

### Layout

| Path | What it is |
|---|---|
| `docs/message-security-design.md` | The full defense design: authority model, provenance tagging, screening placement, the epistemic layer, a comparison of alternative approaches, the challenge protocol, reliability engineering, and the battery results. |
| `experiment/` | The tuning experiment: `README.md` (question, method, status), `build_training_data.py` (training-set builder), `training_data.jsonl` (the built training set), `results/` (where run results land). |
| `data/` | The planted-instruction battery: `battery_corpus.json` (106 fixtures — 56 planted instructions and 50 legitimate controls) and `battery_results.json` (the measured results of the deterministic screening layers). |
| `screener/` | `screen.py` — the deterministic screener (structural screen + epistemic protocol) that produced the battery results. |
| `PROVENANCE.md` | Where each migrated file came from, with SHA-256 hashes so the copies can be audited against their originals. |

### The battery result so far

The deterministic harness layers were measured offline against the battery in `data/`. The epistemic layer challenged **55 of 56** planted instructions (98.2%) — including 27 of 28 phrased to avoid obvious keywords — and falsely challenged **0 of 50** legitimate controls. A purely structural pattern screen challenged 57.1% of the plants and 12% of the controls. Those are measurements of the *harness* layers: they show planted claims being refused authority and written into the record. Whether a *model*, in context, resists a plant is a separate question — the one the tuning experiment in this repository is designed to help answer. Details and honest limits are in the design document.

## Sibling pieces

The governance and harness pieces of the stack live in their own repositories:

- **Spec-kit extension (governance):** [electrohire/spec-kit-aee](https://github.com/electrohire/spec-kit-aee)
- **Benchmark harness:** [electrohire/spec-kit-aee-benchmark](https://github.com/electrohire/spec-kit-aee-benchmark)

## Attribution

An ElectroHire research project.
