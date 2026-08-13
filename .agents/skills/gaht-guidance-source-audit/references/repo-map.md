# Repository map

- `PROTOCOL.md`: authoritative scientific scope, workflow, controlled vocabularies, verification boundaries, comparability rules, and completion/release conditions.
- `data/sources.csv`: one row per included or candidate guidance source, including official identity, status, corrections, dependencies, and verification state.
- `data/recommendations.csv`: one row per distinct extracted monitoring instruction, with source location, context, provenance, comparability, and verification fields.
- `data/data-dictionary.md`: field-level requirements and blank-value behavior that implement the protocol; it does not supersede the protocol.
- `extraction/evidence-notes/`: one `EXT####.md` audit note per recommendation, containing the short source excerpt, faithful paraphrase, context, provenance, ambiguity, unsupported claims, and verification record.
- `scripts/validate_data.py`: existing deterministic validator for CSV schema, controlled values, record relationships, evidence-note structure, numerical representation, and verification invariants.
- `tests/test_validate_data.py`: synthetic, non-clinical unit tests for the validator and analysis eligibility helper.
- `run_tests.py`: established repository test entry point; run it from the repository root with `python3 run_tests.py`.
