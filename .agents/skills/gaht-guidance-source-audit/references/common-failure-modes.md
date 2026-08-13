# Common failure modes

Use this checklist during extraction, provenance review, comparability review, and completion audits. The current `PROTOCOL.md` remains authoritative.

## Passage and record boundaries

- Treating contextual hormone wording or a rationale sentence as an extractable recommendation.
- Giving a context-only passage a recommendation row or using `context_only` as a CSV value.
- Combining separate estradiol and testosterone instructions from one sentence into one record.
- Combining timing, monitoring frequency, target, qualitative range, or action-trigger instructions that have distinct meanings.
- Missing an eligible non-numeric instruction because it cannot be plotted.
- Treating a conditional dosing action at a threshold as a therapeutic lower or upper target.

## Numerical and analyte overreach

- Converting “should not exceed” wording into an invented interval or treating the contextual lower number as a required floor.
- Inventing a lower or upper boundary, substituting zero for missing data, or importing a laboratory interval from elsewhere.
- Converting a physiologic or qualitative instruction into a numerical target.
- Converting `testosterone_unspecified` to `total_testosterone` or grouping the two automatically.
- Writing a paraphrase that is stronger, broader, or more precise than the source.

## Lost context

- Losing peak, trough, mid-cycle, pre-injection, post-application, or other specimen-timing context.
- Applying a mid-cycle target to peak or trough specimens, or treating those specimens as interchangeable.
- Dropping route, formulation, dosing interval, population, treatment phase, assay, laboratory, unit, operator, or analyte specificity.
- Converting adolescent-and-adult or mixed-age guidance into adult-only guidance.
- Assuming a source-wide population or surrounding section automatically applies to every passage.

## Provenance and correction errors

- Assuming blank upstream-source fields prove independence.
- Assuming matching wording or numbers prove provenance, independence, or repeated confirmation.
- Treating a document-level or table-level citation as conclusive item-level provenance.
- Treating an adjacent citation as the documented source of an instruction without explicit support.
- Treating dependent recommendations as independent confirmations.
- Missing corrections, corrigenda, removal notices, updated versions, or the scope of a correction.
- Retaining superseded wording after a correction or applying an unrelated correction to an unaffected recommendation.

## Comparability and verification errors

- Assuming matching numbers prove direct comparability.
- Assigning direct comparability before preserving all material context in Protocol section 23.
- Treating candidate comparison-group membership as a completed or clinical comparison.
- Marking `testosterone_unspecified` or a physiologic-range record directly comparable contrary to protocol rules.
- Marking AI work verified, filling human-verification fields, or claiming a human decision not explicitly made.
- Changing schema, controlled vocabulary, validation logic, tests, or `PROTOCOL.md` merely to make difficult source text fit.
- Claiming a source, correction, command, validation, or test was checked when it was not.
- Creating findings, charts, releases, or clinical conclusions before the protocol's completion and release conditions are met.
