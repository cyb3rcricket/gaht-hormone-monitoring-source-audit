# Fetch and resolve publication records

> **Upstream attribution:** Adapted from Google LLC's [Science Skills `fetch-and-resolve.md`](https://github.com/cyb3rcricket/science-skills/blob/0b42509800f49e6eb7809505d96e20a890ef99bd/skills/pubmed_database/references/fetch-and-resolve.md), copyright 2026 Google LLC, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/legalcode). Modified for this repository to limit retrieval to bibliographic and provenance resolution. The upstream material and this derivative are provided without warranties; this is not an official Google product.

## `fetch_article_abstracts`

Fetch PubMed metadata and abstracts for known candidate PMIDs:

```bash
pubmed_tmp_dir=$(mktemp -d /tmp/gaht-pubmed.XXXXXX)
uv run .agents/skills/pubmed-source-resolution/scripts/pubmed_api.py \
  "$pubmed_tmp_dir/abstracts.json" fetch_article_abstracts \
  '<pmid1>,<pmid2>'
```

Arguments:

- `pmids` (required list): comma-separated PMID strings.
- `webenv` and `query_key` exist upstream but are not part of the local documented workflow.

Each result can include `pmid`, `title`, `authors`, `journal`, `pubdate`, `doi`, and `abstract`. Use metadata to confirm identity or correction relationships. An abstract may help disambiguate a citation, but it cannot establish guidance content or clinical correctness. If title and abstract are both null, report the dead or unpopulated record; do not guess a neighboring PMID.

## `get_full_text_pmc`

Retrieve PMC text only when publication identity, correction scope, or a documented provenance relationship cannot be resolved from metadata:

```bash
pubmed_tmp_dir=$(mktemp -d /tmp/gaht-pubmed.XXXXXX)
uv run .agents/skills/pubmed-source-resolution/scripts/pubmed_api.py \
  "$pubmed_tmp_dir/full-text.json" get_full_text_pmc '<pmid>'
```

The function uses the PMC BioC API and succeeds only for content available through the supported PMC open-access path. Failure may mean the article is unavailable, embargoed, or outside that subset; it does not prove the article does not exist. Keep full text temporary, quote only what is necessary and license-permitted, and never add a full copyrighted publication to the repository.

## `fetch_database_summary`

For this project, use only `database=pubmed` to retrieve summary metadata for known PubMed UIDs:

```bash
pubmed_tmp_dir=$(mktemp -d /tmp/gaht-pubmed.XXXXXX)
uv run .agents/skills/pubmed-source-resolution/scripts/pubmed_api.py \
  "$pubmed_tmp_dir/summary.json" fetch_database_summary \
  pubmed '<pmid1>,<pmid2>'
```

Arguments:

- `database` (required): use `pubmed` in this audit.
- `id_list` (required list): comma-separated PubMed IDs.

Output fields are NCBI database-specific. Inspect returned metadata rather than assuming a fixed schema. Gene, protein, nucleotide, PubChem, or biological-association resolution is out of scope even though the unchanged upstream wrapper can technically call other NCBI databases.

## Resolution checks

Before reporting a match, compare all available fields: title, first author, journal, year, volume or issue when available, pages or article number, DOI, PMID, and publication type. For correction work, verify which original article the notice corrects and whether the corrected passage affects this audit. Preserve unresolved conflicts explicitly.
