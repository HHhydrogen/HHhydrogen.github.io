# HHhydrogen.github.io

This repository hosts my GitHub Pages site powered by MkDocs.

The default site language and navigation are configured in Chinese.

## Quick start

1. Create and activate a virtual environment.
2. Install dependencies:

	pip install -r requirements.txt

3. Run local preview:

	mkdocs serve

4. Build static files:

	mkdocs build --strict

The generated site is written to the site folder.

## GitHub Pages deployment

This repository includes a workflow at .github/workflows/deploy.yml.

On push to main, it will:
- Build the site with MkDocs
- Upload the generated site artifact
- Deploy to GitHub Pages

In repository settings, ensure Pages source is set to GitHub Actions.
