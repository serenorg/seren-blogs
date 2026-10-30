# Repository Guidelines

## Scope

- This repository owns the Markdown source of the blog posts published on the SerenAI website, and the images those posts reference. It contains no application code, build, or deployment.
- The website that renders and serves these posts, and the Seren products the posts describe, live in other repositories. Change product code, site templates, or deploy configuration there, not here.
- Read `README.md` and `CONTRIBUTING.md` before writing or editing a post. They hold the naming convention, post structure, and content standards.

## Structure

- Posts are under `blogs/`, one Markdown file per post, named `YYYYMMDD_Title-With-Hyphens.md`. Some older posts use underscores or spaces. Do not rename, move, or delete existing posts or images without explicit instruction.
- Images are under `blogs/images/`. Posts reference them by relative path, such as `images/<name>.jpg`, in Markdown image links or in an `og_image` front matter field.
- Most posts start with an optional YAML front matter block, then a `#` title. Follow the structure in `CONTRIBUTING.md` for new posts, including the closing published date and tags lines.
- `README.md`, `CONTRIBUTING.md`, and `LICENSE` are the only other files. There are no tests, generated files, or deploy configuration.

## Validation

- This repository has no CI workflows, git hooks, Makefile, or package manifests, so there is no build, lint, or test command.
- Run `git diff --check` before committing to catch trailing whitespace and conflict markers in the lines you changed.
- Check by reading the rendered Markdown that headings, fenced code blocks, links, and image paths are well formed, and that every image a post references exists under `blogs/images/`.
- Keep ASCII diagrams inside fenced code blocks and at most 80 characters wide, as `CONTRIBUTING.md` requires.
- Report exactly which checks ran and which could not.

## Security and Operations

- This repository is public. Everything committed is published, including history. Never commit unpublished drafts that are not meant to be public, private correspondence, customer or user data, or internal-only details.
- Never commit credentials, API keys, tokens, wallet seeds or private keys, passwords, or real account identifiers, including in code examples. Use clearly fake placeholder values.
- Treat anything on `main` as published. Make content changes only when asked, and keep an author's hand-written prose as written unless asked to edit it.

## Changes and Commits

- Keep changes scoped to the posts, images, or documents the task names and preserve unrelated work. Do not add tooling, dependencies, or configuration without a clear request.
- Write Markdown prose as soft-wrapped paragraphs, and use ASCII by default in new documentation.
- Use Conventional Commits with a concise single-line subject, such as `blog: add <title>`. Do not include issue numbers, ticket URLs, personal signatures, co-author trailers, generated-by notices, or tool attribution.
- Do not record issue or pull request numbers, URLs, or commit hashes as history in posts or documents. Fold review corrections into unpublished commits and preserve authorship when integrating contributions.
- Never bypass hooks with `git commit --no-verify`.
- Do not push, delete, or replace remote refs without explicit authorization. Use `--force-with-lease` for an authorized rewrite and verify the remote afterward.
