---
marp: true
theme: claude
paginate: true
size: 16:9
title: Claude Codeの長い会話で指示が外れやすくなる理由と対策
description: コンテキストウィンドウの仕組みと4つの運用方法
---

<style>
section {
  padding-bottom: 72px;
}
section.title h1 {
  max-width: 1040px;
  font-size: 62px;
}
section.title h2 {
  max-width: 980px;
  font-size: 24px;
}
.chapter-no {
  margin-bottom: 18px;
  color: rgba(255, 255, 255, 0.8);
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 1.2px;
}
.topic {
  margin-bottom: 14px;
  color: var(--claude-coral);
  font-size: 14px;
  font-weight: 600;
}
.doc-intro {
  display: grid;
  grid-template-columns: 0.8fr 1.2fr;
  gap: 52px;
  margin-top: 24px;
}
.doc-intro > div {
  border-top: 2px solid var(--claude-coral);
  padding-top: 20px;
}
.doc-intro ol {
  margin-top: 0;
}
.plain-note {
  border-left: 4px solid var(--claude-coral);
  padding: 14px 0 14px 22px;
}
.conversation-table {
  display: grid;
  gap: 12px;
  margin-top: 24px;
}
.conversation-row {
  display: grid;
  grid-template-columns: 92px 1fr;
  align-items: center;
  gap: 18px;
}
.turn-label {
  color: var(--claude-muted);
  font-size: 15px;
  font-weight: 600;
}
.history-line {
  border: 1px solid var(--claude-hairline);
  padding: 12px 16px;
  font-family: var(--claude-mono);
  font-size: 16px;
}
.history-line .past {
  color: var(--claude-muted);
}
.history-line .current {
  color: var(--claude-coral);
  font-weight: 700;
}
.history-stack {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
  align-items: end;
  height: 350px;
  margin-top: 18px;
}
.stack-column {
  display: flex;
  flex-direction: column-reverse;
}
.stack-block {
  border-top: 1px solid var(--claude-canvas);
  background: var(--claude-card-strong);
  padding: 8px 12px;
  color: var(--claude-body);
  font-family: var(--claude-mono);
  font-size: 13px;
}
.stack-block.system {
  background: var(--claude-dark);
  color: var(--claude-on-dark);
}
.stack-block.current {
  background: var(--claude-coral);
  color: #fff;
}
.stack-caption {
  margin-top: 10px;
  color: var(--claude-muted);
  text-align: center;
  font-size: 14px;
}
.window-layout {
  display: grid;
  grid-template-columns: 1.15fr 0.85fr;
  gap: 54px;
  margin-top: 26px;
}
.window-frame {
  border: 3px solid var(--claude-ink);
  padding: 12px;
}
.window-bar {
  display: flex;
  height: 82px;
}
.window-history {
  width: 78%;
  background: var(--claude-card-strong);
  padding: 26px 18px;
  color: var(--claude-muted);
  font-size: 16px;
}
.window-current {
  width: 22%;
  background: var(--claude-coral);
  padding: 16px 10px;
  color: #fff;
  text-align: center;
  font-size: 15px;
}
.window-limit {
  margin-top: 10px;
  text-align: right;
  color: var(--claude-muted);
  font-size: 13px;
}
.definition {
  display: grid;
  grid-template-columns: 0.8fr 1.2fr;
  gap: 48px;
  margin-top: 30px;
}
.term {
  border-top: 3px solid var(--claude-coral);
  padding-top: 18px;
}
.term h3 {
  font-size: 38px;
}
.definition-body {
  border-top: 1px solid var(--claude-hairline);
  padding-top: 20px;
}
.cause-chain {
  display: grid;
  grid-template-columns: 1fr 34px 1fr 34px 1fr;
  gap: 12px;
  align-items: stretch;
  margin-top: 42px;
}
.cause-item {
  border-top: 3px solid var(--claude-coral);
  padding: 20px 12px 0 0;
}
.cause-arrow {
  align-self: center;
  color: var(--claude-coral);
  font-size: 28px;
}
.practice-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 18px 48px;
  margin-top: 28px;
}
.practice-item {
  display: grid;
  grid-template-columns: 52px 1fr;
  gap: 16px;
  border-top: 1px solid var(--claude-hairline);
  padding-top: 18px;
}
.practice-number {
  color: var(--claude-coral);
  font-family: var(--claude-display);
  font-size: 38px;
  line-height: 1;
}
.practice-item h3 {
  margin-bottom: 6px;
  font-size: 25px;
}
.practice-item p {
  color: var(--claude-muted);
  font-size: 17px;
}
.session-table {
  margin-top: 24px;
}
.session-table th:first-child,
.session-table td:first-child {
  width: 140px;
}
.session-table td {
  vertical-align: top;
}
.handoff {
  display: grid;
  grid-template-columns: 0.9fr 1.1fr;
  gap: 48px;
  margin-top: 24px;
}
.prompt {
  background: var(--claude-dark);
  padding: 24px 26px;
  color: var(--claude-on-dark);
  font-family: var(--claude-mono);
  font-size: 18px;
  line-height: 1.6;
}
.handoff ol {
  margin-top: 0;
}
.comparison {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 44px;
  margin-top: 24px;
}
.comparison > div {
  border-top: 3px solid var(--claude-hairline);
  padding-top: 18px;
}
.comparison > div:last-child {
  border-color: var(--claude-coral);
}
.comparison blockquote {
  margin-top: 18px;
  color: var(--claude-body);
  font-size: 20px;
  line-height: 1.55;
}
.reference-cycle {
  display: grid;
  grid-template-columns: 1fr 38px 1fr 38px 1fr;
  gap: 12px;
  align-items: start;
  margin-top: 42px;
}
.reference-step {
  border-top: 2px solid var(--claude-coral);
  padding-top: 18px;
}
.reference-arrow {
  padding-top: 30px;
  color: var(--claude-coral);
  font-size: 26px;
}
.plan-table {
  margin-top: 20px;
}
.plan-table th:first-child,
.plan-table td:first-child {
  width: 210px;
}
.review-flow {
  display: grid;
  grid-template-columns: 1fr 38px 1fr 38px 1fr;
  gap: 12px;
  align-items: start;
  margin-top: 38px;
}
.review-step {
  border-top: 2px solid var(--claude-coral);
  padding-top: 18px;
}
.review-arrow {
  padding-top: 34px;
  color: var(--claude-coral);
  font-size: 26px;
}
.chat-log {
  display: grid;
  grid-template-columns: 110px 1fr;
  gap: 8px 16px;
  margin-top: 18px;
  font-size: 17px;
}
.speaker {
  color: var(--claude-muted);
  font-weight: 600;
}
.message {
  border-bottom: 1px solid var(--claude-hairline);
  padding-bottom: 8px;
}
.bad-history {
  margin-top: 18px;
  border-left: 4px solid var(--claude-coral);
  padding-left: 20px;
}
.rewind-layout {
  display: grid;
  grid-template-columns: 0.8fr 1.2fr;
  gap: 48px;
  margin-top: 24px;
}
.rewind-command {
  border-top: 3px solid var(--claude-coral);
  padding-top: 18px;
}
.rewind-command code {
  font-size: 28px;
}
.revised-prompt {
  background: var(--claude-soft);
  padding: 22px 24px;
  font-size: 18px;
  line-height: 1.55;
}
.decision-table {
  margin-top: 18px;
}
.decision-table th:first-child,
.decision-table td:first-child {
  width: 31%;
}
.summary-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 18px 46px;
  margin-top: 24px;
}
.summary-point {
  border-top: 2px solid var(--claude-coral);
  padding-top: 16px;
}
.summary-point h3 {
  margin-bottom: 6px;
}
.summary-point p {
  color: var(--claude-muted);
  font-size: 17px;
}
.sources {
  display: grid;
  gap: 10px;
  margin-top: 16px;
  font-size: 15px;
}
.source {
  display: grid;
  grid-template-columns: 185px 1fr;
  border-top: 1px solid var(--claude-hairline);
  padding-top: 10px;
}
.small {
  color: var(--claude-muted);
  font-size: 14px;
}
</style>

