# Project Guidelines

## Build and Test
- Use `hugo server -D` for local development and draft preview.
- Use `hugo` to build the site into `public/`.
- Do not edit generated output in `public/` or generated artifacts under `resources/_gen/` unless the task is explicitly about generated files.

## Architecture
- This repository is a Hugo site configured by `hugo.toml` and backed by the vendored theme in `themes/hugo-theme-stack-master/`.
- Treat `content/` as the source of truth for pages and posts. Use `content/post/` for blog posts and `content/page/` for standalone pages such as About, Archives, and Search.
- Prefer root-level overrides in `layouts/`, `assets/`, and `static/` before changing files under `themes/hugo-theme-stack-master/`.

## Conventions
- Keep post front matter consistent with `archetypes/default.md`: YAML front matter with `title`, `date`, `draft`, `categories`, and `tags`.
- Follow existing content examples for metadata style and bilingual content tone, especially `content/post/hello_my_blog.md` and posts under `content/post/*/`.
- Preserve the current site configuration in `hugo.toml`, including Chinese as the default language, the GitHub Pages `baseURL`, and site-wide article math rendering.
- When adding or updating content, prefer Hugo content changes over editing theme templates unless the request is specifically about site structure or presentation.

## References
- For theme capabilities and configuration details, see `themes/hugo-theme-stack-master/README.md`.
- For the repo's existing setup notes and Hugo usage examples, see `content/post/hello_my_blog.md`.