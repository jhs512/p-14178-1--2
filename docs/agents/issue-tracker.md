# Issue tracker

Issues for this repo are tracked as **local markdown files** under `.scratch/`.

## Where issues live

Each feature or work-stream gets a directory:

```
.scratch/<feature-slug>/
```

Within it, each issue is a numbered markdown file:

```
.scratch/<feature-slug>/001-<issue-slug>.md
.scratch/<feature-slug>/002-<issue-slug>.md
```

## Issue file format

Each issue file starts with frontmatter and a body:

```markdown
---
title: <one-line title>
state: needs-triage      # or any label from docs/agents/triage-labels.md
labels: []
---

## Context

<why this issue exists>

## Acceptance criteria

- [ ] ...
```

## Operations

- **Create**: write a new numbered file in the feature directory
- **List**: read the files under `.scratch/<feature-slug>/`
- **Update state**: edit the `state:` field in the frontmatter
- **Close**: set `state:` to a terminal label (e.g. `wontfix` or `done`)

When asked to "create an issue", default to this layout unless the user specifies otherwise.
