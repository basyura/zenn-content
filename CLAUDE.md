# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Zenn content repository containing technical articles written in Japanese. Zenn is a Japanese technical publishing platform similar to Medium or Dev.to.

## Common Commands

- **Preview articles locally**: `npm run preview` or `npx zenn preview`
- **Create new article**: `npx zenn new:article --slug article-slug --title "記事タイトル" --type tech --emoji ✨`
- **Create new book**: `npx zenn new:book`

## Architecture and Structure

- `articles/`: Individual articles in Markdown format with frontmatter
  - File naming: Uses unique slug-based naming (e.g., `447d1df9a4db35.md`)
  - Frontmatter includes: title, emoji, type, topics, published status
- `books/`: Book-format content (collections of chapters)
- `images/`: Static image assets referenced by articles
- Content is written primarily in Japanese

## Article Format

Each article follows Zenn's format:
```yaml
---
title: "Article title in Japanese"
emoji: "📝" 
type: "tech" # or "idea"
topics: ["tag1", "tag2"]
published: false # true when ready to publish
---
```

## Development Notes

- Uses `zenn-cli` for local development and content management
- Articles can be previewed locally before publishing
- Images should be placed in the `images/` directory and referenced relatively
- Published articles sync to zenn.dev/basyura


## 日本語文章の口調ルール

- blog.basyura.org にあるブログ記事の口調･論調に合わせること
- 誇張表現は使わないこと

## 日本語文書の半角スペースルール

日本語文書内で半角英数字の単語を使用する際は、以下のルールに従って半角スペースを挿入する：

### 基本ルール
- 半角英数字の単語の前後に半角スペースを挿入する
- 引用符（"", '', ``）内の文字列には半角スペースを挿入しない
- 句読点（、。）の後には半角スペースを挿入しない

### 例
✅ 正しい例：
- `Web 版の ChatGPT を Vivaldi で使用している。`
- `Enter じゃなくて Ctrl+Enter で送信したい。`
- `Vim ライクなキーバインドを実装。`

❌ 間違った例：
- `Web版のChatGPTをVivaldiで使用している。`
- `Enter じゃなくて Ctrl+Enter で送信したい。` （句読点の後にスペース）
- `"ChatGPT で Vim ライク"` （引用符内にスペース）

