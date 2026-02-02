# Marp Company Template

社内用Marpプレゼンテーションテンプレート - Phase 1(14パターン実装)

## 概要

このテンプレートは、顧客向けプレゼンテーションを中心としたMarpスライド作成用のテンプレートです。明示的なパターン指定とシンプルな保守性を重視した設計になっています。

### 特徴

- **14パターン実装**: タイトル・セクション系5パターン、レイアウト系7パターン、強調系2パターン
- **自動ブランド統合**: ロゴとページ番号の自動挿入
- **統一されたカラーパレット**: プライマリー、セカンダリー、アクセントの3色
- **制限付きHTML許容**: `<div>`, `<span>`, 一部インラインスタイルをサポート
- **シンプルな構成**: テンプレートとCSSのみで保守性を確保

## セットアップ

### 必要な環境

- Node.js(v18以上推奨)
- pnpm

### インストール

```bash
# 依存関係のインストール
pnpm install
```

## 使い方

### 1. 新しいスライドの作成

`slides/` ディレクトリ配下に新しいディレクトリを作成します。

```bash
# 例: 2025年のクライアントプレゼンテーション
mkdir -p slides/2025-client-presentation
```

### 2. スライドファイルの作成

作成したディレクトリ内に `slide.md` を作成します(単数形)。

```bash
# テンプレートをコピー
cp slides/template.md slides/2025-client-presentation/slide.md
```

### 3. 画像の配置

スライド専用の画像は、同じディレクトリ内の `images/` サブディレクトリに配置します。

```bash
mkdir slides/2025-client-presentation/images
```

画像パスは相対パスで記述します。

```markdown
![図](./images/diagram.png)
```

### 4. スライドの編集

Markdownファイルを編集し、パターンを指定します。

```markdown
---
marp: true
theme: company
paginate: true
---

<!-- _class: title -->
![bg](../assets/images/background.png)

# プレゼンテーションタイトル
## サブタイトル

### 発表者名 | 2025-02-02

---

## 通常のスライド

- 箇条書き1
- 箇条書き2
```

## ビルド方法

### HTMLプレビュー

```bash
# 開発サーバーの起動
pnpm start

# ブラウザで http://localhost:8080 にアクセス
```

### 個別ビルド

特定のスライドをHTMLまたはPDFに変換します。

```bash
# HTML出力
npx @marp-team/marp-cli@latest slides/2025-client-presentation/slide.md --html --allow-local-files -o slides/2025-client-presentation/slide.html --no-stdin

# PDF出力
npx @marp-team/marp-cli@latest slides/2025-client-presentation/slide.md --pdf --allow-local-files -o slides/2025-client-presentation/slide.pdf --no-stdin
```

または `/build` スキルを使用してビルドすることも可能です。

## ディレクトリ構造

```
slide-template/
├── themes/
│   └── company.css              # メインテーマCSS
├── slides/
│   ├── template.md              # 空テンプレート(参考用)
│   ├── example.md               # 全パターンのサンプル(参考用)
│   ├── 2025-client-presentation/
│   │   ├── slide.md            # メインスライド
│   │   └── images/             # このスライド専用の画像
│   └── ...
├── assets/
│   ├── images/
│   │   ├── logo.png            # 会社ロゴ(自動挿入用)
│   │   └── background.png       # タイトル背景画像
│   └── .gitkeep
├── docs/
│   └── patterns.md              # パターン一覧ドキュメント
├── .marprc.yml                  # Marp設定
├── package.json                 # 依存関係とスクリプト
├── .gitignore
└── README.md                    # このファイル
```

## パターン一覧

### A. タイトル・セクション系(5パターン)

| パターン | クラス名 | 用途 |
|---------|---------|------|
| タイトルスライド | `title` | プレゼンテーションの表紙 |
| セクション区切り | `section` | 章の開始や大きな区切り |
| アジェンダ・目次 | `agenda` | プレゼンテーションの構成提示 |
| クロージング | `closing` | プレゼンテーションの終了 |
| まとめスライド | `summary` | 重要ポイントのまとめ |

### B. レイアウト系(7パターン)

| パターン | クラス名 | 用途 |
|---------|---------|------|
| 2カラム基本 | `content-2col` | 2つの項目を横並びで比較 |
| Before/After比較 | `content-2col-comparison` | 改善前後の対比表示 |
| 3カラム | `content-3col` | 3つの項目を並列表示 |
| 画像右配置 | `content-image-right` | テキスト+画像の組み合わせ |
| 中央配置メッセージ | `content-center` | 重要メッセージの強調 |
| 2x2グリッド | `content-grid-2x2` | 4つの項目を均等表示 |
| 縦3つステップ | `content-steps` | 段階的なプロセス説明 |
| リスト+補足パネル | `content-list-panel` | メインと補足情報の表示 |