<!-- _class: title -->

# Claude Codeの長い会話で<br>指示が外れやすくなる理由と対策

## コンテキストウィンドウの仕組みと、日常運用で使える4つの対策

---

## この資料で説明すること

<div class="doc-intro">
  <div>

### 対象となる状況

- 同じ条件を何度も伝えている
- 修正するほど意図から離れる
- 会話後半で最初の指示が反映されない

  </div>
  <div>

### 読み進める順番

1. 会話履歴がどのように送られるか
2. 長い入力で精度が落ちるとはどういうことか
3. リセット・模範解答・計画・巻き戻しの使い分け

  </div>
</div>

<div class="plain-note" style="margin-top: 28px;">

結論は、Claude Codeに渡す情報の**量を減らし、必要な情報を残す**ことです。

</div>

---

<!-- _class: section -->

<div class="chapter-no">第1章</div>

# 長い会話で何が起きているのか

## まず、会話のたびにモデルへ送られる内容を確認します。

---

<div class="topic">会話履歴の仕組み</div>

## モデルに送るのは「今回の指示」だけではない

<div class="conversation-table">
  <div class="conversation-row">
    <div class="turn-label">1回目</div>
    <div class="history-line"><span class="past">System</span> + <span class="current">Q1</span> → A1</div>
  </div>
  <div class="conversation-row">
    <div class="turn-label">2回目</div>
    <div class="history-line"><span class="past">System + Q1 + A1</span> + <span class="current">Q2</span> → A2</div>
  </div>
  <div class="conversation-row">
    <div class="turn-label">3回目</div>
    <div class="history-line"><span class="past">System + Q1 + A1 + Q2 + A2</span> + <span class="current">Q3</span> → A3</div>
  </div>
  <div class="conversation-row">
    <div class="turn-label">4回目</div>
    <div class="history-line"><span class="past">System + Q1 + A1 + Q2 + A2 + Q3 + A3</span> + <span class="current">Q4</span> → A4</div>
  </div>
