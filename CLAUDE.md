# RGI Landing Page — Project Context

## What this is

A landing page for the **Randolph Glacier Inventory (RGI)**, replacing the old page at https://www.glims.org/RGI/. The new site will be live at **https://rgidata.org**, hosted on GitHub Pages from this repo (`glims-rgi.github.io`).

## Stack

- **MkDocs 1.6.1** + **Material for MkDocs 9.7.6**
- Python 3.14, conda env named `mkdocs`
- Run dev server: `source activate mkdocs && mkdocs serve`
- Build: `source activate mkdocs && mkdocs build`

## File structure

```
mkdocs.yml                        # Main config
docs/
  index.md                        # Home page — RGI description + RGI 7.0 + citation
  versions.md                     # Version history table
  about.md                        # About the dataset and IACS Working Group
  privacy.md                      # Privacy notice (footer link only, not in navbar)
  stylesheets/extra.css           # All custom CSS
  img/rgi_logo/                   # Logo files (blue, white, black, SVG, favicon)
overrides/
  main.html                       # Extends base.html — injects Plausible scripts in <head>
  partials/header.html            # Custom header — puts nav links inline with title
```

## Key design decisions

**Navigation:** `Home · Versions · About` in the header bar, inline with the site title (not below it). Implemented via `overrides/partials/header.html` — Material's `navigation.tabs` was NOT used. Privacy is in the footer only (not the navbar).

**Why custom header template:** Material has no built-in way to put nav links inline with the title in the same row. The override iterates `nav` directly. Note: the `{% if nav_item.url %}` check was intentionally removed — MkDocs gives index.md an empty string URL which is falsy in Jinja2, causing Home to be hidden.

**White header:** `primary: white` in palette. Logo uses `rgi_logo_blue.png` (white logo would be invisible). Nav link colors are dark grey, not white.

**No search:** `plugins: []` disables it entirely.

**Sidebars:** Both hidden via CSS. Left sidebar: `display: none`. Right TOC: `display: none !important` — the `!important` is necessary because Material's JS sets an inline `style="display: block"` on it at runtime, overriding plain CSS rules.

**Content width:** `.md-grid { max-width: 50rem }` — Material 9 hardcodes `61rem`, there is NO CSS variable for it (`:root { --md-grid-max-width }` does nothing).

**Instant navigation:** `navigation.instant` feature enabled — prevents full-page reload flicker when clicking nav links.

**Footer:** Copyright line with CC BY 4.0 and Privacy link. Privacy link uses `/privacy/` (root-relative) — a relative `privacy/` would resolve incorrectly from sub-pages like `/about/`.

**Favicon:** `docs/img/rgi_logo/favicon.png` — set via `favicon:` key in theme config.

**User guide HTML in `docs/user_guide/`:** The `docs/user_guide/` tree contains pre-built Jupyter Book HTML — this is intentional and correct. It is produced by the sister repo `glims-rgi/rgi_user_guide` whose CI (`deploy-production.yml`) opens a PR here whenever a new user guide release is ready. MkDocs copies all non-Markdown files in `docs/` verbatim, so the Jupyter Book HTML is served as-is at `rgidata.org/user_guide/` with its own Jupyter Book sidebar navigation. It does not appear in the MkDocs nav and requires no changes to `mkdocs.yml`. Do not delete or edit these files — they are not build artifacts of this repo. **If something looks wrong in the user guide content, layout, or CSS, the fix belongs in `glims-rgi/rgi_user_guide`, not here.** Changes made directly in `docs/user_guide/` will be overwritten on the next production deploy PR.

Current structure:

- `docs/user_guide/` — v7.0 HTML (→ `rgidata.org/user_guide/`)
- `docs/user_guide/v7.0/` — permanent archive copy (→ `rgidata.org/user_guide/v7.0/`)

**Analytics:** Plausible (self-hosted at `plausible.oggm.org`). The domain is embedded server-side in the script URL (`pa-PEdNqFQ0VRJj8a0B4ZsJI.js`), so no `data-domain` attribute is needed. Two scripts are required — the `async` CDN script plus an inline init stub that enables optional measurements (outbound links, file downloads, form submissions). Because `extra_javascript` in MkDocs cannot produce inline `<script>` blocks, both scripts live in `overrides/main.html` via `{% block extrahead %}`, which injects them into `<head>`. Do NOT move them to `extra_javascript`.

## Zensical note

The Material for MkDocs team is building a successor called [Zensical](https://github.com/zensical/zensical) (Rust+Python, v0.0.43 as of May 2026). Worth revisiting at v1.0, but not yet stable enough for production use. Migration would be easy given the simple content structure.
