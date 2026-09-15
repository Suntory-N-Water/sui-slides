# 画像と場面

| 型 | 使う場面 |
| --- | --- |
| [`content-image-right`](#content-image-right) | 左に本文、右に画像 1 枚 |
| [`content-image-cards`](#content-image-cards) | 画像を先に見せる三列 |
| [`background-image-full`](#background-image-full) | 全面画像 |
| [`background-image-right`](#background-image-right) | 右半分に大きな画像 |

画像は本文の代わりにしない。本文で「この画像の何を見るか」を書く。画像だけで意味を伝えようとすると、聞き手ごとに読み取る内容が変わる。

## パスと設定

- 画像は slide.md から見た相対パスで指定する（例: `../starbucks-sample/images/signage.svg`）
- ローカル画像は `.marprc.yml` の `allowLocalFiles: true` で読み込まれる。marp CLI を直接叩くときは `--allow-local-files` を付ける
- 代替テキストは、画像が何を示すかを短く書く
- SVG を新しく作るときも `.claude/rules/design-rules.md` に従う。角丸の塗り矩形は CSS では直せないため、行の区切りは線、状態の強調は短い縦バーで示す

---

## content-image-right

左で主張を説明し、右に画像を 1 枚置く。画像を大きく見せたいなら `background-image-right` を使う。

```markdown
<!-- _class: content-image-right -->

## 受け取り場所を先に伝える

<div>

### 迷う前に案内する
完了画面の直後に、受け取り場所を表示します。

</div>

<div>
![店内表示](../starbucks-sample/images/signage.svg)
</div>
```

## content-image-cards

三つの利用場面や事例を、画像を先に見せて紹介する。画像・見出し・説明の順を三列で揃える。同じ画像を使い回すときも、説明の役割は列ごとに変える。

```markdown
<!-- _class: content-image-cards -->

## 三つの利用場面

<div class="image-card-grid">

<div class="image-card">
![画面例](../starbucks-sample/images/signage.svg)
<h3>受付</h3>
注文を受け付けます。
</div>

<div class="image-card">
![画面例](../starbucks-sample/images/signage.svg)
<h3>受け取り</h3>
完成状態を知らせます。
</div>

<div class="image-card">
![画面例](../starbucks-sample/images/signage.svg)
<h3>振り返り</h3>
結果を次へつなげます。
</div>

</div>
```

## background-image-full

章の転換や短い主張を、場面の印象と一緒に伝える。

`![bg]` は Marp が背景層と本文層に分けて処理するため、本文の要素を画像の前後に挟まない。画像の指定はクラスコメントの直後に置き、そのあとに本文を続ける。本文は短くし、画像が明るくて文字が読めないときは `brightness:` や `opacity:` を指定する。

```markdown
<!-- _class: background-image-full -->
![bg brightness:0.65](../starbucks-sample/images/signage.svg)

# 場面が変われば、行動も変わる

## 受け取り場所を先に示す
```

## background-image-right

左で主張を説明し、右で利用場面や対象を大きく見せる。`content-image-right` より画像の面積が大きく、左右の役割が分かれる。

```markdown
<!-- _class: background-image-right -->

## 受け取りの場面を変える

<div class="background-copy">

### 先に行き先を示す
受け取り場所を注文完了画面で案内し、迷う時間を減らします。

</div>

<div class="background-visual">
![店内表示](../starbucks-sample/images/signage.svg)
</div>
```
