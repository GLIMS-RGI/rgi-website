# RGI Landing Page

Source for **[rgidata.org](https://rgidata.org)** — the landing page for the
[Randolph Glacier Inventory (RGI)](https://www.glims.org/RGI/), a global dataset
of glacier outlines produced by the IACS Working Group on the Randolph Glacier Inventory.

Built with [MkDocs](https://www.mkdocs.org/) + [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).
Hosted on GitHub Pages from this repository.

## Setup

```bash
conda create -n mkdocs python=3.12
conda activate mkdocs
pip install mkdocs-material
```

## Development

```bash
conda activate mkdocs
mkdocs serve        # live-reload dev server at http://127.0.0.1:8000
mkdocs build        # build static site into site/
```

## Structure

```text
mkdocs.yml            # site configuration
docs/
  index.md            # home page
  versions.md         # version history
  about.md            # about the dataset and working group
  privacy.md          # privacy notice
  stylesheets/        # custom CSS
  img/                # logo and favicon
overrides/
  main.html           # injects analytics into <head>
  partials/header.html  # custom header with inline nav links
```
