---
marp: true
theme: claude
paginate: true
size: 16:9
---

<style>
/* 画像は後から差し替えるため、確定するまで枠と撮影内容だけを置く */
.shot {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
  min-height: 400px;
  border: 1px dashed var(--claude-muted-soft);
  padding: 24px;
  text-align: center;
}

.shot-label {
  color: var(--claude-coral);
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}

.shot-note {
  max-width: 82%;
  color: var(--claude-muted);
  font-size: 15px;
  line-height: 1.6;
}

/* 送信量の増加は面ではなく線の長さで示す */
.turns {
  margin-top: var(--claude-space-2);
}

.turn {
  display: grid;
  grid-template-columns: 96px 1fr 210px;
  align-items: center;
  gap: 24px;
  border-top: 1px solid var(--claude-hairline);
  padding: 26px 0;
}

/* 同じ長さのバーを並べて比べるため、ラベル列を広く取る */
.turns.compare .turn {
  grid-template-columns: 160px 1fr 300px;
}

.turn-no {
  color: var(--claude-muted);
  font-size: var(--claude-fs-s);
}

.turn-bar {
  display: flex;
  align-items: center;
  gap: 4px;
}

.turn-bar i {
  display: block;
  height: 10px;
}

/* Marpit が style 属性を落とすため、履歴の長さはクラスで持たせる */
.turn-bar .past {
  background: #ddd3c1;
}

.turn-bar .past.w1 {
  width: 130px;
}

.turn-bar .past.w2 {
  width: 340px;
}

.turn-bar .past.w3 {
  width: 550px;
}

.turn-bar .now {
  width: 130px;
  background: var(--claude-coral);
}

.turn-note {
  color: var(--claude-body);
  font-size: var(--claude-fs-xs);
}

.legend {
  display: flex;
  gap: var(--claude-space-3);
  margin-top: var(--claude-space-2);
  color: var(--claude-muted);
  font-size: 15px;
}

.legend span {
  display: flex;
  align-items: center;
  gap: 8px;
}

.legend i {
  display: block;
  width: 28px;
  height: 10px;
}

.legend .past {
  background: #ddd3c1;
}

.legend .now {
  background: var(--claude-coral);
}

.bar {
  height: 10px;
  background: #ddd3c1;
}

.bars {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: var(--claude-space-3) 0 12px;
}

.bars .bar {
  flex: 1;
}

/* 会話の切れ目は短い縦バーで示す */
.cut {
  display: block;
  width: 2px;
  height: 28px;
  background: var(--claude-coral);
}

.bar-caption {
  display: flex;
  gap: 12px;
  margin-bottom: var(--claude-space-3);
  color: var(--claude-muted);
  font-size: 15px;
}

.bar-caption span {
  flex: 1;
  text-align: center;
}

.flow-note {
  color: var(--claude-muted);
  font-size: 15px;
}

/* グリッドのレイアウトでは補足が 1 列目に落ちるため、全幅に戻す */
section.content-2col-comparison > .caption,
section.content-3col > .caption,
section.content-steps > .caption {
  grid-column: 1 / -1;
  margin-top: var(--claude-space-3);
}
</style>

<!-- _class: title -->

# Claude Code を思ったとおりに動かす

---

<!-- _class: content-center -->

## 「さっき言ったはずのことが、守られない」

### 原因はモデルの性能ではなく、会話の長さにあります。

長く話すほど過去のやり取りが増え、本当に必要な指示が埋もれます。

---

<!-- _class: agenda -->

## この資料で扱うこと

1. なぜ指示が埋もれるのか
2. 依頼の書き方を変える
3. 会話を区切る
4. ずれたら戻す
5. 渡す情報量を整える

---

<!-- _class: section -->

# 1. なぜ指示が埋もれるのか

## 会話履歴は毎回まとめてモデルへ送られる

---

## 会話が進むほど、毎回送る量が増える

<div class="turns">

<div class="turn">
<span class="turn-no">1 回目</span>
<span class="turn-bar"><i class="now"></i></span>
<span class="turn-note">指示だけを送る</span>
</div>

<div class="turn">
<span class="turn-no">2 回目</span>
<span class="turn-bar"><i class="past w1"></i><i class="now"></i></span>
<span class="turn-note">1 回分のやり取りを添えて送る</span>
</div>

