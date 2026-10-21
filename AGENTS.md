# Repository Guidelines

## Project Structure & Module Organization

This repository is for writing and publishing Japanese technical articles on Zenn. Use `zenn-cli` to create and preview content locally.

- `articles/`: Markdown articles with YAML frontmatter.
- `books/`: Book content; currently contains only a placeholder.
- `images/`: Article images, such as `sidetoc.png` and `vtab.png`.
- `package.json` and `package-lock.json`: Zenn CLI dependency and npm commands.
- `CLAUDE.md`: Existing writing and commit conventions.

There are no application source modules or automated test directories.

## Build, Test, and Development Commands

- `npm ci`: Install dependencies from the committed lockfile.
- `npm run preview`: Start the local Zenn preview server at `http://localhost:8000`; `npx zenn preview` is equivalent.
- `npx zenn new:article --slug article-slug --title "記事タイトル" --type tech --emoji ✨`: Generate an article.
- `npx zenn new:book`: Generate a book scaffold.

No build, lint, or formatting scripts are configured. `npm test` is a placeholder that exits with an error.

## Writing & Publishing Workflow

Create an article, edit its frontmatter and body, then preview it locally. Keep `published: false` while drafting. When ready to publish, set `published: true`, commit, and push to the branch connected to Zenn. Check deployment status in the Zenn dashboard. Preserve the slug when updating an existing article.

See the [usage guide](https://zenn.dev/ai4u_shunsuke/articles/zenn-cli-usage) and [official CLI documentation](https://zenn.dev/zenn/articles/zenn-cli-guide).

## Coding Style & Naming Conventions

Use Markdown with fenced, language-tagged code blocks. Preserve surrounding formatting and use two-space indentation in JSON configuration.

Choose unique article slugs, such as `inkdrop-003-vim-tips.md`.

Include `title`, `emoji`, `type`, `topics`, and `published` in article frontmatter. Use `tech` or `idea` for `type`; keep drafts at `published: false`.

Write Japanese prose in a calm, direct, plain style, with short paragraphs and restrained emphasis. Separate English words and Japanese text with half-width spaces. Follow `CLAUDE.md` for detailed prose conventions.

## Testing Guidelines

No test framework or coverage threshold is configured. Preview changed content locally and inspect headings, code blocks, links, images, and frontmatter. Verify referenced image paths and review the publication flag before committing.

## Commit & Pull Request Guidelines

Recent history includes `add :`, `update :`, and `publish :` prefixes. The documented article commit convention is `update : [記事のタイトル]`, using the exact frontmatter title.

Keep changes focused. Pull request descriptions should identify affected articles, summarize changes, and report preview checks. Link relevant issues when available; include screenshots when they clarify rendering changes. Check snippets and images for credentials or private information before submission.
