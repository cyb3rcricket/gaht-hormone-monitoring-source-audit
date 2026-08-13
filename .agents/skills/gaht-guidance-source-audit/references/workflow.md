# Operational workflow

This is a concise execution aid. Read the current `PROTOCOL.md` first; its requirements and controlled vocabularies are authoritative.

## Source start

- Inspect `git status -sb`, the relevant source row, related recommendation rows, evidence notes, and task artifacts before changing anything.
- Confirm the task's authorization boundary. Infrastructure, review, or audit requests do not automatically authorize research-data changes.
- Work one source at a time as required by Protocol section 18.

## Authoritative source and version

- Resolve the issuing organization, official title, document type, version or edition, publication year, DOI when available, official URL, access date, population and geographic scope, and current status.
- Use the official publisher, issuing organization, or authoritative repository. Secondary material may locate a source but cannot replace it. See Protocol sections 6-8 and 14.
- Preserve mixed-age scope. A source-level population does not make every passage apply to every population.

## Correction check

- Check for corrections, corrigenda, updates, removal notices, and superseding versions before completing source verification.
- Determine whether each correction affects an in-scope passage; record what was incorporated and what was reviewed but did not alter extraction.
- Use `pubmed-source-resolution` only if a publication identity or correction cannot be resolved from the authoritative source or publisher metadata.

## Passage review and disposition

- Search the authoritative document for explicit serum estradiol, total testosterone, or unspecified serum testosterone monitoring instructions relevant to the protocol-defined population and therapy directions.
- Classify candidates as:
  - **in-scope recommendation**: an explicit eligible monitoring instruction;
  - **supporting context**: context required to interpret an included instruction;
  - **context-only**: relevant documentary context that does not itself justify a recommendation row;
  - **out of scope**: outside the predefined analytes, population, therapy directions, or recommendation types; or
  - **unresolved**: insufficient evidence for a final disposition.
- General references to hormones or sex steroids are context-only unless an in-scope analyte is explicit. `context_only` is not a CSV controlled value. See Protocol sections 9 and 18.

## Extraction

- Create one record per distinct instruction under Protocol section 11. Split by analyte, therapy direction, route, formulation, dosing schedule, specimen timing, treatment phase, population, operator, recommendation type, source location, or upstream relationship when those distinctions change.
- Preserve source wording in `short_source_excerpt` and write a neutral, no-stronger `faithful_paraphrase`.
- Retain population, route, formulation, dosing interval, specimen timing, assay or laboratory context, treatment phase, unit, operator, and the meaning of a number.
- For `testosterone_unspecified`, preserve the measurement wording, document missing specificity, and never normalize it to `total_testosterone`.
- Represent a conditional action trigger as a conditional action threshold, not as a target boundary. Represent “should not exceed” by its source operator without inventing a floor.
- Use current controlled vocabularies exactly. Difficult text remains ambiguous or pending; it does not justify a schema change.

## Evidence-note creation

- Create one `EXT####.md` evidence note for each `REC####` record using all headings in Protocol section 17.
- Keep excerpts short and sufficient; do not copy full copyrighted documents.
- Match source and recommendation IDs, location, context, provenance, comparability, ambiguity, claims-not-supported, and verification state between CSV and note.

## Ambiguity handling

- Use `not_specified` when that controlled value exists; otherwise leave optional or numeric fields blank as the schema directs.
- Describe unresolved facts in `unknowns` and tempting overinterpretations in `claims_not_supported`.
- If passages conflict, preserve them separately when appropriate, check version and correction history, and require further human review. Do not silently reconcile them. See Protocol section 21.

## Provenance

- Classify each recommendation as `original`, `adapted`, `reproduced`, `derived`, `unclear`, or `not_applicable` only from documented evidence.
- Distinguish document-level, table-level, and item-level attribution. A table-level citation does not prove which cited source supports an individual item.
- A blank upstream field is unresolved absence of a recorded relationship, not proof of independence. See Protocol section 22.

## Comparability

- Assign candidate groups only on broad potentially comparable characteristics; grouping is not a clinical comparison.
- Assess `comparable_status` only after contextual extraction. For direct comparability, all materially relevant dimensions in Protocol section 23 must align.
- Treat missing or different analyte specificity, route, formulation, specimen timing, population, treatment phase, recommendation type, unit, or numerical meaning as reasons to qualify, reject, or defer comparison.
- Preserve dependent recommendations as documentary statements without counting them as independent confirmation.

## Validation and human review

- Keep AI-generated drafts `pending`; leave human verifier and date blank. AI may identify discrepancies but cannot perform Protocol section 19 verification.
- After authorized research-data changes, run from the repository root:

```bash
python3 scripts/validate_data.py
python3 run_tests.py
```

- Resolve validation errors only within the authorized record scope. Do not alter validation logic, tests, schema, vocabulary, or protocol to force a pass.

## Source-completion audit

Before calling a source extraction complete, confirm that:

- authoritative metadata and correction status are established;
- all candidate passages have documented dispositions;
- every included instruction has one synchronized REC/EXT pair;
- distinct instructions remain separate and all material context is retained;
- dependencies, ambiguity, and comparability have been assessed without inference;
- all AI drafts still require explicit human verification;
- validator and tests pass after data changes; and
- the report identifies remaining review requirements without claiming project or release completion.
