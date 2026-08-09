---
marp: true
theme: claude
paginate: true
size: 16:9
title: Claude Code の細かい操作 3 つ
description: Esc 2 回での巻き戻し、/copy での持ち出し、ターミナルのショートカットキー
---

<style>
section {
  padding-bottom: 72px;
}
section.title h1 {
  max-width: 1040px;
  font-size: 60px;
}
section.title h2 {
  max-width: 960px;
  font-size: 24px;
}
.chapter-no {
  margin-bottom: 18px;
  color: rgba(255, 255, 255, 0.8);
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 1.2px;
}
.goal-list {
  display: grid;
  gap: 18px;
  /* 右端まで文字を届かせないための版面内マージン */
  max-width: 1080px;
  margin-top: 28px;
}
.goal-item {
  display: grid;
  grid-template-columns: 210px 1fr;
  gap: 28px;
  border-top: 1px solid var(--claude-hairline);
  padding-top: 16px;
}
.goal-item strong {
  color: var(--claude-coral);
}
.op-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 44px;
  margin-top: 24px;
}
.op-layout > div {
  border-top: 2px solid var(--claude-coral);
  padding-top: 18px;
}
.op-layout pre {
  margin-bottom: 16px;
}
.branch-table td:first-child {
  width: 320px;
}
.branch-table td {
  vertical-align: top;
}
.fence-compare {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 44px;
  margin-top: 22px;
}
.fence-compare > div {
  border-top: 1px solid var(--claude-hairline);
  /* 罫線の太さが左右で 3px ずれるぶんを padding で吸収し、見出しの位置を揃える */
  padding-top: 21px;
}
.fence-compare > div:last-child {
  border-top: 4px solid var(--claude-coral);
  padding-top: 18px;
}
.keymap {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0 48px;
  margin-top: 12px;
}
.keymap table {
  margin-bottom: 0;
}
.keymap td:first-child {
  width: 120px;
  font-family: var(--claude-mono);
}
.cheat {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0 48px;
  margin-top: 8px;
}
.cheat h3 {
  border-top: 2px solid var(--claude-coral);
  padding-top: 14px;
}
.cheat table {
  margin-bottom: 0;
  font-size: 16px;
}
.cheat td {
  padding: 9px 10px;
  vertical-align: top;
}
.cheat td:first-child {
  width: 132px;
  font-family: var(--claude-mono);
}
.sources {
  display: grid;
  gap: 10px;
  margin-top: 16px;
  font-size: 15px;
}
.source {
  display: grid;
  grid-template-columns: 260px 1fr;
  border-top: 1px solid var(--claude-hairline);
  padding-top: 10px;
}
</style>

<!-- _class: title -->

# 手が止まる回数を減らす<br>Claude Code の細かい操作 3 つ

## Esc 2 回での巻き戻し、/copy での持ち出し、ターミナルのショートカットキー

---

## 読み終えたあとにできること

<div class="goal-list">
  <div class="goal-item">
    <div><strong>Esc 2 回</strong></div>
    <div>Claude Code が明らかにおかしい方向に進んだとき、キー操作だけで過去のメッセージまで巻き戻せます。</div>
  </div>
  <div class="goal-item">
    <div><strong>/copy</strong></div>
    <div>直近の Claude の応答をクリップボードに取り出し、貼り先で表示が崩れない形で持ち出せます。</div>
  </div>
  <div class="goal-item">
    <div><strong>ターミナルのキーバインド</strong></div>
    <div>長いプロンプトを直すとき、矢印キーを連打せずにカーソル移動と削除ができます。</div>
  </div>
</div>

<p class="caption" style="margin-top: 32px;">3 つとも、知らなくても Claude Code は普通に使えます。</p>

---

<!-- _class: agenda -->

## 目次

1. Esc 2 回での巻き戻し
2. /copy での持ち出し
3. ターミナルのショートカットキー

---

<!-- _class: section -->

<div class="chapter-no">第1章</div>

# Esc 2 回での巻き戻し

## 追加で指示を重ねるのではなく、分岐点まで戻ります。

---

<!-- _class: content-steps -->

## 巻き戻しの手順

<div class="steps-container">
  <div class="step-item">
    <div class="step-number">1</div>
    <div class="step-content">
      <h3><code>Esc</code> を押す</h3>
      <p>実行中の処理が止まります。</p>
    </div>
  </div>
  <div class="step-item">
    <div class="step-number">2</div>
    <div class="step-content">
      <h3>続けて <code>Esc</code> を 2 回</h3>
      <p>過去のメッセージまで巻き戻すメニューが開きます。<code>/rewind</code> と同じメニューですが、打ち込まずに連打で戻せます。</p>
    </div>
  </div>
  <div class="step-item">
    <div class="step-number">3</div>
    <div class="step-content">
      <h3>戻る地点を選ぶ</h3>
      <p>選んだメッセージの時点まで会話が戻ります。</p>
    </div>
  </div>
</div>

---

## 入力欄にテキストが残っている場合

<table class="branch-table">
  <thead>
    <tr><th>入力欄の状態</th><th>Esc を 2 回押した結果</th></tr>
  </thead>
  <tbody>
    <tr><td>空</td><td>巻き戻しメニューが開きます</td></tr>
    <tr><td>テキストが残っている</td><td>先に入力がクリアされ、メニューは開きません</td></tr>
  </tbody>
</table>

<div class="box-medium" style="margin-top: 30px;">

クリアされた下書きは履歴に残っています。<code>↑</code> を押すと呼び出せます。

</div>

<p class="caption" style="margin-top: 24px;">この分岐は公式ドキュメント (Interactive mode) に明記されています。</p>

---

<!-- _class: section -->

<div class="chapter-no">第2章</div>

