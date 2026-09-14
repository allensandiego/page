# Page Workspace Rules

## Automated Git Commit and Push
- Whenever changes are made to blog posts, drafts, layouts, templates, styles, or assets in this repository (`page`), **always automatically stage, commit with a concise conventional commit message, and push directly to `origin/main`**.
- This ensures GitHub Actions (`pages-deploy.yml`) is immediately triggered to build and publish the live GitHub Pages site without requiring manual prompting.
