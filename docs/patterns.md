# パターン一覧ドキュメント

## 概要

このドキュメントでは、Marp Company Themeで使用可能な14パターンの詳細を説明します。

## 使用方法

各パターンは、Markdownファイル内で `<!-- _class: パターン名 -->` として指定します。

```markdown
<!-- _class: title -->

# タイトル
```

---

## A. タイトル・セクション系（5パターン）

### 1. title - タイトルスライド

**クラス名:** `title`

**用途:** プレゼンテーションの表紙

**特徴:**
- グラデーション背景（プライマリー → セカンダリー）
- 大きなタイトル表示
- 背景画像の指定が可能

**使用例:**
```markdown
<!-- _class: title -->
![bg](../assets/images/background.png)

# プレゼンテーション
## タイトル

### サブタイトル
発表者名 | 2025-02-02
```

---

### 2. section - セクション区切り

**クラス名:** `section`

**用途:** 章の開始や大きな区切り

**特徴:**
- プライマリーカラーの背景
- 中央配置
- 大きなテキスト表示

**使用例:**
```markdown
<!-- _class: section -->

# 第1章
## 背景と課題
```

---

### 3. agenda - アジェンダ・目次

**クラス名:** `agenda`

**用途:** プレゼンテーションの目次や構成の提示

**特徴:**
- 下線付き見出し
- 番号付きリストに最適化
- 読みやすい行間

**使用例:**
```markdown
<!-- _class: agenda -->

## アジェンダ

1. 背景と課題
2. ソリューション提案
3. 実装計画
4. まとめ
```

---

### 4. closing - クロージング

**クラス名:** `closing`

**用途:** プレゼンテーションの終了スライド

**特徴:**
- タイトルスライドと同じグラデーション背景
- 中央配置
- 連絡先情報の表示に最適

**使用例:**
```markdown
<!-- _class: closing -->

# ありがとうございました

ご質問・ご相談はお気軽に
company@example.com
```

---

### 5. summary - まとめスライド（ガラス風縦並び）

**クラス名:** `summary`

**用途:** 重要なポイントのまとめ

**特徴:**
- ガラスモーフィズム風のデザイン
- 縦並びレイアウト
- 複数のポイントを構造的に整理

**使用例:**
```markdown
<!-- _class: summary -->

## まとめ

<div class="summary-items">

<div class="summary-item">

### ポイント1
説明文

</div>

<div class="summary-item">

### ポイント2
説明文

</div>

</div>
```

---

## B. レイアウト系（7パターン）

### 6. content-2col - 2カラム基本

**クラス名:** `content-2col`

**用途:** 2つの項目を横並びで比較・表示

**特徴:**
- 均等な幅配分（1:1）
- テキストベースの比較に最適
- 見出しは全幅表示

**使用例:**
```markdown
<!-- _class: content-2col -->

## 2カラムレイアウト

<div>
左側のコンテンツ
</div>

<div>
右側のコンテンツ
</div>
```

---

### 7. content-2col-comparison - Before/After比較

**クラス名:** `content-2col-comparison`

**用途:** 改善前後の比較、対比表示

**特徴:**
- 左側（Before）は薄いグレー背景
- 右側（After）はプライマリーカラーの左ボーダー
- 視覚的に改善を強調

**使用例:**
```markdown
<!-- _class: content-2col-comparison -->

## Before/After比較

<div>

### Before
- 改善前の状態

</div>

<div>

### After
- 改善後の状態

</div>
```

---

### 8. content-3col - 3カラム

**クラス名:** `content-3col`

**用途:** 3つの項目を並列表示

**特徴:**
- 均等な3カラム配分
- 機能比較や選択肢の提示に最適
- コンパクトな情報整理

**使用例:**
```markdown
<!-- _class: content-3col -->

## 3カラムレイアウト

<div>

### カラム1
コンテンツ

</div>

<div>

### カラム2
コンテンツ

</div>

<div>

### カラム3
コンテンツ

</div>
```

---

### 9. content-image-right - 画像右配置

**クラス名:** `content-image-right`

**用途:** テキストと画像の組み合わせ表示

**特徴:**
- 左側にテキスト（2fr）
- 右側に画像（1fr）
- 画像は角丸と影付き

**使用例:**
```markdown
<!-- _class: content-image-right -->

## テキストと画像

<div>

説明文をここに記載

</div>

<div>

![画像](./images/sample.png)

</div>
```

---

### 10. content-center - 中央配置メッセージ

**クラス名:** `content-center`

**用途:** 重要なメッセージの強調表示

**特徴:**
- 画面中央に配置
- 大きな見出し
- シンプルで印象的

**使用例:**
```markdown
<!-- _class: content-center -->

## 重要なメッセージ

### 中央に配置されます

説明文
```

---

### 11. content-grid-2x2 - 2x2グリッド

**クラス名:** `content-grid-2x2`

**用途:** 4つの項目を均等に表示

**特徴:**
- 2行2列のグリッドレイアウト
- 画像とテキストのセット表示に最適
- 各項目に背景色付き

**使用例:**
```markdown
<!-- _class: content-grid-2x2 -->

## 2x2グリッド

<div>

![画像1](./images/1.png)

### 項目1
説明

</div>

<div>

![画像2](./images/2.png)

### 項目2
説明

</div>

<div>

![画像3](./images/3.png)

### 項目3
説明

</div>

<div>

![画像4](./images/4.png)

### 項目4
説明

</div>
```

---

### 12. content-steps - 縦3つステップ

