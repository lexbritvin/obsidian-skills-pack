---
name: xquik-research-notes
description: Build Obsidian notes and Dataview dashboards from Xquik REST API exports for X/Twitter research workflows. Use this skill when the user wants to organize X/Twitter search results, profiles, timelines, trends, community data, or monitor outputs from Xquik into durable vault notes, campaign research pages, or Dataview-ready frontmatter. Trigger when the user mentions Xquik, X/Twitter audience research, tweet search archives, social monitoring notes, trend tracking, or converting API JSON into Obsidian pages.
---

# Xquik Research Notes

Use this skill to turn Xquik API responses into clean Obsidian notes and Dataview-friendly metadata.

## Source Boundaries

- Use Xquik's public REST API and docs as source truth.
- Keep API keys in the user's approved secret store or environment.
- Do not paste, log, or commit API keys.
- Do not infer private account data that is not present in the response.

## Note Shape

Create one note per durable research object:

- Tweet or thread notes for `/api/v1/x/tweets/{id}` and thread-style exports.
- User notes for `/api/v1/x/users/{id}` and profile exports.
- Trend notes for `/api/v1/x/trends`.
- Community notes for `/api/v1/x/communities/*`.
- Search batch notes for `/api/v1/x/tweets/search`.

Use YAML frontmatter for fields that Dataview should query:

```yaml
source: xquik
type: tweet
x_id: "1234567890"
author: "example"
captured_at: "2026-07-03"
tags:
  - xquik
  - x-research
```

## Dataview Dashboards

Prefer simple Dataview tables before custom DataviewJS.

```dataview
TABLE author, captured_at, tags
FROM "Research/X"
WHERE source = "xquik"
SORT captured_at DESC
```

Use DataviewJS only when the view needs grouping, computed metrics, or custom rendering.

## Workflow

1. Confirm the user has a Xquik API key available outside the note content.
2. Fetch the requested Xquik endpoint or read the user's exported JSON.
3. Normalize IDs as strings to avoid precision loss.
4. Preserve source URLs when present.
5. Write concise notes with frontmatter for filtering and body sections for context.
6. Add or update a Dataview dashboard only when the user wants recurring review.

## Safety Checks

- Never store API keys in frontmatter or code blocks.
- Mark uncertain derived labels as notes, not facts.
- Keep raw JSON in attachments only when the user requests an archive.
- Avoid automated follow, like, repost, or write actions from research notes.
