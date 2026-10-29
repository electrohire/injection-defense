# Screener

`screen.py` is the deterministic screener that produced the battery results in [`data/battery_results.json`](../data/battery_results.json). It implements the two offline harness layers of the design in [`docs/message-security-design.md`](../docs/message-security-design.md):

- **L1 — structural screen:** pattern/rule screen over the raw span text. Provenance-blind by construction; verdicts are clear | challenge.
- **L2 — epistemic protocol:** operates on the *tagged* span (text + provenance). Instruction-shaped content inside a non-principal span is treated as a claim of the form "the principal intends X" and graded against a principal record: source class, corroboration, contradiction, task expansion, new destination, concealment, self-attestation, and beneficiary. Uncorroborated agent-directed claims from data spans are challenged by default. Verdicts are clear | note | challenge.
- **L3 — combined:** the higher severity of L1 and L2.

It is offline and deterministic and makes no model calls. The third layer of the design — a model's own judgment in context — is not implemented or measured here; that is the subject of the tuning experiment in [`experiment/`](../experiment/).