</div>

<p class="small" style="margin-top: 24px;">灰色が過去の履歴、コーラルが今回追加した指示です。</p>

---

<div class="topic">会話履歴の増え方</div>

## 過去の質問と回答が、毎回そのまま積み上がる

<div class="history-stack">
  <div>
    <div class="stack-column">
      <div class="stack-block system">System</div>
      <div class="stack-block current">Q1</div>
    </div>
    <div class="stack-caption">1回目</div>
  </div>
  <div>
    <div class="stack-column">
      <div class="stack-block system">System</div>
      <div class="stack-block">Q1</div>
      <div class="stack-block">A1</div>
      <div class="stack-block current">Q2</div>
    </div>
    <div class="stack-caption">2回目</div>
  </div>
  <div>
    <div class="stack-column">
      <div class="stack-block system">System</div>
      <div class="stack-block">Q1</div>
      <div class="stack-block">A1</div>
      <div class="stack-block">Q2</div>
      <div class="stack-block">A2</div>
      <div class="stack-block current">Q3</div>
    </div>
    <div class="stack-caption">3回目</div>
  </div>
  <div>
    <div class="stack-column">
      <div class="stack-block system">System</div>
      <div class="stack-block">Q1</div>
      <div class="stack-block">A1</div>
      <div class="stack-block">Q2</div>
      <div class="stack-block">A2</div>
      <div class="stack-block">Q3</div>
      <div class="stack-block">A3</div>
      <div class="stack-block current">Q4</div>
    </div>
    <div class="stack-caption">4回目</div>
  </div>
</div>

---

<div class="topic">コンテキストウィンドウ</div>

## 1回の処理で参照できる情報量には上限がある

<div class="window-layout">
  <div>
    <div class="window-frame">
      <div class="window-bar">
        <div class="window-history">システム指示・過去の質問・過去の回答</div>
        <div class="window-current">今回の<br>指示</div>
      </div>
    </div>
    <div class="window-limit">固定されたコンテキストウィンドウ</div>
  </div>
  <div>

