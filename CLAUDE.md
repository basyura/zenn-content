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

## Git Commit Rules

コミットログの1行目は以下の形式に従う：
```
update : [記事のタイトル]
```

- タイトルは記事の frontmatter の `title:` にある文字列を使用する
- 例: `update : ChatGPTで Vim ライクなキーバインドを使う`


## 日本語文章の口調ルール

blog.basyura.org にあるブログ記事の口調･論調に合わせること。具体的な特徴：

### 基本的な口調・論調
- 冷静で控えめ、事実を淡々と述べる
- 感情的な表現を抑制し、客観的に語る
- 会話調だが節度を保った文体
- 簡潔で直接的な表現を好む

### 文章の特徴
- 常体（〜だ、〜する）を基本とする
- 短文〜中文（30-60文字程度）
- 段落は2-5行程度の短さ
- 複雑な修辞や華美な表現を避ける

### 内容の語り方
- 個人的な体験を客観視して語る
- 思考の流れに沿った自然な展開
- 個人的な観察や内省を含める
- 不確実性や脆弱性を素直に表現

### 感情表現
- 感情は控えめに表現
- 体験の記述に重点を置く
- 「まあいいか」「静かな満足感」など穏やかな表現
- 誇張表現は使わない

### 技術的内容
- 実用的で経験に基づいた説明
- 個人日記のような親しみやすさ
- 論文調や説明書調を避ける

### 自然な表現への変更例
- 「これらを解決するため」→「これを何とかしたくて」
- 「実装内容」→「やったこと」
- 「ついでに、〜が分かりづらいのと」→「あと、〜が分かりにくいし」
- 「置いている」→「置いた」（過去形で自然に）

### 構成のポイント
- 短い段落（2-5行）に分割する
- 時系列や思考の流れに沿った自然な構成
- フラットで淡々とした語り口
- 個人的な感想や経験を中心とした内容

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

