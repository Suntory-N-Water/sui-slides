---
name: marp-slides
description: このリポジトリで Marp のスライド資料を新しく作る、または既存の slide.md を書き直すための型とデザイン規約。伝えたい内容に合うレイアウトクラスを選び、themes/ の CSS が想定する構造で書く。「スライドを作って」「プレゼン資料を用意して」「提案資料にまとめて」「この内容をスライドにして」「発表用のデッキが欲しい」「既存のスライドを直して」「章立てを考えて」といった依頼では必ず使う。ユーザーが Marp やクラス名を明示しなくても、スライド・資料・プレゼン・発表・デッキの作成や改稿の話が出たら参照する。
---

# Marp スライド作成

## 前提

スライドは `slides/<名前>/slide.md` に置く。見た目は `themes/claude.css` と `themes/starbucks.css` が持ち、Markdown 側は `<!-- _class: ... -->` で型を指定して、CSS が想定する構造の HTML を書く。

型を先に決めるのは、本文を書いてから体裁を整えると、見出しと本文の役割が曖昧なまま面を埋める資料になるため。1 枚に 1 つの目的を置き、目的が変わったらスライドを分ける。

## 手順

### 1. 前提を聞く

不明なら確認する。推測で埋めると後の作り直しが大きくなる。

- 誰に向けて、何を決めてもらう資料か
- 発表時間と想定枚数
- 手元にある事実（数値、利用者の声、既存の図版）
- テーマ（`claude` / `starbucks`）

### 2. テーマを選ぶ

両テーマともレイアウトクラスは同じで、色・書体・装飾だけが違う。差分は `references/parts.md` の末尾を見る。

- `claude` — 生成りの背景に珊瑚色。社内提案、技術説明、汎用
- `starbucks` — 白と緑。店舗・接客・ブランド寄りの題材。`cta` と `reward` はこちらだけ

### 3. 構成を型の列として書き出す

本文を書く前に、「何ページ目にどの型で何を言うか」を一覧にしてユーザーに見せる。ここで枚数と話の順番を合意しておくと、書き直しが 1 枚単位で済む。

骨格は `title` → `agenda` → (`section` + 中身) × 章数 → `summary` → `closing`。

### 4. 型を選ぶ

| 伝えたいこと | 型 | 詳細 |
| --- | --- | --- |
| 資料の始まり・章の切り替え・目次 | `title` / `section` / `agenda` | `references/frame.md` |
| 一文を記憶に残す | `content-center` / `quote` | `references/frame.md` |
| 質問を受け付ける | `qa` | `references/frame.md` |
| 要点を回収して終える | `summary` / `closing` | `references/frame.md` |
| 二つの内容を同じ重さで並べる | `content-2col` | `references/compare.md` |
| 変更前と変更後を比べる | `content-2col-comparison` | `references/compare.md` |
| 三つの案から一つを推す | `content-3col-accent` | `references/compare.md` |
| 数値を目立たせる | `stats` | `references/compare.md` |
| 同じ指標の現在値と目標値を比べる | `content-stats-comparison` | `references/compare.md` |
| 手順を示す | `content-steps` / `content-steps-vertical` | `references/compare.md` |
| 時間の流れを示す | `content-timeline` | `references/compare.md` |
| 項目を一覧にする | `content-3col` / `content-4col` / `content-grid-2x2` / `content-grid-2x3` / `content-icon-list` | `references/list.md` |
| 五段階の現在地を示す | `content-5col-maturity` | `references/list.md` |
| 主張と補足を分ける | `content-list-panel` | `references/list.md` |
| 画像を説明に添える | `content-image-right` / `content-image-cards` | `references/image.md` |
| 画像を場面の中心にする | `background-image-full` / `background-image-right` | `references/image.md` |
| 本文中で重要度を分ける・全面で強調する・コードを見せる | `box-*` / `callout` / `dark` / `content-dark-code` | `references/parts.md` |

型を決めたら、該当する参照ファイルだけを読んで記述例をなぞる。CSS は特定の入れ子を前提にしているため、記述例の構造から離れると崩れる。

クラスを指定しない通常のスライドは、見出しと段落だけの本文ページになる。特別な構造が要らない説明はこれで足りる。

### 5. 書く

先頭の frontmatter は次の形にする。

```markdown
---
marp: true
theme: starbucks
paginate: true
---
```

スライドの区切りは `---` の行。文章は `references/writing.md` の規則に従う。

### 6. 確認する

```bash
pnpm lint:text
```

指摘が出たら文章を直す。表示の確認は `pnpm dev`（`slides` 以下を配信する）。PDF にするときは `pnpm exec marp slides/<名前>/slide.md --pdf` を使う。テーマと画像の設定は `.marprc.yml` と `package.json` の `marp.themeSet` から適用される。

## デザイン規約

`.claude/rules/design-rules.md` に全文がある。スライドを書くうえで外せないのは次の点。

- 面を塗るのはスライド全面の帯（`section` / `closing` / `callout` / `content-dark-code`）とボタン・最上位の強調だけ。カードやパネルに背景色を持たせない
- 区切りは罫線の太さで 3 段階を作る。色の濃さで段階を作らない
- 角丸と影は使わない。例外はバッジとボタン
- 番号を丸いバッジにしない。大きさと配置で順序を示す
- 埋まらないなら余白のまま残す。囲みを足して面積を稼がない
- 色や太さを変えるのは Before/After のように内容に差があるときだけ

## 既存スライドを直すとき

1. `slide.md` を読み、各ページの型と目的を洗い出す
2. 目的に対して型が合っていないページだけを選ぶ（例: 差があるのに `content-2col`、順序があるのに `content-grid-2x2`）
3. 型を変えると構造も変わるので、該当する参照ファイルの記述例に合わせて書き直す
4. `pnpm lint:text` を通す

既に読める資料の型を、網羅のためだけに入れ替えない。直す理由を 1 ページずつ言えるものだけ直す。

## 参考

- `slides/examples/slide.md` — 全部の型の使用例
- `slides/starbucks-sample/slide.md` — 実際の提案資料の例
- [Marp 公式ドキュメント](https://marpit.marp.app/)