### 会話が長くなると

- 過去の履歴が占める量が増える
- 毎回送信する入力トークンが増える
- コンテキストウィンドウの残りが少なくなる

  </div>
</div>

<div class="plain-note" style="margin-top: 34px;">

コンテキストウィンドウは無限ではありません。

</div>

---

<div class="topic">長い入力が精度に与える影響</div>

## 入力トークンが増えるほど、LLMの精度は落ちる

<div class="definition">
  <div class="term">
    <h3>Chromaの研究</h3>
    <p>入力トークンが増えるほど、LLMの精度が大きく落ちることが示されています。</p>
  </div>
  <div class="definition-body">

### Claude Codeの会話との関係

会話が進むほど、過去の履歴を含む入力トークンが増えていきます。  
そのため、会話を続ければ続けるほど賢くなるとは限りません。

<p class="small">参考: Chroma, “Context Rot: How Increasing Input Tokens Impacts LLM Performance”</p>

  </div>
</div>

---

<div class="topic">ここまでの整理</div>

## 最初の指示が反映されにくくなるまでの流れ

<div class="cause-chain">
  <div class="cause-item">
    <h3>会話が続く</h3>
    過去の質問と回答も、毎回モデルへ送られる
  </div>
  <div class="cause-arrow">→</div>
  <div class="cause-item">
    <h3>入力トークンが増える</h3>
    会話が進むほど、送信するトークン量が増える
  </div>
  <div class="cause-arrow">→</div>
  <div class="cause-item">
    <h3>精度が落ちる</h3>
    最初の指示を見失い、意図と異なる結果が出る
  </div>
</div>

<div class="plain-note" style="margin-top: 38px;">

対策は、会話の履歴を増やし続けるのではなく、作業に必要な情報だけを渡し直すことです。

</div>

---

<!-- _class: section -->

<div class="chapter-no">第2章</div>

# コンテキストを整理する4つの対策

## リセット・模範解答・計画・巻き戻しを、状況に応じて使い分けます。

---

<div class="topic">4つの対策</div>

## 4つの対策と役割

<div class="practice-list">
  <div class="practice-item">
    <div class="practice-number">1</div>
    <div><h3>リセット</h3><p>会話を短く区切り、コンテキストの圧迫を避ける。</p></div>
  </div>
  <div class="practice-item">
    <div class="practice-number">2</div>
    <div><h3>模範解答</h3><p>長い指示ではなく、求める出力の例を渡す。</p></div>
  </div>
  <div class="practice-item">
    <div class="practice-number">3</div>
    <div><h3>計画</h3><p>実装を始める前に、依頼内容と進め方を整理する。</p></div>
  </div>
  <div class="practice-item">
    <div class="practice-number">4</div>
    <div><h3>巻き戻し</h3><p>違和感が出た回答を残さず、指示を出し直す。</p></div>
  </div>
</div>

---

<div class="topic">対策1　リセット</div>

## 機能全体を1会話でやり切らず、タスクを分ける

<table class="session-table">
  <thead>
    <tr><th>進め方</th><th>作業の単位</th><th>会話履歴</th></tr>
  </thead>
  <tbody>
    <tr><td>1会話で進める</td><td>機能全体をまとめて実装する</td><td>作業が進むほど長くなる</td></tr>
    <tr><td>会話を分ける</td><td>タスクを分解し、別セッションで実装する</td><td>セッションごとに短く保てる</td></tr>
  </tbody>
</table>

<div class="plain-note" style="margin-top: 28px;">

精度が落ちる主因がコンテキスト圧迫なら、こまめに <code>/clear</code> するのがシンプルな対策です。

</div>

---

<div class="topic">対策1　リセット前の引継ぎ</div>

## 次の会話に必要な情報だけを短く残す

