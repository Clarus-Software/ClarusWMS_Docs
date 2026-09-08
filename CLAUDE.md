# ClarusWMS Documentation

Mintlify documentation site for ClarusWMS — warehouse management platform docs, AI architecture guides, and API reference.

## Quick Reference

- Dev server: `mintlify dev`
- Export draw.io to SVG: `/Applications/draw.io.app/Contents/MacOS/draw.io --export --format svg --output <output.drawio.svg> <input.drawio>`

## Directory Structure

```
architecture/       # Architecture documentation (MDX)
  ai/              # AI layer architecture pages
  deployment/      # CI/CD and deployment pages
api-reference/     # API docs and OpenAPI spec
essentials/        # Mintlify framework guide pages
images/            # Static images
  diagrams/        # Draw.io sources (.drawio) and SVG exports (.drawio.svg)
  knowledgebase/   # KB article screenshots (shared EN/NL)
integrations/      # Integration guides
knowledgebase/     # Support knowledgebase articles (English)
  getting-started/ # Login, password reset, booking diary
  users-and-roles/ # User creation, role guides, visibility
  accounts/        # Account setup, linking, dispatch strategies
  warehouse-setup/ # Sites, warehouses, locations, storage, pickfaces
  products-and-stock/ # Products, stock management, BOMs
  inbound/         # Goods in, receipts, putaway
  outbound/        # Sales orders, picking, packing, dispatch
  financial/       # Charges, invoices, suppliers
  reporting/       # Reports, data grids, scheduled reports
  integrations/    # SFTP, webhooks, DHL, ASN
  cloud-print/     # Cloud Print setup and usage
  hardware/        # HHD configuration
  advanced/        # Serial numbers, kits, works orders
  security/        # Subprocessors, backup
  troubleshooting/ # Error guides, diagnostic articles
  nl/              # Dutch translations (mirrors EN structure, same slugs)
drafts/            # Non-KB articles (announcements, policies) — NOT in navigation
product-updates/   # Changelog / product updates (public tab)
  nl/              # Dutch mirror, same slugs
snippets/          # Reusable MDX snippets
```

## Tech Stack

- **Framework**: Mintlify (MDX-based documentation)
- **Diagrams**: draw.io (XML format, exported as SVG)
- **Config**: `docs.json` (navigation, theme, API playground)

## Conventions

### Mintlify Components

Use Mintlify's built-in components for structured content:
- `<Tabs>` / `<Tab>` for parallel content paths
- `<Steps>` / `<Step>` for sequential flows
- `<Frame>` wrapping `<img>` for zoomable diagrams
- `<Accordion>` for collapsible detail sections
- `<Info>`, `<Tip>`, `<Warning>`, `<Note>` for callouts
- `<CardGroup>` / `<Card>` for navigation grids

### Diagram Embedding Pattern

Every draw.io diagram follows this pattern in MDX:

```mdx
<Frame>
  <img src="/images/diagrams/diagram-name.drawio.svg" alt="Descriptive alt text" zoom />
</Frame>

<sub><a href="/images/diagrams/diagram-name.drawio.svg" download="diagram-name.drawio.svg">Download SVG</a> · <a href="/images/diagrams/diagram-name.drawio" download="diagram-name.drawio">Download draw.io source</a></sub>
```

### File Naming

- MDX pages: kebab-case (`overview.mdx`, `ai-pipelines.mdx`)
- Draw.io sources: `description.drawio`
- Draw.io SVG exports: `description.drawio.svg` (include `.drawio` before `.svg`)
- Static images: kebab-case (`hero-dark.png`)

## Draw.io Practices

### Writing .drawio Files

- Write draw.io XML directly rather than using MCP draw.io tools for complex diagrams. The MCP `batch_insert_vertices` tool is unreliable — vertices may not persist to the saved file.
- **Never put XML comments (`<!-- ... -->`) in .drawio files.** They break the draw.io CLI exporter at scale. The export silently fails or produces corrupt output.
- Use `mxgraph.aws4.resourceIcon` with `resIcon` for AWS architecture icons (e.g., `resIcon=mxgraph.aws4.lambda`).
- Use `mxgraph.aws4.group` shapes for AWS grouping containers (VPCs, accounts, etc.).

### Sequence Diagrams in Draw.io

For sequence/flow diagrams (not architecture diagrams):
- Use invisible 4×4 ellipse anchor dots placed along lifelines as connection points.
- Connect edges between anchor dots with explicit `source` and `target` references.
- This gives precise control over where arrows start and end on vertical lifelines.

### Diagram Style Conventions