<div class="turn">
<span class="turn-no">3 回目</span>
<span class="turn-bar"><i class="past w2"></i><i class="now"></i></span>
<span class="turn-note">2 回分のやり取りを添えて送る</span>
</div>

<div class="turn">
<span class="turn-no">4 回目</span>
<span class="turn-bar"><i class="past w3"></i><i class="now"></i></span>
<span class="turn-note">3 回分のやり取りを添えて送る</span>
</div>

</div>

<div class="legend">
<span><i class="now"></i>今回の指示</span>
<span><i class="past"></i>これまでの会話履歴とシステムプロンプト</span>
</div>

---

## キャッシュがあっても、送る履歴の長さは変わらない

<div class="turns compare">

<div class="turn">
<span class="turn-no">キャッシュあり</span>
<span class="turn-bar"><i class="past w2"></i><i class="now"></i></span>
<span class="turn-note">変わっていない部分の処理を再利用する。応答は速い</span>
</div>

<div class="turn">
<span class="turn-no">キャッシュなし</span>
<span class="turn-bar"><i class="past w2"></i><i class="now"></i></span>
<span class="turn-note">履歴を最初から処理し直す。応答は遅い</span>
</div>

</div>

<div class="box-medium">

どちらもモデルへ送る長さは同じです。キャッシュは処理を速くする仕組みで、コンテキストの圧迫を防ぐものではありません。

</div>

---

<!-- _class: content-3col -->

## キャッシュは、この 3 つで失われる

<div>

### 時間が空いた

メインの会話は 1 時間、それ以外のリクエストは 5 分で期限切れ。

**→ 次のリクエストで履歴を再処理する**

</div>

<div>

### モデルを切り替えた

会話の途中で別のモデルに変えた。

**→ その会話のキャッシュは引き継がれない**

</div>

<div>

### effort を変えた

会話の途中で effort を変えた。

**→ 多くのモデルでキャッシュが無効になる**

</div>

---

<!-- _class: callout -->

## コンテキストは、自分で管理するもの

タスクの最初にモデルと effort を決め、必要がなければ途中で変えない。そのうえで、以降の 4 つの使い方でコンテキストを短く保ちます。

---

<!-- _class: section -->

# 2. 依頼の書き方を変える

## 解釈を任せきりにしない

---

<!-- _class: content-2col-comparison -->

## 5 項目を書くと、どこまでやるかが依頼の時点で決まる

<div>

### 曖昧なまま頼む

```text
ログイン周りのバリデーション
直しておいて
```

どこまで直すかの判断を任せることになり、API 側まで手が入る。

</div>

<div>

### 目的と完了条件を書く

```text
# 目的
入力チェックを仕様どおりにする
# 範囲
src/features/login/ のみ
# 進め方
方針を出してから実装
# 制約
既存テストを壊さない
# 完了条件
pnpm test が通り、空白だけの入力を弾く
```

</div>

<p class="caption">項目は「目的 / 範囲 / 進め方 / 制約 / 完了条件」。簡単な質問に毎回すべて書く必要はなく、実装範囲や成功条件が曖昧な作業で使います。</p>

---

<!-- _class: content-dark-code -->

## 依頼が固まっていないなら、この定型文で質問させる

<div>

### 返ってくるもの

前提の抜けが質問の形で戻ってきます。

- 対象は既存画面の修正か、新規作成か
- 認証は既存の仕組みを使うか (推奨: 使う。理由は…)

数字で答えるだけで依頼が固まります。

</div>

<div>

<div class="status-line">● 依頼が固まっていないときの定型文</div>

```text
良いアウトプットを出すために、
私に聞きたいことを厳選して質問してください。
質問は推奨案と理由も提示。
選択肢は数字で提示。
```

</div>

---

<!-- _class: content-2col-comparison -->

## ルールを並べるより、模範解答を 1 つ渡す

<div>

### 指示を詰め込む

「箇条書きで、敬語で、堅苦しくなりすぎず、部署名は正式名称で、個人名は書きすぎないで議事録を作成して」

</div>

<div>

### 例を示す

「過去に作成した `meeting-2026-05-01.md` と同じトーンで議事録を作成して」

</div>

---

