---
name: gaht-guidance-source-audit
description: Apply this repository's existing protocol to GAHT source verification, guidance-document and recommendation extraction, evidence-note creation, provenance or correction review, ambiguity review, candidate comparison grouping, comparability review, completion audits, and validation of GAHT research records. Use only for the predefined documentary source audit; do not provide clinical advice or expand the evidence base.
---

# GAHT Guidance Source Audit

## Authority and scope

Treat `PROTOCOL.md` as the authoritative scientific and methodological source.

> If this skill conflicts with `PROTOCOL.md`, `PROTOCOL.md` wins.

Use this skill to execute the existing protocol consistently, not to replace, amend, or broaden it. Keep all work documentary and source-bounded. Do not offer clinical advice.

Before acting:

1. Inspect the current worktree and preserve unrelated changes.
2. Read the current `PROTOCOL.md`; do not rely on a cached copy or this skill alone.
3. Inspect the relevant `data/sources.csv` row, related `data/recommendations.csv` rows, evidence notes, and any task-specific artifacts.
4. Read [references/workflow.md](references/workflow.md) for source or record work.
5. Read [references/verification-boundaries.md](references/verification-boundaries.md) before drafting or changing research records.
6. Read [references/common-failure-modes.md](references/common-failure-modes.md) for ambiguous, corrected, adapted, or potentially comparable passages.
7. Use [references/repo-map.md](references/repo-map.md) when orienting or selecting validation commands.

## Operational sequence

1. Establish the requested scope and whether research-data changes are explicitly authorized. If not, remain read-only with respect to `SRC`, `REC`, and `EXT` records.
2. Inspect the source row and existing records before resolving the authoritative document, version, official location, access date, and current status.
3. Check documented corrections, corrigenda, updates, and supersession when the protocol or task requires it.
4. Locate potentially in-scope serum estradiol or testosterone monitoring passages in the authoritative source.
5. Classify each candidate as an in-scope recommendation, supporting context, context-only passage, out-of-scope material, or unresolved material. `context_only` is an audit disposition, not a CSV value.
6. Preserve exact meaning, source location, population, therapy direction, analyte wording, route, formulation, dosing interval, specimen timing, treatment phase, assay or laboratory context, units, and operators. Record missing or ambiguous information without guessing.
7. Split distinct instructions into distinct recommendation records whenever the protocol's unit-of-extraction rules require it, including separate estradiol and testosterone instructions.
8. Use only the controlled vocabularies currently defined by `PROTOCOL.md` and `data/data-dictionary.md`.
9. Assess documented recommendation-level provenance and dependency. Do not infer independence or dependence from blanks, wording, or numerical similarity alone.
10. Form candidate comparison groups only as a screening step. Assess comparability only after complete contextual extraction and only under the protocol-defined rules.
11. Create or modify `SRC`, `REC`, or `EXT` records only when the user explicitly requests research-data changes. Mark every AI-generated draft `pending`, leave human-verification fields blank, and set review flags honestly.
12. Use `pubmed-source-resolution` only for a concrete bibliographic or provenance question. PubMed is not an alternate guidance source and cannot expand the evidence base.
13. After authorized research-data changes, run `python3 scripts/validate_data.py` and `python3 run_tests.py` from the repository root. Do not claim either ran unless it did.
14. Report changed files and affected record IDs; unresolved ambiguity; correction or provenance questions; validator and test results; and required human review.
15. Stop before commit, push, release, or pull-request actions unless the user separately authorizes them.

## Non-negotiable prohibitions

Never:

- mark AI-generated work human verified or fill human-verification fields on a human's behalf;
- invent missing values or unstated numerical boundaries;
- convert qualitative instructions into numerical ranges;
- convert `testosterone_unspecified` to `total_testosterone`;
- treat a PubMed article as a replacement for the authoritative included guidance source;
- use PubMed findings to establish a hormone target, therapeutic range, effectiveness claim, or clinical correctness;
- decide numerical similarity proves comparability, shared provenance, or independent confirmation;
- treat dependent recommendations as independent confirmations;
- silently convert mixed-age or adolescent-and-adult guidance into adult-only guidance;
- drop route, formulation, dosing, specimen-timing, population, treatment-phase, assay, laboratory, or analyte context;
- modify `PROTOCOL.md` merely to accommodate difficult source text;
- change a schema or controlled vocabulary merely to make a passage fit;
- create findings, analysis, visualizations, or releases before the protocol's conditions are met;
- offer clinical advice or patient-specific interpretation; or
- claim a source, correction, command, validator, or test was checked or run when it was not.
