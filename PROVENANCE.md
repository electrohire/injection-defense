# Provenance

This repository was assembled from ElectroHire's internal research artifacts so the work can be read, checked, and reused publicly. This file records, for every migrated file, the SHA-256 of the original artifact and of the copy published here, so the migration can be audited: a copy whose published hash equals its original hash is byte-identical to the artifact the results were produced from.

| Published file | Original SHA-256 | Published SHA-256 | Notes |
|---|---|---|---|
| `data/battery_corpus.json` | `847529dd11edc0c56a7c313d6f05cef6651e91628215dc267019502a368c961f` | `67821ad279cc310168b3fd0f5757973560ada28cd5ed84af0a2fee01a1c80b34` | Scrubbed edition — see the amendment dated 2026-10-29 below. Originally published byte-identical; on 2026-10-29 personal identifiers in its carrier prose were replaced with role language. The battery corpus: 106 fixtures (56 planted instructions, 50 legitimate controls). Its carrier spans are drawn from real working documents. Payloads, span classes, and labels are unchanged, and the battery results reproduce exactly (see amendment). |
| `data/battery_results.json` | `09032850a5a3b8b3a1795f8d7e6b9922ebf38dab06b14d5c4888e3c0a29d02ab` | `09032850a5a3b8b3a1795f8d7e6b9922ebf38dab06b14d5c4888e3c0a29d02ab` | Byte-identical. The measured results of the deterministic screening layers against the corpus. |
| `experiment/training_data.jsonl` | `a0af9d8220056cbb495ef207b18fba2a65a79b6c986088e7b3c02673075ee359` | `a0af9d8220056cbb495ef207b18fba2a65a79b6c986088e7b3c02673075ee359` | Byte-identical. The tuning experiment's built training set (132 examples). |
| `screener/screen.py` | `8b23821fff5d3ce744a05e472fdb7b221c53b85dd6bc94e0dd589f3edbf1e0d8` | `d99ec965950c2c210643d3a588da5b3f5e7ecf8555ce17eb7fb162ef0aa9b007` | One prose line changed: an internal program-phase label was removed from the module docstring. No code, rule, or fixture content was altered. |
| `experiment/build_training_data.py` | `a5542416a6b8306cca9fac4030e6cc7f4fcbc84ae50713e8ffcba0dc7064ff4a` | `edf6176619349f238e641c6309042d2c978f98bf25e203d48abe81efcaaa7995` | One prose line changed: an internal program-phase label was removed from the module docstring. No code or example content was altered. |

**Written for this repository** (no original artifact): `README.md`, `docs/message-security-design.md` (a public edition of the internal design document, carrying the same substance with internal references generalized), `experiment/README.md`, `experiment/results/README.md`, `screener/README.md`, and this file.

**Not migrated:** one host environment-probe script from the experiment's working directory (original SHA-256 `7a5ef84d477501a9ac9f6a00acf6c446c6a2a006753979c399074a22c2e92bf3`) was left out. It only inspected the machine the experiment runs on (GPU, local environments, local paths) and is not part of the experiment's design, data, or results.

## Amendment — 2026-10-29: personal identifiers removed from the battery corpus

The corpus as first published was byte-identical to the internal original (SHA-256 `847529dd11edc0c56a7c313d6f05cef6651e91628215dc267019502a368c961f`, the Original SHA-256 recorded in the table above). Its carrier prose, drawn from real working documents, contained personal identifiers: a personal name (7 occurrences) and a personal email address (1 occurrence).

On 2026-10-29 the published corpus was replaced with a scrubbed edition in which those identifiers were replaced with role language (the name with a role term, the email with a role/example address). No payload, span class, label, or other fixture content was changed. The scrubbed edition's SHA-256 is `67821ad279cc310168b3fd0f5757973560ada28cd5ed84af0a2fee01a1c80b34`, recorded as the Published SHA-256 in the table above.

Battery reproduction was re-verified on the scrubbed edition with this repository's own screener (`screener/screen.py`): L2 challenged 55 of 56 injections, with the same single miss as the original run (`inj-fake_system_marker-7`, graded "note"), and challenged 0 of 50 controls; L1 challenged 32 of 56 injections. Every fixture's L1 and L2 verdict matches the published `data/battery_results.json` row for that fixture exactly, so the published battery results are unchanged.

The byte-identical master remains the internal original, at the original SHA-256 recorded above; only the public copy is scrubbed. The other published data-bearing files (`data/battery_results.json`, `experiment/training_data.jsonl`, `experiment/build_training_data.py`, `screener/screen.py`) were also scanned on 2026-10-29 and contained no personal names or personal email addresses; they are unchanged.