# /copy での持ち出し

## Claude の応答を、外へ貼り付けられる形で取り出します。

---

## /copy の操作と使いどころ

<div class="op-layout">
  <div>

### 操作と結果

```text
/copy
```

直近の Claude の応答がクリップボードに入ります。コードブロックを含む応答では、ブロック単位でコピーするか全文コピーするかを選ぶメニューが開きます。

  </div>
  <div>

### 使いどころ

- Claude が関与できない場所へメッセージを送る
- 別のチャットやセッションに指示を渡す
- 出力されたコマンドを自分のターミナルで実行する

  </div>
</div>

---

## バッククォート 4 つでの出力指示

```text
バッククォート 4 つでコピペしやすいように出力してください
```

<p style="max-width: 900px; margin-top: 32px;">この一文を添えると、応答全体が 4 つのバッククォートで囲まれて出力されます。中にコードブロックが含まれていても、貼り先でブロックが途中で閉じません。</p>

<p class="caption" style="margin-top: 24px;">貼り先が Markdown を解釈しない場所なら、この指示は要りません。</p>

---

## バッククォートを 4 つにする理由

<div class="fence-compare">
  <div>

### 外側が 3 つのとき

内側のコードブロックの ``` がフェンスとして解釈され、そこでブロックが閉じてしまいます。貼り先が Markdown をレンダリングする場所 (VS Code で保存した `.md`、Notion など) では、シンタックスハイライトが効かず表示が崩れます。

  </div>
  <div>

### 外側が 4 つのとき

CommonMark の仕様では、閉じフェンスには開きフェンスと同じ文字数以上のバッククォートが必要です。外側を 4 つにすれば、内側の 3 つはフェンスとして解釈されず、ただの中身として扱われます。

  </div>
</div>

---

<!-- _class: section -->

<div class="chapter-no">第3章</div>

# ターミナルの<br>ショートカットキー

## Claude Code ではなく、ターミナル全般で使えるキー操作です。

---

## キーバインド一覧

<div class="keymap">
  <div>
    <table>
      <thead><tr><th>移動</th><th>動作</th></tr></thead>
      <tbody>
        <tr><td>Ctrl+F</td><td>カーソルを右へ 1 文字移動</td></tr>
        <tr><td>Ctrl+B</td><td>カーソルを左へ 1 文字移動</td></tr>
        <tr><td>Ctrl+A</td><td>行頭へ移動</td></tr>
        <tr><td>Ctrl+E</td><td>行末へ移動</td></tr>
        <tr><td>Ctrl+P</td><td>上へ移動 (前の履歴)</td></tr>
        <tr><td>Ctrl+N</td><td>下へ移動 (次の履歴)</td></tr>
      </tbody>
    </table>
  </div>
  <div>
    <table>
      <thead><tr><th>削除</th><th>動作</th></tr></thead>
      <tbody>
        <tr><td>Ctrl+U</td><td>カーソルから左を全部削除</td></tr>
        <tr><td>Ctrl+K</td><td>カーソルから右を全部削除</td></tr>
        <tr><td>Ctrl+W</td><td>直前の単語を削除</td></tr>
      </tbody>
    </table>
  </div>
</div>

<p class="caption" style="margin-top: 18px;">Emacs 由来で GNU Readline に引き継がれたキーバインドで、bash や zsh を含むターミナル全般で使えます。</p>

---

## Ctrl+U の使いどころと注意点

<div class="op-layout">
  <div>

### いちばん出番が多い操作

長いプロンプトを書き損じたとき、<code>Ctrl+U</code> でカーソルから左を全部消し、頭から打ち直せます。

  </div>
  <div>

### VS Code で効かないとき

VS Code のエディタ上で Claude Code を使っている場合、VS Code 側のキーバインドが優先されて効かないことがあります。効かないキーは、VS Code 側で割り当てを変更すれば使えます。

  </div>
</div>

---

## チートシート

<div class="cheat">
  <div>

### Claude Code

<table>
  <tbody>
    <tr><td>Esc</td><td>実行中の処理を止める</td></tr>
    <tr><td>Esc ×2</td><td>巻き戻しメニューを開く (入力欄が空のとき)</td></tr>
    <tr><td>↑</td><td>クリアした下書きを呼び出す</td></tr>
    <tr><td>/copy</td><td>直近の応答をクリップボードにコピー</td></tr>
    <tr><td>指示文</td><td>バッククォート 4 つで出力させる</td></tr>
  </tbody>
</table>

  </div>
  <div>

### ターミナル

<table>
  <tbody>
    <tr><td>Ctrl+A / E</td><td>行頭 / 行末へ移動</td></tr>
    <tr><td>Ctrl+F / B</td><td>右 / 左へ 1 文字移動</td></tr>
    <tr><td>Ctrl+P / N</td><td>前 / 次の履歴へ移動</td></tr>
    <tr><td>Ctrl+U / K</td><td>カーソルから左 / 右を全部削除</td></tr>
    <tr><td>Ctrl+W</td><td>直前の単語を削除</td></tr>
  </tbody>
</table>

  </div>
</div>

---

## 参考ドキュメント

<div class="sources">
  <div class="source"><strong>Interactive mode</strong><span>code.claude.com/docs/en/interactive-mode</span></div>
  <div class="source"><strong>Slash commands</strong><span>code.claude.com/docs/en/commands</span></div>
  <div class="source"><strong>CommonMark (Fenced code blocks)</strong><span>spec.commonmark.org/0.31.2/#fenced-code-blocks</span></div>
  <div class="source"><strong>Bindable Readline Commands</strong><span>gnu.org/software/bash/manual/html_node/Bindable-Readline-Commands.html</span></div>
</div>
