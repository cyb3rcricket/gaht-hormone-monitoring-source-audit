# Targeted PubMed search and discovery

> **Upstream attribution:** Adapted from Google LLC's [Science Skills `search-and-discovery.md`](https://github.com/madebytommi/science-skills/blob/0b42509800f49e6eb7809505d96e20a890ef99bd/skills/pubmed_database/references/search-and-discovery.md), copyright 2026 Google LLC, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/legalcode). Modified for this repository to limit search to bibliographic and provenance resolution. The upstream material and this derivative are provided without warranties; this is not an official Google product.

Use `search_pubmed` only to resolve a known or incomplete publication, DOI, correction, corrigendum, or explicitly documented upstream reference. Do not use it to discover clinical recommendations or expand the evidence base.

## Command

```bash
pubmed_tmp_dir=$(mktemp -d /tmp/gaht-pubmed.XXXXXX)
uv run .agents/skills/pubmed-source-resolution/scripts/pubmed_api.py \
  "$pubmed_tmp_dir/search.json" search_pubmed \
  '<query>' --max_results 5 --sort_by relevance
```

Arguments:

- `query` (required string): PubMed free-text or structured query.
- `max_results` (optional integer, default `10`): cap results tightly for identity resolution.
- `sort_by` (optional string, default `relevance`): `relevance`, `pub_date`, `Author`, `JournalName`, or `Title`.

Output: a JSON list of PMID strings.

## Resolution strategy

Use the most specific known identifier or metadata first:

1. DOI: `10.xxxx/xxxxx[doi]`.
2. Exact or distinctive title: `"title text"[ti]`.
3. First author, journal, and year.
4. Original publication identity plus `correction[pt]`, `published erratum[pt]`, `corrigendum`, or known correction metadata.

Common PubMed tags useful for targeted resolution:

- `[doi]`: DOI
- `[ti]`: title
- `[au]`: author
- `[jour]`: journal
- `[dp]`: publication date
- `[pt]`: publication type

Use Boolean operators and parentheses explicitly. Avoid over-quoting uncertain titles because exact phrases can exclude the intended record.

## Bounded fallback

If a query fails, try at most three to five materially different, high-quality variants:

1. remove an uncertain page, volume, or exact-phrase constraint;
2. try the full journal name or NLM abbreviation;
3. search a distinctive title fragment plus author or year;
4. use `match_raw_citations` when structured citation fields are available.

If multiple plausible records remain, fetch their metadata and report the ambiguity. Do not select the closest result by intuition. `global_database_discovery` and non-PubMed database exploration are out of scope.
