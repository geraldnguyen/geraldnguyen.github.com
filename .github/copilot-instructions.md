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


## Post Content Type

### File Path Convention
- Posts are stored in subfolders by year: `content/posts/<year>/<slug>/index.md`.

### Typical Front Matters
- Use YAML or TOML front matter at the top of each post.
- Required fields:
  - `categories`: List of categories (at least one required)
  - `tags`: List of tags (at least one required)
- Optional fields:
  - `title`, `subtitle`, `date`, `image`, etc.
  - `subtitle`: A short subheading or summary for the post, displayed below the main title in listings and on the post page.

  - `medium`: Add this field if the article is imported from Medium, with the original Medium URL as the value.
  - When importing from Medium, set the `date` field to the original publication date.

#### Example front matter:
```yaml
---
title: Example Post
subtitle: Example Subtitle
date: 2025-07-23T12:00:00+01:00
categories: [General]
tags: [Example, Hugo]
medium: https://medium.com/example-url
---
```

### Images
- Use the Hugo `figure` shortcode for images instead of Markdown image syntax.
- Example:
  `{{< figure src="/path/to/image.jpg" caption="Image caption" >}}`

---
## Examples
- To display all tags on a page: `{{ partial "terms.html" (dict "taxonomy" "tags" "page" .) }}`
- To add a new post: create a markdown file in `content/posts/<year>/`.
- Social sharing buttons are automatically added to the top and bottom of each article via the `social-sharing.html` partial.

## Social Sharing Feature
- Social sharing buttons are automatically displayed at the top and bottom of each article.
- Supports: Twitter/X, LinkedIn, Facebook, Reddit, Email, and Copy Link functionality.
- Styled to match the theme design with hover effects and responsive layout.
- Uses site parameters from `hugo.toml` for Twitter handle integration.
- Located in `themes/medium/layouts/partials/social-sharing.html` with CSS in `themes/medium/assets/css/main.css`.

---
If you add new conventions or workflows, update this file to help future AI agents and developers.