**クラス名:** `content-steps`

**用途:** 段階的なプロセスの説明

**特徴:**
- 番号付きの円形バッジ
- 縦並びレイアウト
- 左ボーダー付き強調

**使用例:**
```markdown
<!-- _class: content-steps -->

## プロセス

<div class="steps-container">

<div class="step-item">
<div class="step-number">1</div>
<div class="step-content">

### ステップ1
説明文

</div>
</div>

<div class="step-item">
<div class="step-number">2</div>
<div class="step-content">

### ステップ2
説明文

</div>
</div>

<div class="step-item">
<div class="step-number">3</div>
<div class="step-content">

### ステップ3
説明文

</div>
</div>

</div>
```

---

### 13. content-list-panel - リスト＋補足パネル

**クラス名:** `content-list-panel`

**用途:** メインコンテンツと補足情報の表示

**特徴:**
- 左側にメインコンテンツ（2fr）
- 右側に補足パネル（1fr）
- パネルはアクセントカラーの左ボーダー付き

**使用例:**
```markdown
<!-- _class: content-list-panel -->

## タイトル

<div>

メインのリストや説明文

</div>

<div class="panel">

### 補足情報

注意事項など

</div>
```

---

## C. 強調・特殊系（2パターン）

### 14. emphasis-box - 強調ボックス（3段階）

**クラス名:** なし（ユーティリティクラス）

**用途:** 情報の重要度に応じた強調表示

**特徴:**
- 3段階の強調レベル
  - `box-light`: 軽い強調（通常の背景色）
  - `box-medium`: 中程度の強調（左ボーダー付き）
  - `box-strong`: 最強調（中央配置・大きな文字）

**使用例:**
```markdown
## 強調ボックス

<div class="box-light">
通常の情報
</div>

<div class="box-medium">
重要な情報
</div>

<div class="box-strong">
最重要メッセージ
</div>
```

---

### 15. stats - 統計・数値表示

**クラス名:** `stats`

**用途:** 数値データの視覚的な表示

**特徴:**
- 3カラムのグリッドレイアウト
- 大きな数値表示
- ラベル付き

**使用例:**
```markdown
<!-- _class: stats -->

## 実績

<div class="stats-container">

<div class="stat-item">
<div class="stat-number">95%</div>
<div class="stat-label">顧客満足度</div>
</div>

<div class="stat-item">
<div class="stat-number">1,200</div>
<div class="stat-label">導入実績</div>
</div>

<div class="stat-item">
<div class="stat-number">24h</div>
<div class="stat-label">サポート対応</div>
</div>

</div>
```

---

## テキスト強調クラス

インラインテキストを強調するためのユーティリティクラスです。

### highlight-primary

**用途:** プライマリーカラーでテキストを強調

**使用例:**
```markdown
<span class="highlight-primary">重要なテキスト</span>
```

### highlight-accent

**用途:** アクセントカラーでテキストを強調

**使用例:**
```markdown
<span class="highlight-accent">アクセントテキスト</span>
```

### highlight-secondary

**用途:** セカンダリーカラーでテキストを強調

**使用例:**
```markdown
<span class="highlight-secondary">セカンダリーテキスト</span>
```

---

## カラーパレット

テーマで使用されているカラーパレットは以下の通りです。

| 変数名 | カラーコード | 用途 |
|--------|--------------|------|
| `--color-primary` | #1B4565 | プライマリー（濃紺） |
| `--color-secondary` | #3E9BA4 | セカンダリー（ティール） |
| `--color-accent` | #1ab394 | アクセント（ティール系の緑） |
| `--text-dark` | #333333 | 濃い文字色 |
| `--text-light` | #ffffff | 明るい文字色 |
| `--text-muted` | #6b7280 | 控えめな文字色 |

---

## 自動挿入要素

### ロゴ

全スライド（タイトルとクロージング除く）の左下に自動的にロゴが表示されます。

- 位置: 左下（left: 30px, bottom: 30px）
- サイズ: 50px × 50px
- 画像パス: `assets/images/logo.png`

**ロゴを非表示にする:**
```markdown
<!-- _class: no-logo -->
```

### ページ番号

全スライド（タイトルとクロージング除く）の右下にページ番号が自動表示されます。

- 形式: `現在のページ / 総ページ数`
- 位置: 右下（right: 40px, bottom: 30px）

---

## カスタマイズ方法

### カラー変更

`themes/company.css` の `:root` セクションでカラー変数を変更できます。

```css
:root {
  --color-primary: #1B4565;
  --color-secondary: #3E9BA4;
  --color-accent: #1ab394;
}
```

### ロゴ変更

`assets/images/logo.png` を自社ロゴに差し替えてください。

### 背景画像変更

`assets/images/background.png` をタイトル背景用の画像に差し替えてください。

---

## トラブルシューティング

### ロゴが表示されない

- パスが正しいか確認（相対パス: `../assets/images/logo.png`）
- ファイルが存在するか確認
- `.marprc.yml` で `allowLocalFiles: true` が設定されているか確認

### カラムレイアウトが崩れる

- `<div>` タグが正しく閉じられているか確認
- HTMLとMarkdownの混在部分の構文を確認

### PDFエクスポートで画像が表示されない

- `--allow-local-files` オプションが指定されているか確認
- 画像パスが相対パスで正しく指定されているか確認

---

## 参考資料

- [Marp公式ドキュメント](https://marpit.marp.app/)
- `slides/example.md` - 全パターンの実例
- `slides/template.md` - 基本テンプレート
