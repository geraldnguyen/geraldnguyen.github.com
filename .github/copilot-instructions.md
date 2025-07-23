# Copilot Instructions for GeraldNguyen-v3

## Project Overview
- This is a Hugo static site project for https://geraldnguyen.github.io/
- The main site content is in `content/`, with posts organized by year and pages under `content/pages/`.
- The theme in use is `themes/medium`, which contains its own layouts and partials.
- Site configuration is in `hugo.toml`.

## Architecture & Patterns
- **Content:** Markdown files in `content/` (posts, pages, etc.).
- **Layouts:** Hugo templates in `layouts/` and `themes/medium/layouts/`.
- **Assets:** Images and static files in `assets/` and `static/`.
- **Public:** Generated site output in `public/` after running `hugo`.
- **Data:** Custom data files can be placed in `data/`.
- **Taxonomies:** Tags and categories are used; tag lists are rendered via partials (e.g., `{{ partial "terms.html" (dict "taxonomy" "tags" "page" .) }}` in layouts).

## Developer Workflows
- **Build:** Run `hugo` to generate the site into `public/`.
- **Local Preview:** Run `hugo server` to preview locally at `http://localhost:1313`.
- **Deploy:** GitHub Actions workflow `.github/workflows/deploy-github.yml` builds and deploys to GitHub Pages using `peaceiris/actions-gh-pages@v3`.
- **Theme Development:** Edit or add templates in `themes/medium/layouts/`.

## Project-Specific Conventions
- Use partials for reusable template logic (e.g., tag lists).
- Posts are organized by year in subfolders under `content/posts/`.
- About page uses a custom layout: `themes/medium/layouts/pages/about.html`.
- Images for the profile are in both `assets/` and `public/`.

## Key Files & Directories
- `hugo.toml`: Site configuration.
- `content/`: Main content (posts, pages).
- `themes/medium/layouts/`: Theme templates and partials.
- `.github/workflows/deploy-github.yml`: CI/CD for GitHub Pages.

## Examples
- To display all tags on a page: `{{ partial "terms.html" (dict "taxonomy" "tags" "page" .) }}`
- To add a new post: create a markdown file in `content/posts/<year>/`.

---
If you add new conventions or workflows, update this file to help future AI agents and developers.
