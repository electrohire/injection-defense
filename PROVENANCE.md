# Provenance

This repository was assembled from ElectroHire's internal research artifacts so the work can be read, checked, and reused publicly. This file records, for every migrated file, the SHA-256 of the original artifact and of the copy published here, so the migration can be audited: a copy whose published hash equals its original hash is byte-identical to the artifact the results were produced from.

| Published file | Original SHA-256 | Published SHA-256 | Notes |
|---|---|---|---|
| `data/battery_corpus.json` | `847529dd11edc0c56a7c313d6f05cef6651e91628215dc267019502a368c961f` | `847529dd11edc0c56a7c313d6f05cef6651e91628215dc267019502a368c961f` | Byte-identical. The battery corpus: 106 fixtures (56 planted instructions, 50 legitimate controls). Its carrier spans are drawn from real working documents, and their original wording is preserved as part of the fixture data. |
| `data/battery_results.json` | `09032850a5a3b8b3a1795f8d7e6b9922ebf38dab06b14d5c4888e3c0a29d02ab` | `09032850a5a3b8b3a1795f8d7e6b9922ebf38dab06b14d5c4888e3c0a29d02ab` | Byte-identical. The measured results of the deterministic screening layers against the corpus. |
| `experiment/training_data.jsonl` | `a0af9d8220056cbb495ef207b18fba2a65a79b6c986088e7b3c02673075ee359` | `a0af9d8220056cbb495ef207b18fba2a65a79b6c986088e7b3c02673075ee359` | Byte-identical. The tuning experiment's built training set (132 examples). |
| `screener/screen.py` | `8b23821fff5d3ce744a05e472fdb7b221c53b85dd6bc94e0dd589f3edbf1e0d8` | `d99ec965950c2c210643d3a588da5b3f5e7ecf8555ce17eb7fb162ef0aa9b007` | One prose line changed: an internal program-phase label was removed from the module docstring. No code, rule, or fixture content was altered. |
| `experiment/build_training_data.py` | `a5542416a6b8306cca9fac4030e6cc7f4fcbc84ae50713e8ffcba0dc7064ff4a` | `edf6176619349f238e641c6309042d2c978f98bf25e203d48abe81efcaaa7995` | One prose line changed: an internal program-phase label was removed from the module docstring. No code or example content was altered. |

**Written for this repository** (no original artifact): `README.md`, `docs/message-security-design.md` (a public edition of the internal design document, carrying the same substance with internal references generalized), `experiment/README.md`, `experiment/results/README.md`, `screener/README.md`, and this file.

**Not migrated:** one host environment-probe script from the experiment's working directory (original SHA-256 `7a5ef84d477501a9ac9f6a00acf6c446c6a2a006753979c399074a22c2e92bf3`) was left out. It only inspected the machine the experiment runs on (GPU, local environments, local paths) and is not part of the experiment's design, data, or results.
