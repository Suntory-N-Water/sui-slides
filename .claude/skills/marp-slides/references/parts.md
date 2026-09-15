# 補助クラスとテーマ差分

レイアウトの型とは別に、本文の中で使う小さなクラスと、スライド全面の見え方を変えるクラス。色違いを新しい型として増やさないため、番号は付けていない。

## 全面の見え方を変えるクラス

`<!-- _class: ... -->` で指定する。レイアウトの型と同じ位置に書く。

### callout

途中で一文だけを強く伝える全面の帯。章の中で一度だけ使う。続けて使うと帯であることの意味がなくなる。

```markdown
<!-- _class: callout -->

## 途中で、一つだけ強く伝える

<p>読み手に残したい一文を、全面の帯に置きます。</p>
```

### dark

暗い背景で内容を見せる。画面の見本や、明るいスライドが続いたあとの切り替えに使う。

### content-dark-code

左で説明、右でコードや設定を見せる。コードを読ませるスライドは面を塗ってよい数少ない例外。

````markdown
<!-- _class: content-dark-code -->

## 設定例は暗い面にまとめる

<div>

### 説明
左側で記法の役割を説明します。

</div>

<div>

```yaml
pattern: content-2col
purpose: comparison
```

</div>
````

### reward（starbucks のみ）

特典や表彰だけに使う特別な色。通常の情報と区別する必要がある節目以外には使わない。ゴールドを汎用の強調色として使わないための型。

```markdown
<!-- _class: reward -->

<div class="badge badge-reward">特別な一枚</div>

## 節目だけ、特別な色で示す

<div class="panel">

通常の情報と区別する必要がある表彰や達成だけに使います。

</div>
```

## 本文の中で使うクラス

### box-light / box-medium / box-strong

補足・注意・最重要を三段階で示す。塗りがあるのは `box-strong` だけで、下の二つは罫線の太さで差を作る。三種類を一枚に詰め込まず、必要なものだけ使う。

```markdown
## 重要度を三段階で示す

<div class="box-light">

### 補足
流れを止めずに読ませる情報です。

</div>

<div class="box-medium">

### 注意
判断に必要な条件です。

</div>

<div class="box-strong">

### 最重要
最後に覚えてほしい一文です。

</div>
```

### badge

短い分類名を添える。`title` の題名の上や、`reward` で使う。1 枚に 1 つまで。

```markdown
<div class="badge">STORE EXPERIENCE 2026</div>
```

### cta（starbucks のみ）

次の行動を示すボタン。資料の最後か、承認を求めるスライドで 1 つだけ使う。

```markdown
<div><span class="cta">中位案で承認をお願いします</span></div>
```

### caption

見出しや本文に添える小さな注記。`content-center` や `agenda` の補足に使う。

```markdown
<p class="caption">案内を先に示し、利用者の判断を助けます。</p>
```

### panel

`content-list-panel` と `reward` の中で、補足を別の領域として見せる。

## 自動で入る要素

- 通常のスライドは右下にページ番号が入る（frontmatter の `paginate: true` が前提）
- `title` と `closing` ではページ番号を表示しない
- `section` は全面が塗られ、ページ番号の色も切り替わる

## テーマ差分

レイアウトの型はどちらのテーマにも同じクラス名で実装されている。違うのは色・書体と、次の 2 つの有無。

| 項目 | claude | starbucks |
| --- | --- | --- |
| 基調 | 生成りの背景と珊瑚色 | 白と 4 段階の緑 |
| 書体 | Noto Sans JP | Inter / Nunito Sans 系 |
| `reward` | なし | あり |
| `cta` / `cta-outline` | なし | あり |
| `badge-reward` | なし | あり |

テーマを切り替えるときは、`reward` と `cta` を使っているスライドを別の型に置き換える必要がある。
