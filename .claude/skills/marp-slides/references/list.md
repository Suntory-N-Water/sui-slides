# 一覧と分類

同じ役割の項目を並べる型と、主張に補足を添える型。

| 型 | 使う場面 |
| --- | --- |
| [`content-3col`](#content-3col) | 三項目を横に |
| [`content-4col`](#content-4col) | 四項目を横に |
| [`content-5col-maturity`](#content-5col-maturity) | 五段階の現在地 |
| [`content-grid-2x2`](#content-grid-2x2) | 四つの観点 |
| [`content-grid-2x3`](#content-grid-2x3) | 六つの観点 |
| [`content-list-panel`](#content-list-panel) | 主張と補足 |
| [`content-icon-list`](#content-icon-list) | 記号付きの縦並び |

列が増えるほど 1 項目に書ける文字数は減る。文章で説明したくなったら列を減らす。

`<div>` の中に Markdown の見出しや段落を書くときは、`<div>` の直後と `</div>` の直前に空行を入れる。

---

## content-3col

三つの項目を同じ重さで紹介する。順番も推奨も示さない。推す案があるなら `content-3col-accent`（`compare.md`）を使う。

```markdown
<!-- _class: content-3col -->

## 利用者が見る三つの情報

<div>

### 場所
どこで受け取るか。

</div>

<div>

### 時刻
いつ受け取れるか。

</div>

<div>

### 状態
準備ができたか。

</div>
```

## content-4col

四つの機能・段階・判断軸を並べる。`column-number` は順序があるときだけ付ける。順序がないなら省く。

```markdown
<!-- _class: content-4col -->

## 導入を四段階に分ける

<div>

<div class="column-number">01</div>
<h3>調査</h3>
現状を知ります。

</div>

<div>

<div class="column-number">02</div>
<h3>設計</h3>
目標を決めます。

</div>

<div>

<div class="column-number">03</div>
<h3>試行</h3>
小さく試します。

</div>

<div>

<div class="column-number">04</div>
<h3>展開</h3>
対象を広げます。

</div>
```

## content-5col-maturity

成熟度・習熟度・導入段階を五段階で示し、現在地を一つだけ `is-current` で強調する。段階がつながっていることを横罫線で示す型なので、独立した五項目には使わない。

```markdown
<!-- _class: content-5col-maturity -->

## チームの改善段階

<div>
<div class="maturity-level">01</div>
<h3>個人</h3>
担当者が工夫します。
</div>

<div>
<div class="maturity-level">02</div>
<h3>共有</h3>
知見を共有します。
</div>

<div class="is-current">
<div class="maturity-level">03</div>
<h3>標準化</h3>
手順を揃えます。
</div>

<div>
<div class="maturity-level">04</div>
<h3>計測</h3>
成果を見ます。
</div>

<div>
<div class="maturity-level">05</div>
<h3>継続</h3>
改善を習慣にします。
</div>
```

## content-grid-2x2

四つの観点を二行二列で整理する。項目は同じ大きさになる。順序があるなら `content-steps`（`compare.md`）か `content-4col` を使う。

```markdown
<!-- _class: content-grid-2x2 -->

## 課題を四つに分ける

<div>

### 目的
何を良くするか。

</div>

<div>

### 対象
誰が使うか。

</div>

<div>

### 条件
何を満たすか。

</div>

<div>

### 指標
何で測るか。

</div>
```

## content-grid-2x3

六つの機能や観点を一枚で見渡す。各項目は見出し一つと短い説明に絞る。7 つ以上は入れず、スライドを分ける。

```markdown
<!-- _class: content-grid-2x3 -->

## 確認する六つの観点

<div><span class="grid-number">01</span><h3>目的</h3>何のためか</div>
<div><span class="grid-number">02</span><h3>対象</h3>誰が使うか</div>
<div><span class="grid-number">03</span><h3>条件</h3>何を満たすか</div>
<div><span class="grid-number">04</span><h3>手順</h3>どう進めるか</div>
<div><span class="grid-number">05</span><h3>指標</h3>何で測るか</div>
<div><span class="grid-number">06</span><h3>次の行動</h3>何を決めるか</div>
```

## content-list-panel

左で主張を述べ、右の `panel` で条件や例外を補足する。右は主張と同じ重さにしない。左右が対等なら `content-2col`（`compare.md`）を使う。

```markdown
<!-- _class: content-list-panel -->

## 主張と補足を分ける

<div>

### 守ること

1. 結論を先に置く
2. 見出しに役割を書く
3. 補足を短くする

</div>

<div class="panel">

### 注意
例外や判断の前提をここに置きます。

</div>
```

## content-icon-list

番号や短い記号を手がかりに、関連する項目を縦に読ませる。`icon-mark` は装飾ではなく、分類や順序を示すために使う。

```markdown
<!-- _class: content-icon-list -->

## 先に揃える情報

<div class="icon-list">

<div class="icon-item">
<span class="icon-mark">01</span>
<div><h3>目的</h3><p>今回の変更で何を良くするか。</p></div>
</div>

<div class="icon-item">
<span class="icon-mark">02</span>
<div><h3>範囲</h3><p>どこまでを対象にするか。</p></div>
</div>

<div class="icon-item">
<span class="icon-mark">03</span>
<div><h3>制約</h3><p>変えてはいけない条件は何か。</p></div>
</div>

</div>
```