<!-- _class: content-steps -->

## 模範解答が手元にないときは、1 回作って保存する

<div class="steps-container">

<div class="step-item">
<div class="step-number">01</div>
<div class="step-content">

### 対話しながら作る

満足できる出力になるまで、その場で直しながら 1 つ仕上げる。

</div>
</div>

<div class="step-item">
<div class="step-number">02</div>
<div class="step-content">

### ファイルとして残す

`meeting-2026-05-01.md` や `UserService.ts` のように、次回から指せる場所に置く。

</div>
</div>

<div class="step-item">
<div class="step-number">03</div>
<div class="step-content">

### 次回はそれを指す

「`UserService.ts` を参考に同じスタイルで実装して」と書くだけで済む。

</div>
</div>

</div>

<p class="caption">模範解答を育てる過程でも会話は長くなります。できあがったら忘れずにリセットします。</p>

---

<!-- _class: section -->

# 3. 会話を区切る

## 決まったことは、チャットではなくファイルに置く

---

<!-- _class: content-2col-comparison -->

## 1 本の長い会話を、タスク単位に分ける

<div>

### 区切らない

<div class="bars">
<div class="bar"></div>
</div>

<div class="bar-caption"><span>最初から最後まで 1 本の会話</span></div>

<p class="flow-note">決定事項がチャットの中だけにあり、最初の指示が後半のやり取りに埋もれる。</p>

</div>

<div>

### 区切る

<div class="bars">
<div class="bar"></div><span class="cut"></span><div class="bar"></div><span class="cut"></span><div class="bar"></div>
</div>

<div class="bar-caption"><span>タスク 1</span><span>タスク 2</span><span>タスク 3</span></div>

<p class="flow-note">縦線のところで決定事項をファイルへ残して <code>/clear</code>。次の会話は短い状態から始まる。</p>

</div>

---

<!-- _class: content-steps -->

## 決定事項をファイルに残してから `/clear` する

<div class="steps-container">

<div class="step-item">
<div class="step-number">01</div>
<div class="step-content">

### 決定事項を残す

固まった設計や要件は GitHub Issue かローカルの Markdown に書き出す。

</div>
</div>

<div class="step-item">
<div class="step-number">02</div>
<div class="step-content">

### 引き継ぎを作らせる

次のセッション用の指示を Claude Code 自身に考えさせ、クリップボードへ入れる。

</div>
</div>

<div class="step-item">
<div class="step-number">03</div>
<div class="step-content">

### `/clear` で始め直す

新しい会話に貼り付ければ、関係のない履歴を持ち越さずに続きから再開できる。

</div>
</div>

</div>

---

<!-- _class: content-dark-code -->

## 引き継ぎに長い資料はいらない

<div>

### リセットの直前に 1 行

クリップボードに次のセッション用のプロンプトが残るので、`/clear` の直後に貼り付けるだけで再開できます。あとから見返したい決定事項は、Issue や Markdown に残します。

</div>

<div>

<div class="status-line">● macOS の場合</div>

```text
次のセッションの指示を簡潔に考えて、pbcopy してクリップボードに入れてください。
```

</div>

---

## 基本は `/clear`、文脈を保ちたいときだけ `/compact`

| 状況 | 使うもの | 結果 |
| --- | --- | --- |
| 決定事項をファイルに残せた | `/clear` | 履歴を捨てて、短い状態から始める |
| 同じ作業を文脈ごと続けたい | `/compact` | 履歴を要約して続ける |

<p class="caption"><code>/compact</code> は要約で詳細が抜けることがあります。長い休止のあとに実行すると、要約のために履歴を再処理する場合もあります。要件や設計の保存先を先に用意しておけば、ほとんどの場面は <code>/clear</code> で足ります。</p>

---

<!-- _class: section -->

# 4. ずれたら戻す

## 追加の指示を重ねるほど、履歴だけが増える

---

<!-- _class: content-image-right -->

## 実装の前に、計画を立てさせる

<div>

Claude Code は依頼を解釈してすぐ実装を始める傾向があります。結果が想定と違えば、トークンと時間を失います。

- `Shift+Tab` で Plan mode を選ぶ
- 1 回の依頼に `/plan` を付ける
- 要件が曖昧なときは `/grill-me` でインタビューさせる

