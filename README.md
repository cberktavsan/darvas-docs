# Darvas docs (Mintlify)

This directory is the **source of truth** for docs.darvas.app: the data API guide and the custom indicators documentation.

## Architecture

- **Source:** `web/apps/docs/` inside the private `cberktavsan/darvas` monorepo.
- **Mirror:** `cberktavsan/darvas-docs` (public) - auto-synced on every push to `main` by `.github/workflows/sync-docs.yml`.
- **Renderer:** Mintlify - `https://darvas.mintlify.app`.

## Development

```bash
# from monorepo root
cd web/apps/docs
bunx mintlify@latest dev --port 3333
# opens http://localhost:3333 with hot reload
```

Edit any `.mdx` file, save - the browser updates within ~1 second.

## Data API pages

`data-api/openapi.yaml` is written by `web/apps/web/scripts/data-api-openapi.ts` (`bun run data-api-openapi` in `web/apps/web`); never edit it here. Each endpoint page under `data-api/` names its operation in front matter, such as `openapi: "data-api/openapi.yaml GET /insider-trades"`, followed by `hideApiMarker: true` so the menu shows the title without a method badge, and Mintlify adds the reference and the Try it playground below the page's own text. `style.css` holds the layout and color overrides.

## Pages

`docs.json` defines the sidebar/navigation. Every page referenced there must exist as a matching `.mdx` file (no extension in the JSON path; the file extension is implied).

Page front-matter requirements:

```mdx
---
title: "Page title"
description: "One-line summary that appears in search and meta tags."
---
```

## Do NOT edit the public mirror directly

`cberktavsan/darvas-docs` is force-pushed on every monorepo `main` push. Any direct commit to it will be **overwritten** on the next sync.

## How sync works

`.github/workflows/sync-docs.yml`:

1. Triggers on push to `main` that touches `web/apps/docs/**` (or manual `workflow_dispatch`).
2. Checks out the monorepo.
3. Configures an SSH key from `DOCS_SYNC_KEY` secret (write-enabled deploy key on `darvas-docs`).
4. Initializes a fresh git repo inside `web/apps/docs/`.
5. `git push --force` to `git@github.com:cberktavsan/darvas-docs.git`.

The public repo is therefore a **history-less mirror** of this directory. Commit history lives in the monorepo only.
