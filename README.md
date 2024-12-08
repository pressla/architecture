# Documentation Project with Material for MkDocs

This repository contains documentation built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

## Local Development Setup

1. Create and activate a Python virtual environment:

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/MacOS
python -m venv venv
source venv/bin/activate
```

2. Install Material for MkDocs:

```bash
pip install mkdocs-material
pip install mkdocs_puml
```

3. Serve the documentation locally:

```bash
mkdocs serve
```

The documentation will be available at `http://127.0.0.1:8000`

## GitHub Pages Deployment

This repository is configured to automatically deploy the documentation to GitHub Pages. The deployment happens through a GitHub Actions workflow that:

1. Builds the documentation
2. Pushes the built site to the `gh-pages` branch
3. Makes it available through GitHub Pages

The documentation is automatically updated on every push to the main branch. You can view the live documentation at:

`https://[username].github.io/[repository-name]`

Replace `[username]` with your GitHub username and `[repository-name]` with this repository name.

## Project Structure

- `docs/`: Contains the documentation source files in Markdown format
- `mkdocs.yml`: Configuration file for MkDocs
- `.github/workflows/`: Contains the GitHub Actions workflow for automated deployment

## Building Documentation

To build the documentation locally:

```bash
mkdocs build
```

This will create a `site` directory with the built HTML documentation.