<div class="handoff">
  <div>
    <div class="prompt">次のセッションの指示を簡潔に考えて、pbcopyしてクリップボードに入れてください。</div>
  </div>
  <div>
    <ol>
      <li>Claude Codeに次セッション用の指示を作らせる</li>
      <li><code>/clear</code> で会話履歴を消す</li>
      <li>クリップボードの指示を貼り付けて再開する</li>
    </ol>
  </div>
</div>

<p class="small" style="margin-top: 26px;">引継ぎ用に長大なファイルを作らなくても、短いプロンプトで続きから再開できます。</p>

---

<div class="topic">対策2　模範解答</div>

## 曖昧な程度を説明するより、期待する例を示す

<div class="comparison">
  <div>
    <h3>ルールを並べる</h3>
    <blockquote>箇条書きで、敬語で、堅苦しくなりすぎず、部署名は正式名称で、個人名は書きすぎないで……</blockquote>
    <p class="small">「どの程度か」「何を優先するか」が残る。</p>
  </div>
  <div>
    <h3>既存の良い例を示す</h3>
    <blockquote>過去に作成した <code>meeting-2026-05-01.md</code> と同じトーンで議事録を作成して。</blockquote>
    <p class="small">文体・構成・語彙を、1つの参照先からまとめて読み取れる。</p>
  </div>
</div>

---

<div class="topic">対策2　模範解答の作り方</div>

## 良い例がなければ、Claude Codeと一緒に育てる

<div class="reference-cycle">
  <div class="reference-step">
    <h3>1. 出力を作る</h3>
    手元にある指示をもとに、まず成果物を作る。
  </div>
  <div class="reference-arrow">→</div>
  <div class="reference-step">
    <h3>2. やり取りして直す</h3>
    満足できる出力になるまで修正する。
  </div>
  <div class="reference-arrow">→</div>
  <div class="reference-step">
    <h3>3. 参照先として残す</h3>
    次回はルールを再説明せず、その成果物を示す。
  </div>
</div>

<div class="plain-note" style="margin-top: 42px;">

模範解答が完成したら、長くなった作成過程の会話はリセットできます。

</div>

---

<div class="topic">対策3　計画</div>

## 実装前に計画を立てるか、依頼内容を整理する

<div class="comparison">
  <div>
    <h3>依頼内容が整理できている</h3>
    <p><code>/plan</code> でプランモードに切り替え、実装計画を先に立てさせる。</p>
  </div>
  <div>
    <h3>自分の考えが整理できていない</h3>
    <p>インタビュー型のスキル <code>grill-me</code> を使い、依頼内容を先に明確にする。</p>
  </div>
</div>

<div class="plain-note" style="margin-top: 28px;">依頼をよしなに解釈して実装を始めさせる前に、文章の段階で認識を合わせます。</div>

---

<div class="topic">対策3　計画のレビュー</div>

## 別の視点で、実装前に漏れを見つける

<div class="review-flow">
  <div class="review-step">
    <h3>計画を作る</h3>
    Claude Codeに実装計画を作らせる。
  </div>
  <div class="review-arrow">→</div>
  <div class="review-step">
    <h3>サブエージェントが確認</h3>
    シニアエンジニアの視点でプランをレビューする。
  </div>
  <div class="review-arrow">→</div>
  <div class="review-step">
    <h3>人が判断する</h3>
    妥当な指摘だけ反映し、迷う論点は質問させる。
  </div>
</div>

<div class="plain-note" style="margin-top: 42px;">

プランは作っただけでは観点が漏れることがあるため、別の視点を通します。

</div>

---

<div class="topic">対策4　巻き戻し</div>

## 修正を重ねると、誤った方向の履歴も残り続ける

<div class="chat-log">
  <div class="speaker">私</div><div class="message">議事録を作成して。</div>
  <div class="speaker">Claude Code</div><div class="message">作成しました。</div>
  <div class="speaker">私</div><div class="message">もっとフレンドリーにして。</div>
  <div class="speaker">Claude Code</div><div class="message">かなりカジュアルな口調に修正しました。</div>
  <div class="speaker">私</div><div class="message">相手はお客様なので、そこまで崩さないで。</div>
