---
name: pubmed-source-resolution
description: Resolve bibliographic citations, PMIDs, DOIs, publication identity and metadata, corrections or corrigenda, incomplete references, and explicitly documented upstream-source relationships for the GAHT hormone-monitoring source audit. Use PMC text only when necessary for legitimate bibliographic or provenance resolution; never use this skill for clinical inference, therapeutic targets, evidence-base expansion, or literature reviews.
---

# PubMed Source Resolution

## Narrow purpose

Use this helper only for a concrete bibliographic or provenance question within the predefined GAHT guidance source audit:

- resolve a citation to a PMID, DOI, or publication identity;
- retrieve publication metadata or an abstract to disambiguate identity;
- locate and identify a correction or corrigendum;
- resolve an incomplete reference;
- investigate an explicitly documented upstream-source relationship; or
- retrieve PMC text only when needed to answer one of those questions.

`PROTOCOL.md` and the `gaht-guidance-source-audit` skill govern how any result may be used. PubMed metadata or text never replaces the authoritative included guidance document.

## Prohibited uses

Do not use this skill to:

- establish or extract clinical recommendations;
- identify the “best” hormone target or determine therapeutic ranges;
- evaluate treatment effectiveness, safety, superiority, or clinical correctness;
- expand the project's predefined evidence base;
- perform broad biomedical evidence synthesis, a literature review, systematic review, or scoping review unless a future approved protocol amendment explicitly authorizes it;
- investigate genes, proteins, nucleotides, PubChem, biological associations, or genomic questions; or
- infer recommendation-level provenance merely from similar wording, numerical values, citations nearby, or an article's subject matter.

## Before invoking the script

1. Read the current `PROTOCOL.md` and identify the exact bibliographic or provenance question.
2. Confirm that the question does not add a source, recommendation, analyte, or research aim.
3. Read the reference for the function you will use; do not infer arguments:
   - [references/search-and-discovery.md](references/search-and-discovery.md) for `search_pubmed`;
   - [references/fetch-and-resolve.md](references/fetch-and-resolve.md) for `fetch_article_abstracts`, `get_full_text_pmc`, or `fetch_database_summary`; or
   - [references/citation-matching.md](references/citation-matching.md) for `match_raw_citations`.
4. Read [references/UPSTREAM.md](references/UPSTREAM.md) when reviewing provenance, dependencies, or synchronization.
5. Keep output in a unique temporary directory outside the repository. Never store API keys, `.env` files, downloaded full text, or broad result sets in the repository.

## Runtime and command form

The bundled script declares its own Python requirements through inline `uv` metadata:

- Python 3.10 or newer
- `polite-http`
- `python-dotenv`

No main-project package manifest and no separate credentials or `uv` Agent Skill is required. The `uv` executable must already be available in the agent environment; do not introduce a project package-management system solely for this helper.

From the repository root, use:

```bash
pubmed_tmp_dir=$(mktemp -d /tmp/gaht-pubmed.XXXXXX)
uv run .agents/skills/pubmed-source-resolution/scripts/pubmed_api.py \
  "$pubmed_tmp_dir/result.json" <function_name> <arguments>
```

The output path must not already exist. Successful calls write JSON there; failures exit nonzero. Inspect the JSON directly and report the exact query, returned identifiers, unresolved ambiguity, and access date. Never claim a call succeeded without checking its exit code and output.

## Credentials and responsible use

`NCBI_API_KEY`, `USER_EMAIL`, and `NCBI_TOOL` are optional environment variables. The script works without them at the lower rate limit. Never request, create, print, or commit a secret unless the user explicitly authorizes a separate credential workflow. The script may read optional values from `~/.env`; it does not require that file to exist.

Use targeted, bounded queries and the script's rate-limited wrapper. Respect [NCBI policies](https://www.ncbi.nlm.nih.gov/home/about/policies/), the [PubMed disclaimer](https://pubmed.ncbi.nlm.nih.gov/disclaimer/), and each publication's license. Do not copy full article text into the repository.

## Allowed functions only

Document and call only:

- `search_pubmed`
- `fetch_article_abstracts`
- `match_raw_citations`
- `get_full_text_pmc`
- `fetch_database_summary` (use `pubmed` for this audit)

The unchanged upstream script retains other functions for synchronization safety. They are out of scope and must not be called or presented as available project workflows.

## Result boundary

Treat results as bibliographic evidence only. Cross-check a resolved identity against the authoritative publisher or issuing organization when the project requires authoritative source status, version, correction, or guidance content. Record uncertainty rather than selecting a plausible match. Do not create or modify `SRC`, `REC`, or `EXT` records unless the task explicitly authorizes research-data changes; any AI-drafted record remains `pending`.