### C. 強調・特殊系(2パターン)

| パターン | クラス名 | 用途 |
|---------|---------|------|
| 強調ボックス | `box-light`, `box-medium`, `box-strong` | 3段階の強調表示 |
| 統計・数値表示 | `stats` | 数値データの視覚的表示 |

詳細は [docs/patterns.md](docs/patterns.md) を参照してください。

## カラーパレット

| カラー | コード | 用途 |
|-------|--------|------|
| プライマリー | `#1B4565` | 濃紺(見出し、重要要素) |
| セカンダリー | `#3E9BA4` | ティール(サブ見出し) |
| アクセント | `#1ab394` | ティール系の緑(強調) |
| テキスト(濃) | `#333333` | 本文 |
| テキスト(明) | `#ffffff` | 白背景時のテキスト |
| テキスト(控えめ) | `#6b7280` | ページ番号など |

## 自動挿入要素

### ロゴ

全スライド(タイトルとクロージング除く)の左下に自動的に会社ロゴが表示されます。

- **画像パス**: `assets/images/logo.png`
- **サイズ**: 50px × 50px
- **位置**: 左下(left: 30px, bottom: 30px)

**ロゴを非表示にする:**
```markdown
<!-- _class: no-logo -->
```

### ページ番号

全スライド(タイトルとクロージング除く)の右下にページ番号が自動表示されます。

- **形式**: `現在のページ / 総ページ数`
- **位置**: 右下(right: 40px, bottom: 30px)

## カスタマイズ

### カラーの変更

`themes/company.css` の `:root` セクションでカラー変数を変更できます。

```css
:root {
  --color-primary: #1B4565;
  --color-secondary: #3E9BA4;
  --color-accent: #1ab394;
}
```

### ロゴの差し替え

`assets/images/logo.png` を自社ロゴに差し替えてください。推奨サイズは50px × 50px です。

### 背景画像の差し替え

`assets/images/background.png` をタイトル背景用の画像に差し替えてください。

## HTMLとMarkdownの使い分け

### Markdownで記述できるもの

- 見出し(`#`, `##`, `###`)
- 箇条書き(`-`, `*`, `1.`)
- 画像(`![alt](path)`)
- コードブロック(` ``` `)
- 太字・斜体(`**bold**`, `*italic*`)

### HTMLを使う必要があるもの

- 複数カラムレイアウト(`<div>`でグリッド構成)
- 強調ボックス(`<div class="box-light">`)
- テキスト色変更(`<span class="highlight-primary">`)
- 統計表示(`<div class="stats-container">`)

### インラインスタイル許容範囲

以下のプロパティは最小限の使用であれば許容されます。

- `color` - テキスト色(ただし変数推奨)
- `font-weight` - 太字強調
- `font-size` - サイズ調整(特殊ケースのみ)
- `margin`, `padding` - 微調整(多用厳禁)

## トラブルシューティング

### ロゴが表示されない

- パスが正しいか確認(相対パス: `../assets/images/logo.png`)
- ファイルが存在するか確認
- `.marprc.yml` で `allowLocalFiles: true` が設定されているか確認

### カラムレイアウトが崩れる

- `<div>` タグが正しく閉じられているか確認
- HTMLとMarkdownの混在部分の構文を確認

### PDFエクスポートで画像が表示されない

- `--allow-local-files` オプションが指定されているか確認
- 画像パスが相対パスで正しく指定されているか確認

```bash
# 正しいコマンド例
npx @marp-team/marp-cli@latest slides/example.md --pdf --allow-local-files -o output.pdf --no-stdin
```

## 開発ワークフロー

1. `slides/` 配下に新しいディレクトリを作成
2. `slide.md` を作成(`template.md` をコピー)
3. 必要に応じて `images/` ディレクトリを作成
4. スライドを編集
5. プレビュー: `pnpm start`
6. PDF出力: ビルドコマンドまたは `/build` スキル

## 保守性のポイント

- **CSSクラス名は意味が分かりやすい名前を使用**
- **インラインスタイルは最小限に抑える**
- **パターン追加時は `example.md` と `patterns.md` を同時更新**
- **相対パスを使用して移植性を確保**

## ライセンス

社内用テンプレート

## 参考資料

- [Marp公式ドキュメント](https://marpit.marp.app/)
- [Marp CLI](https://github.com/marp-team/marp-cli)
- `slides/example.md` - 全パターンの実例
- `docs/patterns.md` - パターン詳細ドキュメント

## フェーズ2について

現在はPhase 1(14パターン)を実装済みです。将来的にPhase 2として残り24パターンを追加し、合計38パターンまで拡張する予定です。