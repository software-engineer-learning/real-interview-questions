# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Write-ups of questions actually asked in real interviews. There is no build or test
suite — the "product" is plain CommonMark markdown plus the hand-maintained
navigation file. It is one of the content repos published at
[swe.springlee.dev](https://swe.springlee.dev) (section: *Real Interview Questions*)
by the sibling `swe-site` repo.

```text
README.md            # section landing page
SUMMARY.md           # navigation — hand-maintained, GitBook format
companies/
  README.md          # company index table
  <company-slug>/
    README.md        # company overview: rounds, format, timeline
    <question-slug>.md
```

## Conventions

### Naming

- Company folders: lowercase kebab-case (`grab/`, `shopee/`, `line-plus/`).
- Question files: lowercase kebab-case describing the question, not the round
  (`design-a-rate-limiter.md`, `why-is-hashmap-not-thread-safe.md`).
- Group several questions in one file only when they came from the same round and
  read as a unit; otherwise one question per file.

### Question page template

```md
# <Question as it was asked>

- **Company:** <name>
- **Round:** <phone screen | technical 1 | system design | hiring manager | onsite>
- **Role:** <e.g. Senior Backend Engineer, Java>
- **Asked:** <YYYY-MM>

## What they were probing

Why this question was asked — the signal the interviewer wanted.

## Answer

The answer, written the way you would deliver it out loud.

## Follow-ups

- <follow-up question> — <short answer>

## Notes

What went well, what to say differently next time, resources to read.
```

Keep the metadata block: it is what makes this section different from the
theory-first `swe` section. Use `_unknown_` rather than dropping a field.

### Math and code fences

Same rules as the other content repos, because the same MkDocs build renders them:

- Use `$$...$$` (KaTeX) for math, e.g. `$$O(n \log n)$$`. Single `$...$` is not
  rendered.
- Open code fences with three backticks and a bare language token, nothing else
  (`go`, not `go []`), and always close the final fence — strict CommonMark
  parsers (MkDocs/pymdown-superfences) mangle the rest of the page otherwise.

## Navigation

`SUMMARY.md` is **hand-maintained** — there are no generator scripts here (unlike
`leetcode-algorithms`). It uses GitBook format (`## Heading` sections, 2-space
nested lists) because `swe-site/scripts/convert_summary.py` converts it into
mkdocs-literate-nav format at build time.

When adding, renaming, or removing a page, update all three:

1. `SUMMARY.md` — the site navigation.
2. `companies/README.md` — the company index table.
3. `README.md` — the "Jump right in" links, if a new top-level area appears.

## Publishing

Pushes to `main` fire `.github/workflows/trigger-site-deploy.yml`, which sends a
`content-updated` `repository_dispatch` to `lyxuansang91/swe-site` to rebuild the
site. It needs the `SITE_DISPATCH_TOKEN` secret: a fine-grained PAT for
`lyxuansang91/swe-site` with **Contents: read and write**.

To preview locally, run `bash scripts/prepare.sh && mkdocs serve` from the sibling
`swe-site` checkout — it picks up this working tree, not GitHub.
