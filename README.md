# Transcript Viewer

A single-page viewer for long AI-safety **audit transcripts** — the kind produced
by automated alignment-auditing harnesses (Petri / Gram-style), where an
**auditor** model drives a **target** agent through a scenario and one or more
**judges** score the resulting transcript.

It renders the full auditor↔target↔judge exchange in a readable "Lanes" layout:
grouped tool calls, branch navigation, target-activity silhouettes, inline judge
highlights, and per-dimension scores — built to stay legible even on transcripts
thousands of events long.

## Quick start

```bash
npm install
npm run dev        # http://localhost:5173
```

Then open the app and pick an audit from the list.

## Data

The viewer is data-only at runtime — it loads JSON from `public/data/` via
`fetch`, so it has no build-time coupling to any harness:

```
public/data/
  index.json          # array of audit summaries (id, title, models, scores, …)
  <id>.json           # one file per transcript (events, branches, judge block)
```

`public/data/` is **git-ignored** — you supply your own transcripts. Drop the
JSON files in, refresh, and they appear. The expected shape of each file is
defined by the TypeScript types in [`src/lib/types.ts`](src/lib/types.ts)
(`AuditIndexEntry` for `index.json`, `TranscriptData` for the per-audit files).

Transcripts are typically generated from evaluation logs by a separate dump step
in your auditing harness; this repo only consumes the resulting JSON.

> ⚠️ Audit transcripts often embed full scenario content (and may contain canary
> or benchmark strings that must not be published). Keep `public/data/` private;
> scrub any transcript before committing it anywhere public.

## Build

```bash
npm run build      # emits a self-contained dist/ (inlined via vite-plugin-singlefile)
npm run preview
npm run check      # svelte-check + tsc
```

The build inlines JS/CSS into a single `index.html`, so `dist/` can be served as
static files or opened directly.

## Stack

Svelte 5 · TypeScript · Vite · `marked` + `highlight.js` for message rendering.

`mockups/` holds the original static design explorations that led to the current
layout — reference only, not part of the app.
