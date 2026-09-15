# 比較・数値・手順・時間

判断のために差を見せる型と、順序や時間の流れを見せる型。

| 型 | 使う場面 |
| --- | --- |
| [`content-2col`](#content-2col) | 同じ重さの二つ |
| [`content-2col-comparison`](#content-2col-comparison) | 変更前と変更後 |
| [`content-3col-accent`](#content-3col-accent) | 三案から一つを推す |
| [`stats`](#stats) | 数値を 3 つまで大きく |
| [`content-stats-comparison`](#content-stats-comparison) | 同じ指標の現在値と目標値 |
| [`content-steps`](#content-steps) | 横に並ぶ 3 工程 |
| [`content-steps-vertical`](#content-steps-vertical) | 縦に読む工程 |
| [`content-timeline`](#content-timeline) | 時間の流れ |

`<div>` の中に Markdown の見出しや段落を書くときは、`<div>` の直後と `</div>` の直前に空行を入れる。空行がないと Markdown として解釈されず、見出しがそのまま文字で出る。

---

## content-2col

関係する二つの内容を同じ重さで並べる。左右に優劣や前後がある場合は `content-2col-comparison` を使う。

```markdown
<!-- _class: content-2col -->

## 二つの視点を並べる

<div>

### 利用者の視点
迷わず受け取れることを優先します。

</div>

<div>

### 店舗の視点
案内の手間を増やさないことを優先します。

</div>
```

## content-2col-comparison

変更前と変更後、問題と対策など、差を見せる。左を現状、右を変更後に固定し、右へ視線が進むよう罫線の太さで差を示す。

```markdown
<!-- _class: content-2col-comparison -->

## 案内を変える

<div>

### 現状
受け取り場所を画面の最後にだけ表示します。

</div>

<div>

### 変更後
注文完了の直後に受け取り場所を表示します。

</div>
```

## content-3col-accent

三つの選択肢を比べ、推す案を一つ示す。三列は同じ大きさのまま、`is-recommended` を付けた列だけ太い上罫線が付く。色の違いだけで優劣を示さない。

推す案がないなら `content-3col`（`list.md`）を使う。

```markdown
<!-- _class: content-3col-accent -->

## 三つの案から一つを選ぶ

<div>

### 小さく始める
対象を一店舗に絞ります。

</div>

<div class="is-recommended">

### 段階的に広げる
効果を確かめて対象を増やします。

</div>

<div>

### 一度に切り替える
全体を短期間で変更します。

</div>
```

## stats

重要な数値を 3 つまで大きく示す。数値そのものが主役で、`stat-label` は意味を短く添えるだけ。複数の状態を比べるなら `content-stats-comparison` を使う。

```markdown
<!-- _class: stats -->

## まず覚える三つの数字

<div class="stats-container">

<div class="stat-item">
<div class="stat-number">6.4分</div>
<div class="stat-label">現在の待ち時間</div>
</div>

<div class="stat-item">
<div class="stat-number">41%</div>
<div class="stat-label">完了記録率</div>
</div>

<div class="stat-item">
<div class="stat-number">3店</div>
<div class="stat-label">試行する店舗</div>
</div>

</div>
```

## content-stats-comparison

同じ指標の現在値と目標値、変更前と変更後を行で揃えて比べる。`is-after` を付けた列が変更後・目標として強調される。見出し行の `<span>` と各行の `<span>` の数を揃える。

```markdown
<!-- _class: content-stats-comparison -->

## 変更後に目指す状態

<div class="stats-comparison">
<div class="stats-comparison-head">
<span>指標</span><span>現在</span><span>目標</span>
</div>
<div class="stats-comparison-row">
<span>待ち時間</span><span>6.4分</span><span class="is-after">4.0分</span>
</div>
<div class="stats-comparison-row">
<span>誤受け取り</span><span>月12件</span><span class="is-after">月3件</span>
</div>
<div class="stats-comparison-row">
<span>完了記録率</span><span>41%</span><span class="is-after">95%</span>
</div>
</div>
```

## content-steps

三つの工程を横に並べ、左から右へ進む順番を示す。各工程の説明は一文まで。説明が長いなら `content-steps-vertical` を使う。

```markdown
<!-- _class: content-steps -->

## 三段階で試す

<div class="steps-container">

<div class="step-item">
<div class="step-number">01</div>
<div class="step-content">

### 観察する
現状の数字を集めます。

</div>
</div>

<div class="step-item">
<div class="step-number">02</div>
<div class="step-content">

### 試す
小さな範囲で変更します。

</div>
</div>

<div class="step-item">
<div class="step-number">03</div>
<div class="step-content">

### 広げる
結果を見て対象を増やします。

</div>
</div>

</div>
```

## content-steps-vertical

工程を上から下へ読ませる。構造は `content-steps` と同じで、番号が左に揃い、工程の境目が横罫線になる。読む方向が違うので、横に入りきらないときはこちらへ替える。

```markdown
<!-- _class: content-steps-vertical -->

## 判断までの進め方

<div class="steps-container">

<div class="step-item">
<div class="step-number">01</div>
<div class="step-content">

### 事実を集める
数字と利用者の声を揃えます。

</div>
</div>

<div class="step-item">
<div class="step-number">02</div>
<div class="step-content">

### 仮説を試す
小さな範囲で効果を確かめます。

</div>
</div>

<div class="step-item">
<div class="step-number">03</div>
<div class="step-content">

### 判断を残す
結果と次の条件を記録します。

</div>
</div>

</div>
```

## content-timeline

年月やリリースなど、時間の流れを示す。時点は 4 つまで。日付と出来事を分けて書く。日付を伴わない工程の順番なら steps を使う。

```markdown
<!-- _class: content-timeline -->

## 展開の予定

<div class="timeline">

<div class="timeline-item">
<div class="timeline-date">2026.04</div>
<h3>調査</h3>
<p>課題を定めます。</p>
</div>

<div class="timeline-item">
<div class="timeline-date">2026.06</div>
<h3>試行</h3>
<p>小さく試します。</p>
</div>

<div class="timeline-item">
<div class="timeline-date">2026.08</div>
<h3>評価</h3>
<p>数字を比較します。</p>
</div>

<div class="timeline-item">
<div class="timeline-date">2026.10</div>
<h3>展開</h3>
<p>範囲を広げます。</p>
</div>

</div>
```
