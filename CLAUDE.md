# CLAUDE.md

## ビルド

`/build` スキルを使用。または直接:

```bash
# HTML(プレビュー用)
npx @marp-team/marp-cli@latest slides/[file].md --html --allow-local-files -o slides/[file].html --no-stdin

# PDF(配布用)
npx @marp-team/marp-cli@latest slides/[file].md --pdf --allow-local-files -o slides/[file].pdf --no-stdin
```

## 機能一覧

| 種類 | 名前 | 用途 |
|-----|------|------|
| skill | `/build` | スライドビルド |
| command | `/review-slide` | 総合レビュー |
| command | `/check-structure` | 構成チェック |
| command | `/fact-check` | ファクトチェック |
| rule | slide-writing | スライド作成ルール(自動適用) |
