# Furiosa Docs

Central documentation repository for FuriosaAI, built on [Mintlify](https://mintlify.com). This repo is the **center of documentation**: content authored here is the source of truth, and selected content is synchronized from product repositories.

## Architecture

### Repository roles

| Content | Home | Sync |
|---|---|---|
| Docs authored for the docs site (guides, overviews, releases) | this repo | two-way sync supported |
| furiosa-runtime developer docs, Cloud-Native Toolkit, HW manual | **moved into this repo** (ported from `furiosa-runtime/docs/developer`) | maintained here |
| Example source code | individual product repos (e.g. `furiosa-runtime/docs/developer/examples`) | tested by CI/CD in the source repo, then AI-synced into this repo |
| Other repos' docs (TCL, vISA, Torch, …) that prefer to stay in their repo | their own repo | one-way sync into this repo |

### Example-code sync boundary

Example code shown in the docs lives under [`snippets/examples/`](snippets/examples/) — one MDX snippet per example file, imported by pages that show it. This is the machine boundary for the CI/CD sync pipeline:

- Source of truth: example files in the product repo, exercised by that repo's CI.
- The sync pipeline rewrites only `snippets/examples/*` — never page prose.
- Do not hand-edit files in `snippets/examples/`; change the source example in the product repo instead.

## Site structure

Four navigation tabs (see `docs.json`):

- **Software Stack** — overview, supported models, roadmap, release notes, getting started, device management (SMI, host tuning)
- **Furiosa-LLM** — serving guides, Python API reference, examples
- **Cloud-Native Toolkit** — container support, Kubernetes components (device plugin, feature discovery, metrics exporter, NPU operator, DRA driver), LLM-D, Furiosa-LLM Kubernetes deployment
- **Hardware Manual** — RNGD specification

## Conventions

- Content ported from Sphinx RST is a faithful, syntax-only conversion — prose preserved verbatim.
- Internal links are root-relative without extensions (`/furiosa-llm/intro`).
- Every page has `title` (+ `description`) frontmatter.
- Branding: `docs.json` (colors, logo) + `style.css` (generated from `@furiosa-ai/ui-kit` — do not hand-edit) + `custom.css` (hand-maintained overrides).

## Development

```bash
npm i -g mint
mint dev            # local preview at localhost:3000
mint validate       # build validation
mint broken-links   # internal link check
```

Pushing to `main` deploys automatically once the Mintlify GitHub App is installed.

## Roadmap (phase 2)

- CI/CD example-sync pipeline (product repos → `snippets/examples/`)
- OpenAPI spec + interactive playground for the Furiosa-LLM OpenAI-compatible server
- One-way sync onboarding for TCL / vISA / Torch docs
- Versioned docs per release, custom domain, analytics