</div>

<div class="bad-history">

この時点では、曖昧な最初の指示、期待と違う出力、程度の曖昧な修正が、すべて履歴に含まれています。

</div>

---

<div class="topic">対策4　巻き戻して指示し直す</div>

## 違和感が出た分岐点まで戻り、条件をまとめ直す

<div class="rewind-layout">
  <div class="rewind-command">
    <h3>戻り方</h3>
    <p><code>Esc × 2</code></p>
    <p>または <code>/rewind</code></p>
    <p class="small">「議事録を作成して」と依頼した時点まで戻る。</p>
  </div>
  <div>
    <div class="revised-prompt">
      相手はお客様です。敬語をベースに、堅苦しくなりすぎない口調で議事録を作成してください。文体は <code>meeting-2026-05-01.md</code> を参考にしてください。
    </div>
    <p class="small" style="margin-top: 16px;">重要な条件と模範解答を、最初の依頼にまとめて入れ直します。</p>
  </div>
</div>

---

<!-- _class: section -->

<div class="chapter-no">第3章</div>

# 日常運用での使い分け

## どのタイミングで、どの対策を使うかを整理します。

---

<div class="topic">使い分け</div>

## 状況に応じて選ぶコマンドと行動

<table class="decision-table">
  <thead>
    <tr><th>状況</th><th>行動</th><th>減らしたいもの</th></tr>
  </thead>
  <tbody>
    <tr><td>実装を始める前</td><td><code>/plan</code> で実装計画を先に立てる</td><td>実装後の手戻り</td></tr>
    <tr><td>出力の程度を説明しにくい</td><td>既存の良い成果物を参照させる</td><td>曖昧なルール</td></tr>
    <tr><td>回答に違和感が出た</td><td><code>/rewind</code> で分岐点まで戻る</td><td>誤った方向の履歴</td></tr>
    <tr><td>作業が一区切りついた</td><td>必要なら引継ぎを作り <code>/clear</code></td><td>完了した作業の履歴</td></tr>
  </tbody>
</table>

---

## まとめ

<div class="summary-grid">
  <div class="summary-point">
    <h3>長い会話は履歴が積み上がる</h3>
    <p>毎回、システム指示と過去の質問・回答もモデルへ送られる。</p>
  </div>
  <div class="summary-point">
    <h3>入力トークンが増えると精度が落ちる</h3>
    <p>会話を続ければ続けるほど賢くなるとは限らない。</p>
  </div>
  <div class="summary-point">
    <h3>不要な履歴を残さない</h3>
    <p>作業を分けてリセットし、誤った分岐は巻き戻す。</p>
  </div>
  <div class="summary-point">
    <h3>必要な情報を明確にする</h3>
    <p>模範解答と計画を使い、曖昧な説明や手戻りを減らす。</p>
  </div>
</div>

<div class="plain-note" style="margin-top: 26px;">

4つの対策はすべて、Claude Codeに渡す情報を「今回の作業に必要なもの」に整理するための方法です。

</div>

---

## 参考資料

<div class="sources">
  <div class="source"><strong>Context Rot</strong><span>arxiv.org/abs/2601.15300</span></div>
  <div class="source"><strong>Grill me</strong><span>azukiazusa.dev/blog/before-implementation-interview-design-requirements-grill-me/</span></div>
  <div class="source"><strong>Grill with docs</strong><span>github.com/mattpocock/skills/tree/main/skills/engineering/grill-with-docs</span></div>
  <div class="source"><strong>Claude prompting</strong><span>platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices</span></div>
  <div class="source"><strong>関連記事</strong><span>zenn.dev/aun_phonogram/articles/4be2f4745726fb</span></div>
  <div class="source"><strong>関連記事</strong><span>ma-ji.ai/articles/933e1d8c-4df6-4e19-9d9d-90b398ce0686</span></div>
</div>

<p class="small" style="margin-top: 22px;">本資料は上記資料と、日常的なClaude Code運用の実践をもとに再構成しています。</p>