</div>

<div>

<div class="shot">
<div class="shot-label">画像 TODO</div>
<div class="shot-note">Plan mode で実装計画が提示され、承認待ちになっている画面。<code>Shift+Tab</code> のモード表示が見えるように撮る。</div>
</div>

</div>

---

<!-- _class: content-list-panel -->

## 立てた計画は、そのまま採用しない

<div>

### 別の視点を 1 回通す

計画は立てただけでは観点が漏れていることがあります。作成した計画ファイルをサブエージェントにレビューさせると、実装前に抜けを拾えます。

</div>

<div class="panel">

### 使っている指示

プランファイルはシニアエンジニアのサブエージェントにレビューさせ、妥当な指摘だけ修正し、判断に悩むものは必ず私に質問する。

</div>

---

<!-- _class: content-2col-comparison -->

## 意図と違ったら、指示を重ねずに巻き戻す

<div>

### 指示を重ねるといつまでも食い違う…

```text
私「議事録作成して」
🤖「はい、作成しました」
私「これ、敬語が丁寧口調すぎるな…もうちょいフレンドリーにして。」
🤖「はい、フレンドリーにしました(もはやタメ口)」
私「え？フレンドリーすぎん？相手はお客様で取引相手だよ？」
私「なんか思ってたんと違うなぁ….」
```

</div>

<div>

### 過去に戻ってなかったことにする！

```text
私「議事録作成して」
🤖「はい、作成しました」

// このタイミングでエスケープキー 2 回(または `/rewind`)で
　　最初の指示まで戻す

私「議事録作成して。相手はお客様で取引相手です。
　　敬語がベースだけど、堅苦しくなりすぎず、過去に作成した
　　`meeting-2026-05-01.md` くらいの少しだけフレンドリーな
　　口調で作成して。」
🤖「はい、作成しました」
```

</div>

---

<!-- _class: content-image-right -->

## エスケープキー 2 回で戻す

<div>

エスケープキーを 2 回押す、または `/rewind` で過去の会話に戻れます。

意図と異なる回答が返ってきたときは、追加で説明を重ねる前に最初の指示まで戻し、要件を整理して依頼し直します。ここでも模範解答をあわせて渡すと伝わりやすくなります。

</div>

<div>

