# Marp Company Template

社内用Marpプレゼンテーションテンプレート - 14パターン実装済み。pnpmを使用。

プロジェクトのログ・コメント・GitHubのコミットメッセージ等は日本語で記載する。

## プロジェクト構造

- `themes/` - メインテーマCSS (`company.css`)
- `slides/` - スライドファイル格納ディレクトリ
  - `template.md` - 空テンプレート(参考用)
  - `example.md` - 全パターンのサンプル(参考用)
  - `[プロジェクト名]/` - 各プレゼンテーション用ディレクトリ
    - `slide.md` - メインスライドファイル(単数形)
    - `images/` - スライド専用の画像格納
- `assets/` - 共通リソース
  - `images/logo.png` - 会社ロゴ(自動挿入用)
  - `images/background.png` - タイトル背景画像
- `docs/` - パターン一覧ドキュメント

## 主要コマンド

```bash
# 開発サーバー起動(プレビュー)
pnpm start

# HTML出力
npx @marp-team/marp-cli@latest slides/[file].md --html --allow-local-files -o slides/[file].html --no-stdin

# PDF出力
npx @marp-team/marp-cli@latest slides/[file].md --pdf --allow-local-files -o slides/[file].pdf --no-stdin
```

または `/build` スキルを使用してビルド可能。

## 実装パターン

### A. タイトル・セクション系(5パターン)

- `title` - プレゼンテーションの表紙
- `section` - 章の開始や大きな区切り
- `agenda` - プレゼンテーションの構成提示
- `closing` - プレゼンテーションの終了
- `summary` - 重要ポイントのまとめ

### B. レイアウト系(7パターン)

- `content-2col` - 2つの項目を横並びで比較
- `content-2col-comparison` - Before/After比較
- `content-3col` - 3つの項目を並列表示
- `content-image-right` - テキスト+画像の組み合わせ
- `content-center` - 重要メッセージの強調
- `content-grid-2x2` - 4つの項目を均等表示
- `content-steps` - 段階的なプロセス説明
- `content-list-panel` - メインと補足情報の表示

### C. 強調・特殊系(2パターン)

- `box-light`, `box-medium`, `box-strong` - 3段階の強調表示
- `stats` - 数値データの視覚的表示

## カラーパレット

- `--color-primary: #1B4565` - 濃紺(見出し、重要要素)
- `--color-secondary: #3E9BA4` - ティール(サブ見出し)
- `--color-accent: #1ab394` - ティール系の緑(強調)

## 自動挿入要素

- **ロゴ**: `company` テーマのみ、全スライド(タイトルとクロージング除く)の左下に自動挿入
  - 非表示: `<!-- _class: no-logo -->`
  - `starbucks` テーマはロゴ・ワードマークを挿入しない
- **ページ番号**: 右下に自動表示(`現在のページ / 総ページ数`)

## 開発ワークフロー

1. `slides/` 配下に新しいディレクトリを作成
2. `slide.md` を作成(`template.md` をコピー)
3. 必要に応じて `images/` ディレクトリを作成
4. スライドを編集
5. プレビュー: `pnpm start`
6. PDF出力: ビルドコマンドまたは `/build` スキル

## 保守性のポイント

- CSSクラス名は意味が分かりやすい名前を使用
- インラインスタイルは最小限に抑える
- パターン追加時は `example.md` と `patterns.md` を同時更新
- 相対パスを使用して移植性を確保

## その他

- 画像パスは相対パスで記述(`./images/diagram.png`)
- HTMLとMarkdownを適切に使い分け
- Phase 2で24パターン追加予定(合計38パターン)
