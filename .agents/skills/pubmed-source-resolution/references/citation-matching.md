# Citation matching

> **Upstream attribution:** Adapted from Google LLC's [Science Skills `citation-matching.md`](https://github.com/cyb3rcricket/science-skills/blob/0b42509800f49e6eb7809505d96e20a890ef99bd/skills/pubmed_database/references/citation-matching.md), copyright 2026 Google LLC, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/legalcode). Modified for this repository to limit citation matching to bibliographic and provenance resolution and to reflect the included wrapper's actual output. The upstream material and this derivative are provided without warranties; this is not an official Google product.

Use `match_raw_citations` to resolve an incomplete structured citation to a PMID. Do not use it for literature discovery.

## Citation format

Each citation is one string with seven pipe-delimited fields and a trailing pipe:

```text
journal|year|volume|first_page|author_name|key|
```

Only `journal` is required by the endpoint. Empty segments are valid. Use the first author's surname plus first initial, lowercase, without commas or periods; prefer an NLM journal abbreviation when known.

Examples:

```text
example journal|2020|12|34|author a|ref1|
example journal|2021|13|56|author b|ref2|
```

## Command

```bash
pubmed_tmp_dir=$(mktemp -d /tmp/gaht-pubmed.XXXXXX)
uv run .agents/skills/pubmed-source-resolution/scripts/pubmed_api.py \
  "$pubmed_tmp_dir/matches.json" match_raw_citations \
  'example journal|2020|12|34|author a|ref1|'
```

Pass multiple citations as one comma-separated argument. The included wrapper returns only a JSON list of matched PMIDs and silently omits unmatched citations; it does not preserve keys in its output. When one-to-one identity matters, resolve citations separately or independently verify every returned PMID with `fetch_article_abstracts`.

## Matching strategy

Strong combinations include:

- journal + first author + year;
- journal + volume + first page; or
- journal + year + volume + first page + first author when every field is reliable.

Partly wrong metadata can prevent a match. If no result is returned:

1. remove the least reliable field, often first page or volume;
2. try the full journal name or NLM abbreviation;
3. search PubMed by DOI or distinctive title;
4. fetch candidate metadata and report any remaining ambiguity.

Never infer that an unmatched citation is nonexistent or that a matched article establishes recommendation-level dependency. Matching resolves identity only; provenance requires explicit documentary support.
