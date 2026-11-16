---
name: update-github-info
description: Refresh the GitHub Info website with practical, sourced updates from the GitHub Blog, Changelog, and Awesome Copilot.
intent: Keep Mona's GitHub Info content current with concise, useful developer guidance backed by official GitHub sources.
on:
  schedule: daily
  workflow_dispatch:
  skip-if-match: 'is:pr is:open "gh-aw-workflow-id: update-github-info" in:body'
permissions:
  contents: read
  pull-requests: read
engine:
  id: copilot
  copilot-sdk: true
tools:
  edit: true
  web-fetch:
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    base-branch: main
    draft: false
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Read `notes/mona-notes.md` and the current `site/content/github-info.md`. Web-fetch all three:

- https://github.blog/latest/
- https://github.blog/changelog/
- https://awesome-copilot.github.com/workflows/

Select up to five recent, verified updates or workflows that are useful to developers and fit Mona's editorial guidance. Refresh `site/content/github-info.md` with short, practical summaries, preserving its existing purpose and structure. Cite the specific source link for every item. Do not invent details or add claims unsupported by the fetched pages.

When there are relevant updates and the file would materially change, use the configured `create-pull-request` safe output to open a non-draft pull request against `main` for Mona to review. Address Mona in the PR description, summarize the proposed content change, and include the source links. Do not write directly to `main` or change any other file.

Call `noop` with a brief reason if either source cannot be fetched, no sufficiently relevant and verified updates are available, or the file needs no changes. Do not open an empty or duplicate pull request.