- Gray background boxes (`fillColor=#EFF0F3;strokeColor=none`) for component groups
- AWS icon fill colors follow the official AWS palette (Lambda=#D05C17, API Gateway=#E7157B, Bedrock=#01A88D, etc.)
- Dashed orange edges (`strokeColor=#ea580c;dashed=1`) for MCP/internal connections
- Solid dark edges (`strokeColor=#232F3E`) for standard request flows
- White font on dark icon backgrounds, dark font (#232F3E) on light backgrounds
- `labelBackgroundColor=#EFF0F3` on labels over gray group backgrounds

### Exporting

```bash
/Applications/draw.io.app/Contents/MacOS/draw.io --export --format svg --output images/diagrams/name.drawio.svg images/diagrams/name.drawio
```

The CLI sometimes reports "Error: Export failed" even on success — verify by checking the output file timestamp and content.

## Knowledgebase Articles

### Editorial Standards

- **Voice**: Second-person ("you"), active, concise
- **Structure**: Lead with key information (inverted pyramid). Don't bury the action behind lengthy introductions.
- **Titles**: Strip "Clarus WMS" / "in Clarus WMS" — the site context makes it obvious. Keep titles short and action-oriented.
- **Frontmatter**: Every page needs `title`, `description` (search-optimised, 1-2 sentences). Add `sidebarTitle` only when the title is too long for the sidebar.
- **Stale content**: Add `needs_review: true` to frontmatter if article content may be outdated

### Content Patterns

- Sequential instructions → `<Steps>` / `<Step>` components
- Supplementary Q&A that adds value beyond the main content → `<AccordionGroup>` / `<Accordion>` at the bottom
- Strip FAQ sections that merely restate what the main content already covers
- UI element references → `**bold**` (e.g., click **Save**)
- Screenshots → `<Frame><img src="/images/knowledgebase/descriptive-name.png" alt="Descriptive alt text" /></Frame>`
- Cross-references to other KB articles → `[link text](/knowledgebase/category/slug)`
- Don't repeat generic functionality on every page. Data grid customisation (columns, views, filters) is covered in the "Using Clarus" section — link to it rather than re-explaining it.

### Callout Usage

- `<Info>` — supplementary context the reader should be aware of
- `<Tip>` — helpful advice or best practice
- `<Warning>` — something that could cause problems if ignored
- `<Note>` — additional reference information

### i18n / Dutch Translations

- Dutch pages live in `knowledgebase/nl/{category}/` using the **same slug filenames** as English
- Dutch pages share the same images as English (images are language-agnostic)
- Light cleanup only for Dutch: fix spacing around bold tags, fix broken formatting. Do NOT rewrite for fluency.
- System terms (GIBAY, GOBAY, field names in the UI) stay in English in Dutch articles — they are not translated in the product
- Navigation uses `languages` array in `docs.json`: English has all tabs, Dutch only has "Kennisbank"
- Always update both EN and NL when making content changes

### Navigation Structure

- `docs.json` uses nested collapsible groups to keep the sidebar manageable
- Top-level groups: Getting Started, Using Clarus, System Setup, Products & Stock, Warehouse Operations, Financial, Reporting, Integrations & Tools, Advanced & Reference, Troubleshooting
- Sub-groups use `"expanded": false` to collapse by default
- Troubleshooting articles live exclusively in the troubleshooting section — feature pages link to them with cross-references
- New KB pages must be added to both the EN and NL navigation in `docs.json`

### Image Conventions

- Save to `images/knowledgebase/` with descriptive kebab-case names (e.g., `packing-desk-scan-barcode.png`)
- Naming is language-agnostic — shared between EN and NL pages
- Use the article slug as a prefix with a number suffix (e.g., `serial-number-tracking-1.png`, `serial-number-tracking-2.png`)

### MDX Gotchas

- Curly braces `{{ }}` in MDX are parsed as JSX expressions — wrap in backtick inline code or fenced code blocks to prevent parse errors
- Always use ````text` or ````liquid` for template syntax examples containing `{{ }}`

## Product Updates / Changelog

`product-updates/` is a public tab holding an overview page and a running changelog, mirrored in Dutch under `product-updates/nl/` with the same slugs.

### Structure

- **One `<Update>` per calendar month**, newest first. Label is the month (`September 2026`), `description` is the date range covered.
- Inside an entry: named `##` sections for that month's significant features, then `## New`, `## Improved`, `## Fixed` lists.
- Group related tickets into one feature story. Lead with the user-visible outcome, never the ticket title.

### Mintlify mechanics

- `rss: true` in frontmatter gives a subscribe button and a feed at `<page>/rss.xml`. **RSS only works on public docs**, so the tab and its groups need `"public": true`.
- **Do NOT add `tags` to `<Update>`.** Tags replace the right-hand table of contents with tag filters — you get one or the other, and the per-entry jump links are more useful here. Entries without tags are also hidden whenever a filter is active.
- Give every entry an explicit `rss={{ title, description }}`. RSS entries strip components, so without it a subscriber gets an entry gutted of its callouts and tables.
- Labels become the anchor and the table-of-contents text, so keep them unique and short enough to read in a narrow panel.

### Where the detail goes

The changelog says **what changed and when**; the knowledgebase says **how to use it**. Link to the guide rather than repeating it — and if no guide covers the feature, write or extend one rather than leaving a dead end or explaining it only in the changelog.

The exception, which stays in the changelog because it would age badly in a guide:

- What the feature **replaces**, and the old way of doing it.
- **Rollout state** — coming out from behind a feature flag, or reaching all customers after a test group.
- **Migration notes** and differences between versions of a feature.

### Wording rules

- **Configurable features — especially HHD/handheld flows — are "possible to add", not automatic.** HHD flows are configured per customer, so a change makes something *available to configure*; it does not turn up on anyone's device. Write "can now be configured to show…", not "the handheld now shows…".
- **Performance and scale work gets benefit wording, not before-and-after.** "More efficient pick processing" — not "pick processing now scales up so orders no longer queue", which implies it was broken. Avoid "instead of timing out", "no longer fails", "no longer queues".
- Fixes: describe the corrected behaviour, not the depth of the defect. Never imply a customer's data was at risk.
- UI element references → `**bold**`, using the product's exact wording (see below).

### Never publish

- **Customer or account names.** Genericise a customer-specific fix, or leave it out. Check against the account tags on the ClickUp task.
- **Security specifics** — penetration test findings, the class of a vulnerability, or anything that dates one. A security fix framed as "closing a finding from our recent pen test" tells an attacker what to look for on unpatched versions. Leave security work out, or state the positive assurance only.
- **Multi-tenancy and capacity internals** — tenants, shared capacity, per-subdomain limits, "one busy tenant cannot exhaust…", or that an incident affected several customers ("across multiple domains").
- **Commercial mechanics** — plans, credits, entitlement, quotas.
- **Internal delivery process** — "the implementation team can now…", "without a developer", "previously hard-coded per customer".
- **Third-party vendors and implementation detail** — the SDK behind a widget, bundle splitting, cloud provider names, server time.
- **Anything still behind a feature flag or not yet GA.** Check the ClickUp status and the Notion PRD status before writing an entry as generally available.
- **Internal-only concepts.** Some things in the product exist for Clarus staff, not customers — **task targets** are one. A completed ticket is not evidence that a change is customer-facing. If only internal users configure or see it, leave it out; if you cannot tell from the ticket, ask.

### Checking facts before writing

Never write an entry from a ticket title alone. In rough order of authority:

1. **ClickUp** — the full task description and acceptance criteria, not the name. Ticks and crosses against ACs show what actually shipped.
2. **Notion** — PRDs under *Planning & PRDs*, and the sales enablement product-launch pages, which are already written at a customer-facing altitude. Check the PRD **Status**: an epic still `In progress` means the feature it feeds has not shipped, even if a child ticket is complete.
3. **The product repos** — `claruswms_backend` and `claruswms_frontend` (attach with `add_repo`) for validations, constraints and guardrails the ticket only implies.
4. **`claruswms_frontend/translations/en-gb.json` and `nl.json`** for the *exact* UI strings, including Dutch. Use these for Dutch pages rather than translating UI terms yourself.

### Verification

```bash
mint broken-links          # whole site; must be clean
```

Also compile changed pages as MDX before pushing — a Mintlify build failure is slower to diagnose than a local parse error.

## What NOT to Do

- NEVER add XML comments to .drawio files
- NEVER trust MCP draw.io tools for batch vertex insertion — write XML directly
- NEVER use non-`.drawio.svg` naming for draw.io SVG exports
- NEVER edit docs.json navigation without understanding the tab/group/languages structure
- NEVER create new MDX pages without adding them to `docs.json` navigation (both EN and NL if applicable)
- NEVER repeat data grid customisation instructions on individual feature pages — link to the Using Clarus section instead
- NEVER translate system/UI terms (field names, location types like GIBAY/GOBAY) in Dutch articles
- NEVER add `tags` to a changelog `<Update>` — it replaces the table of contents with filters
- NEVER name a customer, disclose a security finding, or expose tenant, capacity or commercial mechanics in the changelog
- NEVER describe an HHD/handheld change as automatic — those flows are configured, so the change makes something possible to configure
- NEVER write performance work as a before-and-after that implies the product was previously broken
- NEVER assume a completed ticket is customer-facing — internal-only features (e.g. task targets) must stay out of the changelog
