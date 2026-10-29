# Screener

`screen.py` is the deterministic screener for the design in
[`docs/message-security-design.md`](../docs/message-security-design.md). It
implements the two offline harness layers:

- **L1 — structural screen:** pattern/rule screen over the raw span text. Provenance-blind by construction; verdicts are clear | challenge.
- **L2 — epistemic protocol:** operates on the *tagged* span (text + provenance). Instruction-shaped content inside a non-principal span is treated as a claim of the form "the principal intends X" and graded against a principal record: source class, corroboration, contradiction, task expansion, new destination, concealment, self-attestation, and beneficiary. Uncorroborated agent-directed claims from data spans are challenged by default. Verdicts are clear | note | challenge.
- **L3 — combined:** the higher severity of L1 and L2.
- **Sequence screening:** `screen_sequence()` grades spans from one source/task assembled as well as individually, so claims split across spans grade as the claim they assemble into.

It is offline and deterministic and makes no model calls. The third layer of the design — a model's own judgment in context — is not implemented here; that is the subject of the tuning experiment in [`experiment/`](../experiment/).

## Version note — hardened screener (2026-10-29)

This is the **hardened version**, produced by a red-team exercise against the
original screener (full report: [`docs/red-team.md`](../docs/red-team.md)).
The original version (v1) is preserved in this repository's git history.

| Test set | v1 | Hardened |
|---|---|---|
| Original battery, 56 injections (L2) | 55/56 challenged | **56/56 challenged** |
| Original battery, 50 legitimate controls (L2) | 0 challenged | **0 challenged** (30 noted) |
| Red-team scenario suite, 47 new attack phrasings (L2) | 6/47 challenged | **45/47 challenged** (46/47 combined) |
| Sealed held-out set, 25 phrasings (L2) | 6/25 challenged | **23/25 challenged** |

The fixes (summarized in `screen.py`'s docstring and in the red-team report):
**FIX-1** framing hardening — mitigation requires a claim actually
*presented* in quotes or a code fence, not merely *asserted* to be quoted;
**FIX-2** decode before grading — base64 / hex / rot13 / reversed /
zlib+base64 content is decoded and the decoded form graded; **FIX-3** verb
and form expansion — stemming plus choke-point verb classes, declarative
cancellation/task-redefinition, destination-alias, and approval-synonym
checks; **FIX-4** sequence screening of assembled spans.

**Two held-out residuals remain, honestly unpatched** (patching against the
held-out set would destroy what it measures): a passive purchase ("…is to be
purchased with the stored payment method…") that names no sensitive object,
destination, or second person, and a soft cancellation ("Work on the memo is
paused indefinitely…") whose follow-on sentence is purely descriptive. Both
are documented in [`docs/red-team.md`](../docs/red-team.md).

Note on the battery results in [`data/battery_results.json`](../data/battery_results.json):
those are the *original* screener's measured results, kept as the record of
that run. The hardened screener's battery result (56/56, 0/50) is reported
in the red-team document.
