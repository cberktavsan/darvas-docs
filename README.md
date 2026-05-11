# Darvas docs (Mintlify)

This directory is the **source of truth** for the Darvas custom-indicators documentation site.

## Architecture

- **Source:** `web/apps/docs/` inside the private `cberktavsan/darvas` monorepo.
- **Mirror:** `cberktavsan/darvas-docs` (public) - auto-synced on every push to `main` by `.github/workflows/sync-docs.yml`.
- **Renderer:** Mintlify - `https://darvas.mintlify.app`.

## Development

```bash
# from monorepo root
cd web/apps/docs
npx mintlify@latest dev
# opens http://localhost:3000 with hot reload
```

Edit any `.mdx` file, save - the browser updates within ~1 second.

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