![image](https://pub-151065dba8464e6982571edb9ce95445.r2.dev/images/c4bab60837c4dcdd0f3a378149b5ae57.png)

</div>

---

<!-- _class: section -->

# 5. 渡す情報量を整える

## 毎回の会話に載せるものを、必要な分だけにする

---

## タスクに合わせてモデルを選ぶ

| 場面 | 選ぶモデル |
| --- | --- |
| 関連ファイルの確認、短い要約 | 軽いモデル |
| 調査、簡単な実装 | Sonnet |
| 設計、要件定義、少し複雑な実装 | Opus / Fable |

<p class="caption">サブエージェントも同じです。モデル指定のないサブエージェントは、メインの会話で使っているモデルを引き継ぎます。簡易タスクと判断された場合のみ、公式の Explorer サブエージェント (Haiku) が使われます。</p>

---

<!-- _class: content-dark-code -->

## 調査用のサブエージェントは軽いモデルで動かす

<div>

### 定義ファイルに `model` を書く

`.claude/agents/` に置いたファイルでモデルを固定できます。利用可能なモデル名は随時更新されるため、設定時に最新の情報を確認します。

</div>

<div>

<div class="status-line">● .claude/agents/quick-investigator.md</div>

```markdown
---
name: quick-investigator
description: 関連ファイルの調査に使う
tools: Read, Grep, Glob
model: haiku
---

関係するファイルと根拠を挙げ、
調査結果を短く報告してください。
```

</div>

---

<!-- _class: content-2col-comparison -->

## ログは貼る前にエラー周辺だけ取り出す

<div>

### そのまま渡す

```text
ビルドログ全体を会話に貼り付ける
```

関係のない行までコンテキストに入り、必要な情報が埋もれる。

</div>

<div>

### 絞ってから渡す

```bash
rg -n -C 2 "ERROR|WARN" \
  build.log > relevant-lines.log
```

`relevant-lines.log` だけを読ませる。`-C 2` は一致した行の前後 2 行を含める。

</div>

---

<!-- _class: content-3col -->

## 週に 1 回、この 3 つに当てはまるものを無効にする

<div>

### 使っていない

追加したまま一度も呼ばれていない MCP サーバーやスキル。

</div>

<div>

### 役割が重複している

同じことができるものが 2 つ以上ある。

</div>

<div>

### 接続に失敗している

エラーのまま残っている。

</div>

<p class="caption">対応環境では MCP のツール定義は必要になったときに検索されるため、追加のたびに全定義がコンテキストへ入るわけではありません。それでも不要な定義は、意図しない呼び出しの原因になり得ます。</p>

---

<!-- _class: content-2col-comparison -->

## CLAUDE.md には、毎回必要になるものだけ書く

<div>

### 書かない

```text
slides/ にスライドが入っている
themes/company.css がテーマ本体
```

ファイルを 1 つ読めば分かることは、必要になったときに Claude Code 自身が調べられる。

</div>

<div>

### 書く

```text
テストは pnpm test で実行する
色は themes/ の CSS 変数以外に足さない
```

プロジェクト固有のルール、テストの実行方法、コードからは分からない設計上の決定。

</div>

<p class="caption">CLAUDE.md は毎回の会話の開始時に読み込まれます。不要な説明を書くと指示が長くなり、本当に必要な情報が見つけにくくなります。</p>

---

<!-- _class: content-dark-code -->

## 自動メモリはオフにする

<div>

### 終わった作業の前提を持ち越さない

自動メモリは初期状態で有効です。些細な情報が保存されたり、終わった作業の前提が別の会話に入り込んだりするのを防ぐため、オフにしています。

環境変数を使う場合は `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1` で有効化できます。

</div>

<div>

<div class="status-line">● ~/.claude/settings.json</div>

```json
{
  "promptCacheTtl": "1h",
  "subagentPromptCacheTtl": "1h",
  "autoMemoryEnabled": false
}
```

</div>

---

<!-- _class: content-image-right -->

## 仕様は公式ドキュメントのチャットに聞く

<div>

Claude Code の公式ドキュメントには、機能や制限をチャットで質問できる仕組みがあります。
回答と一緒に該当ページへのリンクが表示されるため、仕様を覚えておく必要はありません。

設定を変更する前に、まずここで確認します。

https://code.claude.com/docs/en/overview


</div>

![image](https://pub-151065dba8464e6982571edb9ce95445.r2.dev/images/ed0cae84fb43e66131583d4156f6c4fd.png)

</div>

---

<!-- _class: summary -->

## まとめ: 会話の長さを管理する

<div class="summary-items">

<div class="summary-item">

### 履歴は増え続ける

キャッシュは処理を効率化するだけで、コンテキストの圧迫は防げない。

</div>

<div class="summary-item">

### 区切って捨てる

決定事項をファイルに残して `/clear`。次の指示はクリップボードに用意する。

</div>

<div class="summary-item">

### 情報量を整える

プロンプト、モデル、ツール結果、CLAUDE.md、MCP、スキルを必要な分だけにする。

</div>

</div>

---

<!-- _class: summary -->

## まとめ: 出力のずれを減らす

<div class="summary-items">

<div class="summary-item">

### 模範解答を渡す

ルールを羅列するより、良い例を 1 つ示すほうが意図どおりになる。

</div>

<div class="summary-item">

### 先に計画を立てる

実装前に計画させ、考慮不足を防ぐためにサブエージェントにレビューさせる。

</div>

<div class="summary-item">

### ずれたら戻す

追加の指示を重ねず、`/rewind` で過去の指示に戻して直す。

</div>

</div>

---

## 参考

- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- [How Claude Code uses prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Create custom subagents](https://code.claude.com/docs/en/sub-agents)
- [Connect Claude Code to tools via MCP](https://code.claude.com/docs/en/mcp)
- [How Claude remembers your project](https://code.claude.com/docs/en/memory)
- [今更だけどよくやる Claude Code 活用法](https://suntory-n-water.com/blog/claude-code-context-management-tips)

<p class="caption">仕様は 2026 年 9 月 14 日時点の公式ドキュメントに基づきます。キャッシュ TTL の設定には Claude Code v2.1.242 以降が必要です。</p>
