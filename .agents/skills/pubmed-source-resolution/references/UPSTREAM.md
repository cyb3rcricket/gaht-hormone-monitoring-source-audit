# Upstream provenance

Original upstream:
google-deepmind/science-skills

Reference fork:
cyb3rcricket/science-skills

Source skill:
skills/pubmed_database

Pinned reference commit:
0b42509800f49e6eb7809505d96e20a890ef99bd

Local purpose:
Bibliographic and source-provenance resolution for the GAHT hormone-monitoring source audit.

Local policy:
The GAHT-specific SKILL.md intentionally narrows the upstream PubMed capability.
PubMed must not be used for clinical inference or expansion of the project's predefined evidence base.

## Local copy status

`scripts/pubmed_api.py` was copied unchanged from `skills/pubmed_database/scripts/pubmed_api.py` at the pinned reference commit. Its upstream SHA-256 is:

```text
f67f40116ef17678b074f42cf0e96df06eaeb247152cf43deb89d77f569a2478
```

The unchanged copy retains the upstream Google LLC copyright and Apache 2.0 header, inline `uv` dependencies (`polite-http` and `python-dotenv`), NCBI rate-limiting logic, and functions outside this project's scope. Retention of unused functions avoids rewriting working networking code and makes byte-level upstream comparison possible; the local `SKILL.md` prohibits their use.

The local `search-and-discovery.md`, `fetch-and-resolve.md`, and `citation-matching.md` are modified, shortened derivatives of the upstream references. They:

- replace upstream bundle-relative commands with project-local paths;
- remove unrelated discovery, biological linking, bulk synthesis, and credential-skill workflows;
- narrow all examples to bibliographic and provenance resolution;
- require temporary output outside the repository; and
- document the included citation wrapper's PMID-only output.

## Licensing

The pinned reference fork's [README licensing terms](https://github.com/cyb3rcricket/science-skills/blob/0b42509800f49e6eb7809505d96e20a890ef99bd/README.md#licensing--disclaimer) state that software is licensed under Apache 2.0 and all other materials are licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

Accordingly:

- `scripts/pubmed_api.py` remains unchanged Apache-2.0 software with its Google LLC copyright and license header preserved.
- The adapted `search-and-discovery.md`, `fetch-and-resolve.md`, and `citation-matching.md` reference files are distributed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/legalcode). Each file identifies Google LLC as the upstream creator and copyright holder, links to its exact pinned source and the license, states that it was modified, retains the upstream warranty disclaimer, and notes that this is not an official Google product.

Copyright 2026 Google LLC applies to the upstream material. The modification summary above identifies the local adaptations.

The full Apache License 2.0 text from the reference fork's root `LICENSE` is copied unchanged as `LICENSE-APACHE-2.0.txt` for the software. No upstream `NOTICE` file was present at the inspected commit. The repository's root license is not changed.
